# Plan de futuro — Microservicios para Distribuidora Gas El Volcán

Guía para retomar cuando la asignatura lo exija. **No implementar antes de la entrega actual.**

---

## 1. Contexto actual (por qué esto es así)

- Sitio **100% frontend** (HTML/CSS/JS), sin backend, alojado en GitHub Pages.
- Persistencia en **`localStorage`** mediante `leer()` y `guardar()`.
- **Un solo archivo** `JS/java.js` (1351 líneas) maneja todo: sesión, carrito, catálogo, admin, checkout.
- Esto entrega con éxito los requisitos R.1–R.10. **No tocar hasta aprobar.**

---

## 2. Principio rector: el "seam" (costura)

Hoy TODO el acceso a datos pasa por dos funciones de `JS/java.js`:
- `guardar(key, valor)` → línea 47 → `localStorage.setItem(key, JSON.stringify(valor))`
- `leer(key)` → línea 38 → `JSON.parse(localStorage.getItem(key))`

**Regla de oro:** que el proyecto siga leyendo/escribiendo SIEMPRE a través de estas funciones (nunca `localStorage` a mano). Cuando llegue el día, solo cambiamos su interior por `fetch()` hacia la API y la UI completa sigue funcionando sin retoques.

---

## 3. Mapa de microservicios

Cada servicio = una acción + dueño de sus datos (base propia).

| Servicio | Dueño de los datos | Endpoints futuros (REST) |
|---|---|---|
| **auth** | sesiones, credenciales | `POST /api/login`, `POST /api/logout`, `GET /api/sesion` |
| **usuarios** | cuentas (admin/cliente/vendedor) | `GET /api/usuarios`, `POST /api/usuarios`, `GET /api/usuarios?filtro=rol` |
| **productos** | catálogo + inventario/stock | `GET /api/productos`, `POST /api/productos`, `PUT /api/productos/:id`, `DELETE /api/productos/:id` |
| **carrito** | ítems por usuario | `GET /api/carrito`, `POST /api/carrito`, `PUT /api/carrito/:id`, `DELETE /api/carrito/:id` |
| **ventas/órdenes** | pedidos confirmados | `POST /api/ordenes`, `GET /api/ordenes/:id` |
| **pagos** | transacciones | `POST /api/pagos`, `GET /api/pagos/:id` |
| **contacto** | solicitudes de contacto | `POST /api/solicitudes` |

---

## 4. Los modelos de datos YA existen (en `JS/java.js`)

Son los objetos JSON que hoy se guardan en localStorage y serán el **payload** de las APIs:

| Dato | Línea en java.js | Forma |
|---|---|---|
| `PRODUCTOS` | 4 | `{ id, codigo, nombre, descripcion, residencial, comercial, stock, stockCritico, categoria, imagen }` |
| `ADMIN_DEFAULT` | 28 | `{ run, nombre, correo, contrasena, rol, estado }` |
| `REGIONES` | 219 | `[{ nombre, comunas: [...] }]` |
| `usuarios` | — | `{ fecha, run, nombre, correo, contrasena, rol, estado, tipoCliente, direccion, region, comuna, telefono }` |
| `carrito` | — | `[{ id, cantidad }]` |
| `ordenes` | — | `{ numero, fecha, cliente, direccion, comuna, tipoCliente, metodoPago, items: [{id, cantidad, precioUnitario}], total }` |
| `solicitudes` | — | los mensajes del formulario de contacto |

**Conclusión:** la estructura de datos ya está diseñada. Los microservicios solo "cuelgan" una API encima de ella.

---

## 5. Roadmap de implementación (cuando toque)

### Fase 0 — Preparación (barata, SIN riesgo para la entrega)
- [ ] Seguir usando solo `leer()`/`guardar()` para datos (nunca localStorage a mano).
- [ ] (Opcional) Documentar contratos de API desde ya con los modelos del punto 4.

### Fase 1 — API monolítica modular (mejor relación costo/beneficio)
- [ ] Instalar **Node.js** (hoy NO está instalado en el equipo).
- [ ] Crear servidor **Express**: ONE server con un módulo de rutas por acción.
      `routes/auth.js`, `routes/usuarios.js`, `routes/productos.js`, `routes/carrito.js`, `routes/ordenes.js`, `routes/pagos.js`, `routes/solicitudes.js`
- [ ] Persistencia inicial: archivos JSON por servicio (o `lowdb`). Servir también el frontend estático.
- [ ] Cambiar `leer()`/`guardar()` por `fetch()` a `/api/...` (gracias al seam).

### Fase 2 — Separar en microservicios reales
- [ ] Mover cada módulo de rutas a su **propia carpeta/servidor** (`servicios/auth`, `servicios/carrito`…) con **su base propia**.
- [ ] Comunicación por HTTP entre servicios (o mensajería: RabbitMQ/Kafka si el curso lo pide).
- [ ] **Gateway** que centralice: `http://localhost:3000` → redirige cada `/api/X` al puerto del servicio (3001, 3002…).
- [ ] **JWT**: reemplaza `localStorage.setItem('sesion', ...)` por un token emitido por `auth` y validado en cada request.
- [ ] (Opcional) **Docker** para orquestar contenedores por servicio.

---

## 6. Recordatorios importantes

- **GitHub Pages no puede correr backend.** Cuando haya API, hostear en: Render, Railway, Vercel (serverless) o local.
- No instalar Node / no crear servicios **antes** de la entrega actual.
- Mantener el "seam": es la garantía de que la UI no se reescribe el día de la migración.
- Después de la migración, **protección por rol** sigue igual: el frontend decide redirigir (admin → admin.html), pero el **servidor valida** (Autorización) — el frontend NO es autoridad, solo UX.

---

## 7. ¿Por dónde empezar el día que se pida?

1. Preguntar al profe si basta **API REST modular** (monolito) o exigen **servicios separados**.
2. Si hay libertad → Fase 1 (mucho más rápido de defender).
3. Si exigen separación real → Saltar directo a Fase 2 pero con los endpoints de la Fase 1 ya probados.