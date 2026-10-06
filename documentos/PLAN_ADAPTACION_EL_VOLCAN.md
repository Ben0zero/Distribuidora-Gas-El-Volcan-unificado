# Plan de Adaptación — MiniMarket → Distribuidora de Gas El Volcán

Reutilizar el template de microservicios "MiniMarket" (Spring Boot) para el dominio de la distribuidora de gas.
**No partir de cero**: heredar los 6 servicios + Eureka y adaptar entidades, datos semilla y reglas.

---

## 1. Arquitectura objetivo

| # | Servicio | Puerto | Papel en "El Volcán" |
|---|---|---|---|
| 1 | **Eureka** | 8761 | Discovery: los 6 servicios se registran en UP |
| 2 | **ms-auth** (LoginAuth) | 8081 | Autenticación + usuarios + roles (JWT) |
| 3 | **ms-catalogo** | 8082 | Catálogo de productos de gas (cilindros, reguladores, mangueras, accesorios) |
| 4 | **ms-produccion** | 8083 | **Recepción de mercadería** (lote de cilindros que entra y sube stock) |
| 5 | **ms-inventario** | 8084 | Stock por SKU + alerta stock crítico + descontar (evita negativo) |
| 6 | ~~Pesaje~~ | ~~8085~~ | **SE ELIMINA**: el gas se vende por unidad, no por kilo suelto |
| 7 | **ms-ventas** | 8086 | Órdenes: `PENDIENTE_PAGO` → confirmar pago → descuenta inventario |

**Nuevo deployable objetivo:** 5 servicios + Eureka (se descarta Pesaje).

---

## 2. Mapa de mapeo por servicio

### 2.1 ms-auth (LoginAuth)
- **Renombrar rol** `PANADERO` → `VENDEDOR`; mantener `ADMIN`; agregar `CLIENTE`.
- **Registro** (`RegistroRequestDTO`): agregar campos del frontend → `direccion`, `region`, `comuna`, `tipoCliente` (residencial/comercial), `telefono` (opcional).
- **Usuario base** (DataInitializer): `admin@duoc.cl / Admin123`, RUN `19011022`, nombre "Juan Pérez", rol **Administrador** (igual que `ADMIN_DEFAULT` del frontend, `java.js:28`).
- **Endpoints**: `POST /api/auth/login`, `GET /api/v2/usuarios` (HATEOAS) — se mantienen.

### 2.2 ms-catalogo (productos de gas)
- Sustituir semilla de panadería por los **14 productos reales** que ya están en `java.js:4-18` (PRODUCTOS):
  - Cilindros: CL001 (5kg), CL002 (11kg), CL003 (15kg), CL004 (45kg).
  - Reguladores: RG001 (estándar), RG002 (alta presión), RG003 (dual).
  - Mangueras y conexiones: MG001 (1.5m), MG002 (3m), MG003 (abrazadera), MG004 (kit).
  - Accesorios: AC001 (carro), AC002 (tapa válvula), AC003 (detector de gas).
- **DTO `ProductoRequestDTO`**: hoy tiene un solo `precio` → **agregar `precioResidencial` y `precioComercial`** (el frontend cobra según tipo de cliente).
- Al crear producto por admin → Feign avisa a Inventario (se mantiene el flujo actual).

### 2.3 ms-inventario
- **No cambia de lógica**: stock por SKU, `stockCritico`, `disminuir` (rechaza stock negativo con `OverstockException`/`InsufficientStockException`).
- Semilla: stock inicial de cada producto según `java.js:4-18` (ej: CL003 → 90, stockCrítico 5).

### 2.4 ms-produccion → Recepción de mercadería
- Renombrar concepto: "Lote de fabricación de pan" → **"Lote de cilindros recibidos"**.
- `ProduccionRequestDTO`: `productoSku` + `cantidad` (mismo shape; el comentario cambia a "cilindros recibidos").
- Efecto secundario: Feign suma stock en Inventario (igual que hoy).

### 2.5 ms-ventas
- Mantener flujo: **crear venta (PENDIENTE_PAGO)** → **confirmar pago** → descuenta stock vía Inventario.
- Quitar dependencia de `PesajeClient` (ya no existe pesaje); el carrito completo es por unidad (`DetalleVentaRequestDTO { productoId, cantidad }`).
- `VentaRequestDTO`: considerar enviar `tipoCliente` para usar el precio correcto del catálogo.

### 2.6 ~~Pesaje~~ → descartar
No se descarga/compila. No se integra en `ms-ventas`.

---

## 3. Renombrados de código

| Servicio | Paquete actual | Paquete objetivo |
|---|---|---|
| auth | `com.example.LoginAuth` | `cl.elvolcan.auth` |
| catálogo | `com.example.catalogoproductos` | `cl.elvolcan.catalogo` |
| producción | `minimarket.produccion` | `cl.elvolcan.recepcion` |
| inventario | `inventario.inventario_service` | `cl.elvolcan.inventario` |
| ventas | `Minimarket.ventas` | `cl.elvolcan.ventas` |

