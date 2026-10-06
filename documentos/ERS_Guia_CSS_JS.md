# Guía CSS + JS — Distribuidora Gas El Volcán

Ubicaciones exactas (archivo + línea) para defender estilos y lógica.

---

## PARTE 1 — `CSS/estilos.css` (907 líneas)

### Colores corporativos (por si preguntan)
- Azul principal `#0d3b73` → p. ej. `.texto` 14, `.resumen-carrito a` 721
- Naranjo `#ff6600` → p. ej. `.btn-login` 56, `.resumen-total strong` 716

### General / portada
| Selector | Línea | Para qué |
|---|---|---|
| `.fondo` | 1 | Fondo de portada / login en toda la página |
| `.texto`, `.texto-2`, `.texto-3` | 14, 23, 31 | Tipografías (hero, tarjetas, tablas) |
| `.cilindro` | 40 | Imagen de cilindros del hero |

### Header / navbar
| Selector | Línea |
|---|---|
| `.boton-nav` | 49 |
| `.btn-login` + `:hover` | 56, 63 |
| `.btn-carrito` + `:hover` | 103, 113 |
| `.boton-tienda` (flotante) + `img` + `:hover` | 192, 201, 207 |

### Catálogo (R.1)
| Selector | Línea |
|---|---|
| `.catalogo` | 69 |
| `@media 768px` (catálogo responsive) | 73 |
| `.tarjeta-catalogo` + `:hover` | 266, 272 |
| `.img-catalogo` | 276 |
| Flechas carrusel escritorio `#carruselProductosDesktop` | 787–814 |
| Flechas carrusel móvil `#carruselProductosMobile` | 816–844 |

### Tiendas
| Selector | Línea |
|---|---|
| `.titulo-tiendas` (h1, p) | 223–241 |
| `.texto-referencial` | 242 |
| `.lista-tiendas` (h2) | 250–265 |
| `.tienda-card` (+ h4, p, i) | 286–320 |
| `.badge-matriz` / `.badge-futura` | 321, 327 |
| `.mapa-contenedor` / `.mapa-tiendas` / `iframe` | 336–373 |
| `.leyenda-mapa` | 374 |
| `.info-futuras` | 391 |
| `@media 768px` (mapas) | 421 |

### Registro (R.4 / R.8)
| Selector | Línea |
|---|---|
| `.registro-contenedor` (caja) | 445 |
| `.icono-registro` | 462 |
| `.registro-contenedor .form-label` | 467 |
| `.form-control` / `.form-select` + foco | 472–483 |
| Enlaces | 485, 491 |
| `@media 768px` | 495 |

### Login (R.3)
| Selector | Línea |
|---|---|
| `.texto-login a` + `:hover` | 507, 513 |
| `.form-login` foco inputs | 517–518 |

### Carrito (R.2)
| Selector | Línea |
|---|---|
| `.titulo-carrito` | 527 |
| `.icono-carrito` | 545 |
| `.productos-carrito` | 553 |
| `.producto-carrito` (grilla) | 569 |
| `.producto-imagen img` | 581 |
| `.producto-info` (h3, p) | 587–597 |
| `.tipo-producto` | 598 |
| `.producto-precio` (+ strong, small) | 609–625 |
| `.producto-cantidad` / `.cantidad-control` / `.btn-cantidad` | 626–661 |
| `.btn-eliminar` + `:hover` | 662, 669 |
| `.resumen-carrito` | 676–729 |
| `.resumen-total` (+ strong naranjo) | 704–719 |
| `.aviso-carrito` (carrito vacío) | 734–754 |
| `@media 768px` (responsive carrito) | 759–781 |

### Admin (responsive + fondo)
| Selector | Línea |
|---|---|
| `.fondo-admin` (fondo imagen) | 847 |
| `.fondo-admin .contenido-admin` (velo blanco 0.9) | 854 |
| `@media 991.98px` `#sidebarAdmin.show/.collapsing` (hamburguesa) | 858 |
| `@media 767.98px` `.contenido-admin` (padding móvil) | 865 |
| `.page-link:hover` (paginación) | 872 |

