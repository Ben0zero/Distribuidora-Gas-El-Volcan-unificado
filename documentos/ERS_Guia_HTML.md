# Guía de ubicaciones por página — Distribuidora Gas El Volcán

Guía de referencia para defender el proyecto: cada cosa con su **archivo y línea exacta**.

---

## 0. Esqueleto común (lo mismo en las 13 páginas)

| Qué | Dónde |
|---|---|
| CSS Bootstrap + Bootstrap Icons + `estilos.css` | `<head>`, líneas ~7–11 |
| Botón flotante a tiendas (`.boton-tienda`) | línea ~12–14 |
| Navbar principal (`navbar-expand-lg`) | línea ~16–17 |
| **Hamburguesa** (`data-bs-target="#menuNavegacion"`) | línea ~21–22 |
| Menú desplegable `#menuNavegacion` | línea ~24–25 |
| Enlaces (`navbar-nav`) | líneas ~26–31 |
| Botón "Acceder" (btn-login) + **`#contador-carrito`** | líneas ~32–35 |
| Bootstrap JS (`bootstrap.bundle`) + **`JS/java.js`** | últimas líneas del `<body>` |

> El JS se carga en TODAS las páginas (ej: `carrito.html:103`, `index.html:98`, `admin.html:284`). Cada función se activa solo si encuentra su elemento.

---

## 1. `index.html` — Portada

| Qué | Línea |
|---|---|
| Botón flotante tiendas | 14 |
| Navbar + hamburguesa + contador | 17–36 |
| Fondo "hero" (`section class="fondo"`) | 43 |
| Slogan "El mejor calor…" | 45–47 |
| Imagen de cilindros portada | 49 |
| **Ventajas/servicios** (4 tarjetas con íconos) | 53–72 |
| Footer (logo, contacto, copyright) | 77–95 |
| Scripts (bootstrap + java.js) | 97–98 |

**Ojo:** línea 29 hay `Nosotros → nosotros.html` (archivo inexistente). Título genérico `Document` (línea 6).

---

## 2. `catalogo.html` — Catálogo (R.1) — 797 líneas

| Qué | Línea |
|---|---|
| **Carrusel escritorio** `#carruselProductosDesktop` (3 productos por slide) | 48 |
| **Carrusel móvil** `#carruselProductosMobile` | 418 |
| Tarjetas `.tarjeta-catalogo` (imagen + precio res/com + botón "Agregar") | p. ej. 58–79 |
| Botón "Agregar" de un producto | 73, 98, 123… (uno por tarjeta) |
| `#contador-carrito` (badge del carrito) | 34 |
| Script java.js | 794 |

**Para defender:** las tarjetas están en HTML, pero el JS (`iniciarCatalogo`) las **casa por el nombre de la imagen** y les conecta el botón (así el carrito guarda el id real del producto).

---

## 3. `carrito.html` — Carrito (R.2)

| Qué | Línea |
|---|---|
| Mensaje "carrito vacío" `#carrito-vacio` | 48 |
| Zona que se muestra con productos `#carrito-resumen` (arranca `d-none`) | 54 |
| Encabezados de columna (Producto/Cant/Unit/Subtotal) | 56–62 |
| **Donde JS dibuja cada fila `#lista-carrito`** | 64 |
| **Total general `#carrito-total-precio`** | 68 |
| Nota de tarifa `#nota-tarifa-carrito` | 70 |
| Botón **Vaciar carrito** `#boton-vaciar-carrito` | 73 |
| Enlace **Continuar compra** → `ventas.html` | 74 |
| Script java.js | 103 |

---

## 4. `registro.html` — Registro (R.4 + R.8)

| Qué | Línea |
|---|---|
| Formulario `#formulario-registro` | 66 |
| `#nombre` (≤100) | 75 |
| **`#run`** (RUN 7–9, patrón en HTML) | 89 |
| `#correo` (dominio permitido en JS) | 104 |
| `#telefono` (opcional) | 117 |
| `#tipo-cliente` (Residencial/Comercial) | 129 |
| `#direccion` | 151 |
| **`#region`** (la llena JS) | 162 |
| **`#comuna`** (se llena al elegir región) | 176 |
| `#password` (4–10) | 192 |
| `#confirmar-password` | 207 |
| Script java.js | 273 |

---

## 5. `login.html` — Login (R.3)

| Qué | Línea |
|---|---|
| Sección de login `.fondo` `#inicio-sesion` | 42 |
| Caja blanca `#inicio-datos` | 43 |
| Formulario `#form-login` | 52 |
| `#correo` | 58 |
| `#contrasena` | 67 |
| Botón `#btn-iniciar-sesion` | 79 |
| Script java.js | 125 |

**Para defender:** el JS valida dominio (@duoc.cl/@profesor.duoc.cl/@gmail.com) y contraseña 4–10, busca en `usuarios`, guarda `sesion` y redirige según rol (admin → `admin.html`).

---

## 6. `contacto.html` — Contacto (R.5)

| Qué | Línea |
|---|---|
| Formulario `#form-contacto` | 73 |
| `#nombre` con **maxlength 100** | 79–81 |
| `#correo` | 88 |
| `#telefono` (opcional) | 96 |
| `#asunto` (select) | 103 |
| `#mensaje` con **maxlength 500** | 124–127 |
| Botón Enviar | 132 |
| Script java.js | 176 |

---

## 7. `tiendas.html` — Puntos de venta