- `artifactId` en `pom.xml`: `loginauth`, `catalogoproductos`, … → `elvolcan-auth`, `elvolcan-catalogo`, etc.
- Nombres de clases de negocio: `Producto`→se mantiene; `LoteProduccion`→`LoteRecepcion`; `Venta`/`DetalleVenta` se mantienen.

---

## 4. Datos semilla (DataInitializer de cada servicio)

- **auth**: admin + un vendedor de ejemplo (`vendedor@elvolcan.cl`) + 1 cliente.
- **catálogo**: los 14 productos del frontend (SKU, nombre, descripción, precios res/com, categoría, nombre de imagen para el frontend).
- **inventario**: SKU + stock actual + stock crítico por producto.
- **recepcion**: sin datos semilla (se generan lotes al operar).

---

## 5. Configuración (application.properties)

- Tabla puertos: 8761 / 8081 / 8082 / 8083 / 8084 / 8085(eliminado) / 8086.
- Cada servicio registra en Eureka (`eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka`).
- **Base de datos**: el template trae `OracleWallet/` (Oracle Cloud) + MySQL como alternativa. Decidir: **MySQL local** para simplificar las pruebas de defensa, y adaptar las URLs/credenciales en cada `application.properties`.
- Verificar que **Java 21 + Maven (wrapper `mvnw`) + Spring Boot 4.0.6** compilen en el equipo (hoy no se ha usado).

---

## 6. Seguridad (JWT)

- Un token emitido por **ms-auth** vale para los demás servicios (misma `JwtUtil`/secreto compartido — confirmar dónde se define la clave secreta).
- Niveles:
  - `ADMIN` → gestiona usuarios, productos, inventario.
  - `VENDEDOR` → registra recepciones y ventas.
  - `CLIENTE` → solo catálogo + su carrito/órdenes (acotar en `SecurityConfig`).
- `CustomAuthenticationEntryPoint` → 401 controlado si falta token (ya existe).

---

## 7. Integración con el frontend (futuro)

Cuando se migre el sitio web, se aplica el "seam" ya documentado en
`ERS_Plan_Microservicios.md`: cambiar `leer()`/`guardar()` de `java.js` (líneas 38/47)
por `fetch()` a los endpoints de arriba. Los **modelos JSON ya están iguales**
(PRODUCTOS en `java.js:4`, usuarios, órdenes).

---

## 8. Roadmap

### Fase A — Preparar el terreno
- [x] Extraer los 4 ZIPs pendientes (catálogo, producción, inventario, ventas) a carpetas limpias.
- [x] Verificar que **Eureka + cada módulo compilan** (Java 21.0.10 LTS, `./mvnw compile` → EXIT 0 en los 6).
- [ ] Decidir BD (sugerencia: MySQL local).
- [ ] Levantar los 6 servicios y validar que aparezcan UP en Eureka.

**Notas de la Fase A (2026-09-15):**
- Java 21 instalado en el equipo; `mvnw` descargó Maven y todas las dependencias OK.
- Los 6 proyectos compilan: eureka, auth, catálogo, inventario, producción, ventas. EXIT=0.
- `ms-produccion` no traía `.mvn/wrapper` en ambas copias → se copió el wrapper desde `ms-ventas` y compiló.
- Los ZIPs traían **copias duplicadas anidadas** (ej: `ms-inventario/MiniMarket-Proyecto-ms-inventario/...`) → en la limpieza final borrar las copias anidadas y quedarse con el proyecto de la raíz.

### Fase B — Adaptar (un servicio por sesión)
- [ ] **auth**: roles + registro con dirección/comuna/tipoCliente + semilla admin.
- [ ] **catálogo**: productos de gas con doble precio + semilla.
- [ ] **inventario**: semilla de stock.
- [ ] **recepcion** (ex-producción): renombrar concepto y paquetes.
- [ ] **ventas**: quitar Pesaje, ajustar DTO.
- [ ] Renombrar paquetes a `cl.elvolcan.*`.

### Fase C — End-to-end + defensa
- [ ] Repetir la guía de pruebas del README MiniMarket adaptada a "El Volcán"
      (login → token → crear producto → recepción → venta → confirmar pago → stock baja).
- [ ] Caminos de error: 401 (token malo), 403 (rol sin privilegios), 400 (validación),
      409 (stock insuficiente al vender).
- [ ] Registrar el nuevo README de pruebas para la defensa.

---

## 9. Pendientes/riesgos

- Falta el módulo **Pesaje** descargado (no está en la carpeta): correcto, igual se elimina del flujo.
- ZIPs sin extraer: catálogo, producción, inventario, ventas.
- Config de Oracle API (`ojdbc.properties`) apunta a una cuenta Oracle Cloud personal → revisar o migrar a MySQL.
- Verificar la versión de Spring Boot 4.0.6 + Java 21 funcionando antes de empezar (es muy nueva).