### Otros
| Selector | Línea |
|---|---|
| `.icono-seguridad` (ventajas index) | 881 |
| `.blog-resumen` / `.blog-expandido` (estilos para página "Nosotros", que no existe) | 890–906 |

---

## PARTE 2 — `JS/java.js` (1348 líneas)

### Datos base (constantes)
| Dato | Línea |
|---|---|
| `PRODUCTOS` (catálogo harzeado: código, nombre, categoría, precios, stock, stock crítico, imagen) | 4 |
| `CLAVE_CARRITO/USUARIOS/PRODUCTOS/EDICIONES/ORDENES/SESION` (claves localStorage) | 21–26 |
| `ADMIN_DEFAULT` (admin@duoc.cl / Admin123, RUN 19011022, rol Administrador) | 28 |
| `REGIONES` (Región de Ñuble + 7 comunas) | 219 |

### Almacenamiento (localStorage)
| Función | Línea | Para qué |
|---|---|---|
| `leer(key)` | 38 | Leer JSON del localStorage |
| `guardar(key, valor)` | 47 | Guardar JSON |
| `obtenerUsuarios()` / `obtenerSesion()` / `tipoSesion()` | 51, 55, 64 | Accesos a datos de usuarios/sesión |

### Utilitarios
| Función | Línea |
|---|---|
| `formatearPrecio(n)` | 69 |
| `hoyISO()` | 129 |
| `toast(mensaje, tipo)` (alertas bonitas) | 136 |

### Productos
| Función | Línea |
|---|---|
| `productosCompletos()` (base + ediciones) | 73 |
| `obtenerProducto(id)` | 101 |
| `precioResidencialProducto` / `precioComercialProducto` / `precioSegunTipo` | 111, 118, 125 |

### Validaciones
| Función | Línea | Regla |
|---|---|---|
| `validarCorreo` | 163 | formato básico (contiene @, sin espacios) |
| `correoDominioPermitido` | 168 | @duoc.cl, @profesor.duoc.cl, @gmail.com |
| `validarTelefono` | 173 | opcional, 9 díg. |
| `validarRun` | 178 | 7–9 díg. sin puntos/guion |

### Usuario administrador
| Función | Línea |
|---|---|
| `asegurarAdminUsuario()` | 183 | asegura el admin base + migra credenciales viejas |

### Regiones / comunas (R.8)
| Función | Línea |
|---|---|
| `cargarRegiones()` | 223 (listener `change` de región en 236) |

### Carrito (R.2)
| Función | Línea |
|---|---|
| `obtenerVecesProducto` | 255 |
| `obtenerCarrito` / `guardarCarrito` | 264, 268 |
| `contadorCarrito` / `sincronizarContador` | 273, 282 |
| `totalCarrito(tipo)` | 296 |
| `agregarAlCarrito(id)` | 309 |
| `cambiarCantidad(id, delta)` | 326 |
| `eliminarDelCarrito(id)` | 339 |
| `vaciarCarrito()` | 346 |
| `actualizarVistasCarrito()` | 352 |
| `renderizarCarrito()` | 361 |
| `iniciarListaCarrito()` | 418 (delegación de clics en 422) |

### Catálogo (R.1)
| Función | Línea |
|---|---|
| `iniciarCatalogo()` | 448 (conecta botones "Agregar" en 469) |

### Registro (R.4 / R.8)
| Función | Línea |
|---|---|
| `iniciarRegistro()` | 478 (submit en 483) |

### Login (R.3)
| Función | Línea |
|---|---|
| `iniciarLogin()` | 576 (botón 581; redirige pedido pendiente de ventas en 616–620) |

### Contacto (R.5)
| Función | Línea |
|---|---|
| `iniciarContacto()` | 628 (submit en 633) |

### Admin — productos (R.7)
| Función | Línea |
|---|---|
| `iniciarAdminNuevoProducto()` | 686 (submit 691) |
| `iniciarAdminInventario()` | 758 (botón Editar 827, Eliminar 840) |
| `abrirModalEdicion(producto, tipo)` | 857 (pre-rellena el modal) |
| `iniciarEdicionProducto()` | 883 (submit 888) |
| `eliminarProductoAdmin(codigo)` | 986 |