| Qué | Línea |
|---|---|
| Botón flotante | 38 |
| Navbar + hamburguesa | 46–69 |
| Título "¿Dónde encontrar…?" | 84–110 |
| Sección **sucursales + mapa** | 118–145+ |
| Lista `.lista-tiendas` y tarjetas `.tienda-card` | 129 y 144 |

*Página estática (sin JS propio, salvo header). Aquí se pueden agregar los horarios por zona si se desea.*

---

## 8. `consejos.html` — Consejos (informativo)

| Qué | Línea |
|---|---|
| Botón flotante | 13 |
| Menú/navbar | 25–35 |
| Fondo `.fondo` | 42 |
| Título consejos `.titulo-consejos` | 46 |
| Sección instalación con imagen `.cilindro-consejo` | 66–79+ |
| Script java.js | 275 |

---

## 9. `ventas.html` — Checkout (R.2 aplicado)

| Qué | Línea |
|---|---|
| Vista "carrito vacío" `#pago-vacio` | 48 |
| Contenido normal `#contenido-pago` | 55 |
| **Resumen** `#lista-resumen-pago` (JS dibuja ítems) | 66 |
| **Total `#total-pago`** | 69 |
| Nota `#nota-precio-pago` (tarifa res/com) | 71 |
| Datos cliente `#cliente-nombre/-correo/-tipo` | 79–81 |
| Form `#form-pago` | 84 |
| `#direccion-pago` | 87 |
| `#comuna-pago` (las 7 comunas de Ñuble) | 91 |
| Método de pago (Efectivo / Tarjeta) | 104–111 |
| Botón Confirmar pedido | 114 |
| **Confirmación** `#confirmacion-pago` + `#numero-orden` + `#total-orden` | 126–136 |
| Script java.js | 168 |

---

## 10. `admin.html` — Dashboard

| Qué | Línea |
|---|---|
| Fondo admin en `<body class="fondo-admin">` | 13 |
| **Barra móvil con hamburguesa** (`d-lg-none`, solo pantallas chicas) | 14–24 |
| Sidebar `#sidebarAdmin` (Collapse; en móvil se abre con hamburguesa) | 28 |
| Menú lateral (Dashboard, Inventario, Empleados…) | 36–57 |
| Contenido `<main class="contenido-admin">` (velo blanco) | 112 |
| Saludo + campanita | 114–122 |
| Script java.js | 284 |

---

## 11. `admin_nuevo_producto.html` — Inventario (R.7)

| Qué | Línea |
|---|---|
| Tabla `#tabla-inventario` + cuerpo `#cuerpo-inventario` (JS la rellena) | 127–139 |
| Form **crear producto** `#form-nuevo-producto` | 156 |
| `#codigo-producto` (min 3) | 161 |
| `#nombre-producto` (max 100) | 167 |
| `#descripcion-producto` + `#precio-res-producto` + `#precio-com-producto` | 173, 180, 184 |
| `#stock-producto` (entero) + `#stock-critico` (opcional) | 192, 196 |
| `#categoria-producto` | 204 |
| `#imagen-producto` (file, opcional) | 217 |
| **Modal editar** `#modal-editar-producto` | 232 |
| Form modal `#form-editar-producto` (ids ocultos `#editar-id`/`#editar-tipo`) | 240–242 |
| Campos edit: código, nombre, precios, stock, categoría, descripción | 247–290 |
| Script java.js | 309 |

---

## 12. `admin_nuevo_usuario.html` — Crear usuario (R.6)

| Qué | Línea |
|---|---|
| Form `#form-nuevo-usuario` | 126 |
| `#nombre` | 130 |
| `#correo` | 135 |
| **`#run`** con `pattern="[0-9]{7,9}"` y `maxlength=9` | 140 |
| `#contrasena` (min 4 / max 10) | 145 |
| `#confirmar-contrasena` | 150 |
| `#telefono` | 155 |
| **`#rol`** (Administrador / Cliente / **Vendedor**) | 160 |
| **`#region`** y **`#comuna`** (dinámicas por JS) | 171, 177 |

---

## 13. `admin_lista_usuarios.html` — Listado (R.6a)

| Qué | Línea |
|---|---|
| Filtro `#filtro-usuarios` (Todos/Admins/Clientes/Vendedores) | 126 |
| Tabla `#tabla-usuarios` (cuerpo lo rellena JS) | 136 |
| **Paginación estilo mockup** (estática) | 172–179 |

---

## Mapa rápido JS → HTML (qué función anima cada página)

| Página | Funciones JS que la mueven |
|---|---|
| `index` | `sincronizarContador`, `iniciarHeaderSesion`, `iniciarCierreSesion` |
| `catalogo` | `iniciarCatalogo`, `agregarAlCarrito`, `contadorCarrito` |
| `carrito` | `iniciarListaCarrito`, `renderizarCarrito`, `totalCarrito`, `vaciarCarrito` |
| `registro` | `iniciarRegistro`, `cargarRegiones` |
| `login` | `iniciarLogin` |
| `contacto` | `iniciarContacto` |
| `tiendas` / `consejos` | ninguna (solo header) |
| `ventas` | `iniciarVentas`, `renderizarResumenPago`, `cambiarCantidad` |
| `admin` | solo header/cierre de sesión |
| `admin_nuevo_producto` | `iniciarAdminNuevoProducto`, `iniciarAdminInventario`, `abrirModalEdicion`, `iniciarEdicionProducto`, `eliminarProductoAdmin` |
| `admin_nuevo_usuario` | `iniciarAdminNuevoUsuario`, `cargarRegiones` |
| `admin_lista_usuarios` | `renderizarUsuarios`, `iniciarListaUsuarios` |