### Admin — usuarios (R.6)
| Función | Línea |
|---|---|
| `iniciarAdminNuevoUsuario()` | 995 (submit 1000) |
| `renderizarUsuarios(filtro)` | 1075 (filtros por rol en 1083–1091) |
| `iniciarListaUsuarios()` | 1115 (listener del filtro 1121) |

### Checkout (R.2 aplicado)
| Función | Línea |
|---|---|
| `renderizarResumenPago()` | 1128 |
| `iniciarVentas()` | 1180 (guarda 'pendiente' = ventas si no hay sesión en 1187; delegación 1216; submit 1233) |

### Header / sesión / extras
| Función | Línea |
|---|---|
| `iniciarVerMas()` | 1286 |
| `iniciarHeaderSesion()` | 1303 (cambia "Acceder" por "Cerrar sesión") |
| `iniciarCierreSesion()` | 1320 |
| Disparadores iniciales (arrancan todo al cargar) | 1344–1348 |

---

## PARTE 3 — JSON: ¿qué es y dónde se usa?

**Resumen para memorizar:** *"JSON es el formato de intercambio de datos. Los catálogos viven como objetos JSON en el código, y lo que se genera en runtime (usuarios, carrito, órdenes, sesión) se serializa a texto JSON con `JSON.stringify` y se guarda en `localStorage`, leyéndose con `JSON.parse`."*

### Dónde: datos base como objetos JSON en `JS/java.js`
| Dato | Línea | Forma |
|---|---|---|
| `PRODUCTOS` | 4 | Arreglo de 14 objetos `{ id, codigo, nombre, descripcion, residencial, comercial, stock, stockCritico, categoria, imagen }` |
| `REGIONES` | 219 | `[{ nombre: 'Región de Ñuble', comunas: ['Chillán', 'Chillán Viejo', ...] }]` |
| `ADMIN_DEFAULT` | 28 | `{ run: '19011022', nombre: 'Juan Pérez', correo: 'admin@duoc.cl', contrasena: 'Admin123', rol: 'Administrador' }` |

### Dónde: persistencia como string JSON en `localStorage`
- Claves de almacenamiento (líneas 21–26): `carrito`, `usuarios`, `productos`, `edicionesProductos`, `ordenes`, `sesion` (+ `solicitudes` del contacto).
- Funciones centrales:
  - `guardar(key, valor)` → línea 47: `localStorage.setItem(key, JSON.stringify(valor))` (objeto → texto).
  - `leer(key)` → línea 38: `JSON.parse(localStorage.getItem(key))` (texto → objeto).
- Ejemplo real: `guardar('sesion', usuario)` deja en el navegador el string
  `'{"run":"19011022","nombre":"Juan Pérez","correo":"admin@duoc.cl","rol":"Administrador"}'`.

### Qué se guarda en cada clave
| Clave | Contenido |
|---|---|
| `usuarios` | Registrados por cliente + creados por admin (rol Cliente/Vendedor/Administrador) |
| `sesion` | El usuario logueado o `null` al cerrar sesión |
| `carrito` | Arreglo `[{id, cantidad}]` |
| `productos` / `edicionesProductos` | Inventario tras crear/editar desde admin |
| `ordenes` | Pedidos confirmados en `ventas.html` |
| `solicitudes` | Mensajes del formulario de contacto |

---

## Notas útiles para defensa
- **localStorage:** todo se persiste en el navegador. No hay backend.
- **1 archivo JS, 13 páginas:** cada `iniciarXxx()` se ejecuta siempre, pero solo "agarra" si su form/id existe en la página actual (por eso funciona en todas).
- **Delegación de eventos:** carrito (422) y checkout (1216) usan UN listener sobre el contenedor para muchos botones (mejor rendimiento).
- El CSS tiene estilos para una página "Nosotros" (`blog-resumen`, 890) pero ese HTML no existe.