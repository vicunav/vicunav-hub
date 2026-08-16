# Especificación durable del vertical restaurante v1

Estado: aprobada para implementación por `REST-01` el 2026-08-16.

## 1. Propósito y autoridad

Esta especificación define la frontera de producto y arquitectura de
`vicunav-restaurante` antes de crear el repositorio. No implementa WordPress ni
reemplaza el futuro contrato público del plugin. Cuando ese repositorio exista, será
propietario de sus firmas, schemas, hooks, migraciones y pruebas; el hub conservará
las decisiones y dependencias.

Las palabras **debe**, **no debe** y **requiere** son normativas para v1. Una decisión
que cambie propiedad de datos, estados, fórmula de totales o contratos entre paquetes
requiere coordinación en el hub.

## 2. Fuentes y baseline

### Fuente aprobada

- Repositorio: `vicunav-design-to-claude-demo-restaurante`.
- Branch: `main`.
- Commit inmutable: `1e1f62787e088c0ca9701500e764802499d1b253`.
- Producto representado: Bonasera. El nombre Guasábara es una etiqueta residual del
  proyecto de diseño y no identifica el producto.
- Handoff y specs revisados: `CODEX_HANDOFF`, constructor de pizzas, reservas, API,
  mapeo de datos y mapa de componentes WordPress.

La fuente estaba limpia en el commit indicado. `npm run lint`, `npm test` con 66 de 66
pruebas y `npm run build` pasaban durante `DESIGN-REST-01`. `npm run format` reportaba
56 archivos, por lo que no se considera un check limpio ni se atribuye al trabajo de
WordPress.

### Hallazgos que gobiernan la migración

- La referencia actual es una SPA con siete pantallas efectivas y sin rutas reales:
  inicio, menú, pizzas, carrito, checkout, reserva y pizzas guardadas.
- El constructor de pizzas sí renderiza cuando se alcanza su posición real. No existe
  evidencia suficiente para declarar una sección en blanco.
- Tampoco se demostró un fallo sistemático del primer clic de navegación.
- El header y el overflow fallan a 768 px y 390 px. En móvil, el campo de zona llega a
  quedar reducido a unos 30 px.
- Carrito, checkout, reservas y pizzas guardadas duplican el H1 entre el hero y el
  contenido.
- Faltan el video hero y dos mapas. No se deben inventar, descargar ni publicar assets
  sustitutos sin procedencia y licencia.
- La paridad no incluye defectos. Gutenberg debe corregir responsive, jerarquía de
  headings, labels, foco, contraste, movimiento reducido y tamaños táctiles.
- Los cuatro métodos de pago, la carga teatral de comprobantes y el avance manual de
  estados del legacy no son un contrato de producción.

El prototipo es una especificación visual y funcional, no una dependencia de runtime.
El skill `transform-claude-to-gutenberg` es tooling de desarrollo y tampoco se incluye
en paquetes desplegados.

## 3. Fronteras de paquetes

| Paquete | Propiedad en v1 | Exclusiones explícitas |
| --- | --- | --- |
| `vicunav-theme-core` | Tokens, style variations, templates, template parts y patterns visuales reutilizables | CPT, pricing, carrito, pedidos, pagos, capacidad o reservas |
| `vicunav-plugin-core` | FAQ, testimonios, settings compartidos, seguridad reutilizable, menú administrativo y base REST `vicu/v1` | Menú de restaurante, ingredientes, pizzas, pedidos, delivery o reservas |
| `vicunav-restaurante` | Dominio completo descrito por este documento, bloques dinámicos y superficies operativas | Ciclo de vida interno de pagos y composición Bonasera |
| `vicunav-pagos` | Solicitudes de pago, proveedor manual, estados y eventos públicos | Pedidos, checkout editorial, archivos, cuentas bancarias o reservas |
| `vicunav-demo-restaurante` | Contenido, media licenciada, páginas y composición FSE de Bonasera | Lógica reutilizable, schemas o contratos de negocio |

`vicunav-restaurante` requiere `vicunav-plugin-core` dentro del contrato mayor 1 y
`vicunav-pagos` dentro del contrato publicado que incluya el proveedor manual 0.3.0.
No requiere WooCommerce.

## 4. Modelo de datos y persistencia

### Principios

- Todo importe se almacena como entero en unidad menor y con moneda ISO 4217.
- Los identificadores públicos son opacos y no enumerables. Los IDs internos nunca
  funcionan como credenciales.
- Pedidos y reservas conservan snapshots históricos. Una edición posterior del menú,
  ingrediente, impuesto o zona no reescribe una operación existente.
- Las escrituras concurrentes usan tablas InnoDB, transacciones y revisión monotónica.
- Las opciones pequeñas y de crecimiento acotado usan Settings/Options API. Los datos
  que crecen con la operación usan tablas propias versionadas.
- ACF genuino y gratuito puede mejorar la edición de campos. La persistencia, el
  contrato y el runtime no dependen de que ACF esté activo.

### Entidades

| Entidad | Propietario y almacenamiento v1 | Datos esenciales |
| --- | --- | --- |
| `RestaurantSettings` | Option estructurado del vertical, administrado mediante Settings API | Zona horaria, moneda, tasas, propinas, horarios, reglas de reserva y vigencia de carritos |
| `MenuCategory` | Taxonomía propia sobre `vicu_menu_item` | Slug estable, nombre, orden y estado visible |
| `MenuItem` | CPT público `vicu_menu_item` con meta registrada y relaciones estructuradas | ID público, nombre, copy, precio base, moneda, categoría, media, disponibilidad, alérgenos y reglas de personalización |
| `Ingredient` | Tabla propia canónica | ID estable, nombre, categoría, modificador de precio, disponibilidad, alérgenos, etiquetas dietarias y revisión |
| `MenuItemIngredient` | Tabla relacional | Ingrediente, función requerida/removible/opcional, orden y posible sustitución |
| `PizzaOption` | Tabla propia | Tipo `size`, `crust` o `sauce`, precio o modificador, orden, disponibilidad y revisión |
| `PizzaConfiguration` | Valor versionado dentro de carrito, pizza guardada y pedido | Tamaño, masa, salsa, queso, toppings por zona, cantidad y versión de schema |
| `Cart` | Tablas propias de sesión y líneas | Token de sesión hasheado, usuario opcional, estado, revisión, expiración y totales calculados |
| `CartItem` | Tabla propia | Tipo menú/pizza, cantidad, selección normalizada, snapshot provisional y revisión de catálogo |
| `DiscountCode` | Tabla propia administrativa | Código normalizado, tipo fijo/porcentaje, valor, vigencia, mínimo, límites y estado |
| `DeliveryZone` | Tabla propia administrativa | ID, nombre, estado, tarifa, ETA informativa y orden |
| `Order` | Tabla transaccional autoritativa y proyección administrativa privada `vicu_order` | ID público, cliente, tipo, dirección, estados, importes, moneda, revisiones y vínculo de pago |
| `OrderItem` | Tabla propia inmutable después del checkout | Snapshot de nombre, configuración, cantidad, precio unitario, ajustes y total de línea |
| `OrderEvent` | Tabla append-only | Actor, transición, fecha UTC, motivo seguro y metadatos no sensibles |
| `PaymentEvidence` | Tabla privada del vertical | ID opaco, pedido, referencia introducida, estado y retención; nunca credenciales bancarias |
| `Reservation` | Tabla propia | ID público, token de gestión hasheado, contacto, horario, duración, comensales, estado y revisión |
| `ReservationOccupancy` | Tabla por intervalos bloqueables | Fecha, intervalo, capacidad consumida y revisión |
| `SavedPizza` | Tabla propia ligada a `user_id` | Nombre, configuración versionada, fechas y propietario |
| `IdempotencyRecord` | Tabla propia | Scope, hash de clave, huella de request, resultado, estado y vencimiento |

Las tablas usan el prefijo real de `$wpdb` y un prefijo de paquete como
`vicu_rest_`. El nombre físico y el schema final viven en `vicunav-restaurante`; no
son API pública. La proyección `vicu_order` sirve a wp-admin, pero la tabla de pedidos
es autoridad para importes, revisión y transiciones. La proyección es derivada,
reconstruible y nunca participa en pricing o autorización. Un fallo de sincronización
se marca en salud y se repara de forma idempotente; toda acción administrativa delega
en el servicio autoritativo.

## 5. Menú, ingredientes y disponibilidad

### Menú

- Un elemento de menú tiene exactamente una moneda, que debe coincidir con la moneda
  operativa del sitio en v1.
- Precio, disponibilidad y personalizaciones se validan en servidor al añadir al
  carrito y nuevamente al crear el pedido.
- Los alérgenos y etiquetas dietarias usan vocabularios controlados. El frontend debe
  aclarar que la información no elimina el riesgo de contaminación cruzada.
- Las notas libres del cliente nunca pueden modificar el precio ni convertir un
  ingrediente no disponible en disponible.
- Dos líneas de menú solo se fusionan cuando item, opciones, ingredientes retirados,
  nota normalizada y precio autoritativo son equivalentes.

### Ingredientes

El catálogo de ingredientes es único para constructor, relaciones del menú y
administración. `available = false` impide selecciones nuevas. Un ingrediente
requerido no disponible puede volver no disponible al plato, salvo que exista una
sustitución explícita y seleccionable.

La disponibilidad se publica con una revisión global. Un carrito cuya revisión quedó
obsoleta se revalida antes de checkout y devuelve cambios legibles por línea. No se
reemplazan selecciones silenciosamente.

### Configuración de pizzas

El schema público inicial es `pizza_configuration` versión 1:

```text
size_id
crust_id
sauce_id
cheese_ingredient_id
toppings: { ingredient_id: whole|left|right }
quantity
```

Reglas normativas preservadas del baseline:

- Tamaño, masa, salsa y queso son obligatorios y deben estar disponibles.
- El máximo es seis toppings sumados entre todas las zonas.
- Un topping solo puede aparecer una vez. Cambiar de zona lo reasigna.
- `whole`, `left` y `right` cuestan lo mismo. Una mitad no aplica medio precio.
- Tamaño fija el precio base. Masa, queso y toppings aportan modificadores. La salsa
  no cambia el precio en el catálogo Bonasera auditado.
- El flag premium es presentacional; el importe siempre proviene del modificador.
- Una configuración malformada, con versión desconocida o referencias obsoletas falla
  de forma cerrada. No se completa con defaults.
- El cliente puede mostrar un estimado, pero solo un quote del servidor autoriza la
  incorporación al carrito.

`extra_ids` y reglas de incompatibilidad entre toppings quedan fuera de v1 hasta que
exista una necesidad de producto explícita.

## 6. Carrito

- Un carrito pertenece a una sesión anónima opaca o a un usuario WordPress. Al iniciar
  sesión puede asociarse de forma idempotente; no mezcla automáticamente dos carritos
  sin una política visible.
- El secreto de sesión se almacena hasheado en servidor y se transporta mediante una
  cookie segura, `HttpOnly`, `SameSite` y limitada al sitio. Las escrituras requieren
  además un token CSRF ligado a la sesión y validación de origen.
- Cada mutación incluye `expected_revision`. Una revisión obsoleta devuelve `409` con
  la revisión actual y no sobrescribe cambios.
- Las líneas de pizzas personalizadas no se fusionan automáticamente. Duplicar crea
  otra línea. Editar conserva la línea original hasta confirmar el reemplazo, corrige
  el comportamiento legacy que podía perderla al abandonar la edición.
- Los carritos expiran según configuración. La expiración no elimina pedidos ya
  creados.
- El servidor recalcula el carrito completo tras cada mutación y al iniciar checkout.
  No acepta subtotal, descuento, impuesto, propina, delivery o total del cliente.

## 7. Cálculo autoritativo de totales

Todos los cálculos usan enteros. Los porcentajes se almacenan en puntos base. El
redondeo es half-up a la unidad menor y se aplica una vez por componente agregado,
salvo que una regla fiscal futura exija cálculo por línea.

```text
subtotal = suma(precio_unitario_autoritativo * cantidad + ajustes_de_linea)
discount_total = suma(descuentos_validos), limitada a subtotal
net_merchandise = subtotal - discount_total
tax_total = round_half_up(net_merchandise * tax_rate_bps / 10000)
tip_total = round_half_up(net_merchandise * tip_rate_bps / 10000)
delivery_total = tarifa vigente de la zona, o 0 para pickup
total = net_merchandise + tax_total + tip_total + delivery_total
```

La secuencia descuento, impuesto, propina y delivery preserva la regla auditada. En v1
delivery y propina no forman parte de la base fiscal. La tasa es configuración del
comercio, no asesoría fiscal; cualquier cambio legal requiere revisar esta fórmula.

Reglas adicionales:

- Un descuento fijo no puede producir un neto negativo.
- El total contractual de v1 debe ser positivo para cumplir el contrato de
  `vicunav-pagos`. Los pedidos gratuitos quedan fuera de v1 y no se simulan con una
  solicitud de pago de monto cero.
- Los códigos se normalizan, validan y consumen en servidor. Límites concurrentes usan
  bloqueo y contador transaccional al crear el pedido, no al escribir en el campo.
- La propina es opcional. El cliente puede elegir una tasa configurada o cero; no se
  preselecciona una opción distinta de cero.
- El tipo `delivery` exige zona activa y dirección. `pickup` no acepta una tarifa de
  entrega.
- Un tipo de cambio a bolívares, si se muestra, es informativo, fechado y separado del
  monto contractual del pedido y de pagos.
- El pedido congela desglose, tasas, nombres y revisiones usadas. La solicitud de pago
  siempre usa exactamente `Order.total` y `Order.currency`.

## 8. Pedido y máquina de estados

### Estados

| Estado | Significado |
| --- | --- |
| `pendiente_pago` | Pedido creado y solicitud de pago pendiente |
| `pago_en_revision` | El proveedor manual recibió una nueva evidencia |
| `confirmado` | Pago confirmado y pedido aceptado para operación |
| `en_preparacion` | Cocina inició el pedido |
| `listo` | Pedido listo para retiro o despacho |
| `en_reparto` | Pedido delivery entregado al repartidor |
| `completado` | Operación terminada |
| `cancelado` | Pedido cancelado con motivo auditado |
| `expirado` | La solicitud de pago venció antes de confirmarse |

### Transiciones

| Origen | Destinos permitidos |
| --- | --- |
| creación | `pendiente_pago` |
| `pendiente_pago` | `pago_en_revision`, `cancelado`, `expirado` |
| `pago_en_revision` | `pendiente_pago` por rechazo, `confirmado`, `cancelado`, `expirado` |
| `confirmado` | `en_preparacion`, `cancelado` por operador autorizado |
| `en_preparacion` | `listo`, `cancelado` por operador autorizado |
| `listo` | `completado` para pickup, `en_reparto` para delivery |
| `en_reparto` | `completado` |
| terminales | Ninguno |

Cada transición usa compare-and-swap sobre `revision`, registra un `OrderEvent` una
sola vez y valida actor, tipo de entrega y estado de pago. Cancelar después de pago
confirmado requiere capability elevada y motivo. V1 no automatiza devoluciones: la
interfaz debe señalar la intervención manual pendiente sin alterar el estado confirmado
de la solicitud de pago.

El número visible del pedido es distinto de su ID interno. Consultar o gestionar un
pedido anónimo requiere un token secreto de alta entropía enviado fuera de la URL, o
una cuenta propietaria autenticada.

## 9. Checkout e integración idempotente con pagos

### Creación

1. El cliente envía checkout con una `Idempotency-Key` y la revisión del carrito.
2. El vertical bloquea el carrito, revalida catálogo, disponibilidad, descuentos,
   delivery y totales.
3. En una transacción propia crea pedido, líneas, eventos y consumo de descuento.
   También congela `payment_expires_at`; todos los reintentos reutilizan exactamente
   ese timestamp.
4. Después del commit llama a `Vicu\Pagos\PaymentRequests::create()` con:

```php
array(
	'external_type' => 'vicu_order',
	'external_id'   => $order_public_id,
	'amount_minor'  => $order_total,
	'currency'      => $order_currency,
	'expires_at'    => $expires_at_utc,
)
```

5. Guarda `payment_request_id`, revisión y estado observado. Si el proceso falla entre
   ambos commits, el pedido queda recuperable con estado de sincronización pendiente.
   Repetir la misma creación contra pagos devuelve la solicitud existente.
6. La misma clave y huella de checkout devuelve el resultado original. La misma clave
   con otro payload devuelve `409 vicu_restaurante_idempotency_collision`.

No existe una transacción distribuida entre plugins. La recuperación idempotente es
parte del flujo normal, no una excepción manual.

### Proveedor manual

- El sitio debe habilitar explícitamente el proveedor `manual`.
- `vicunav-restaurante` renderiza el checkout y las instrucciones configuradas para el
  sitio. `vicunav-pagos` no contiene copy, cuentas ni presentación.
- V1 acepta una referencia textual de la operación. El vertical persiste el mínimo
  necesario en `PaymentEvidence`, genera un ID opaco y pasa a pagos una referencia como
  `vicu-order-evidence:{id}`. Pagos no recibe el contenido privado.
- La entrega llama a `ManualPaymentProvider::submit_proof()` con clave idempotente y
  revisión esperada. La clave enviada a pagos se deriva de forma estable del ID de la
  evidencia, no de un valor efímero del request. Repetir la misma evidencia no crea
  filas ni eventos duplicados.
- La subida de archivos queda fuera de v1. Incorporarla exigiría almacenamiento
  privado, validación de tipo/tamaño, autorización de descarga, retención y borrado.

### Eventos y reconciliación

El vertical escucha `vicu_pagos_comprobante_recibido`, `vicu_pagos_confirmado`,
`vicu_pagos_rechazado` y `vicu_pagos_expirado` con payload `1.0.0`.

Antes de aplicar un evento debe comprobar:

1. `external_reference.type === 'vicu_order'`;
2. existencia del pedido por `external_reference.id`;
3. coincidencia exacta de monto y moneda;
4. revisión de pago mayor que la última aplicada;
5. transición de pedido todavía válida.

Un duplicado o evento obsoleto no cambia estado. Una discrepancia de monto, moneda o
referencia bloquea la transición y crea una alerta administrativa sin registrar datos
sensibles.

Los hooks son notificación rápida, no la única garantía. Un job repetible consulta
`PaymentRequests::get()` para pedidos no terminales con sincronización pendiente y
reconcilia por revisión. También existe una acción administrativa protegida para
reintentar un pedido. Si el fallo ocurrió después de crear la solicitud pero antes de
guardar su ID local, el job repite `PaymentRequests::create()` con la misma referencia,
monto, moneda y vencimiento para recuperar el resultado existente. El vertical nunca
lee tablas o meta internos de pagos.

## 10. Reservas, horarios y capacidad

### Configuración

- Zona horaria IANA del restaurante. Todo cálculo local usa esa zona; persistencia y
  eventos usan UTC.
- Horarios semanales con uno o más periodos por día.
- Excepciones por fecha y cierres recurrentes explícitos.
- Intervalo de slots, duración, capacidad, aviso mínimo y tamaños mínimo/máximo.
- Política de confirmación automática o pendiente.

Lunes cerrado y domingo solo almuerzo para reservas son contenido inicial de Bonasera,
no defaults universales del plugin.

### Disponibilidad

Un slot empieza dentro de un periodo y debe terminar antes de su cierre. La capacidad
se calcula sobre todos los intervalos de ocupación que cruza la duración, no solo sobre
reservas con la misma hora de inicio. Una solicitud está disponible únicamente si cada
intervalo conserva capacidad suficiente para el grupo.

En creación, una transacción:

1. crea o selecciona las filas de ocupación del rango;
2. las bloquea en orden cronológico mediante `SELECT ... FOR UPDATE`;
3. vuelve a comprobar horario, aviso, capacidad y estado;
4. inserta la reserva y aumenta ocupación;
5. confirma todo o revierte todo.

Así, dos solicitudes por los últimos asientos no pueden confirmar ambas. Un conflicto
de capacidad devuelve `409` y alternativas cercanas recalculadas.

### Estados y privacidad

Estados v1: `pendiente`, `confirmada`, `completada`, `cancelada` y `no_asistio`.
`pendiente` y `confirmada` consumen capacidad. Cancelar libera capacidad en la misma
transacción. Estados terminales no se reabren.

La reserva pública muestra un código humano solo para referencia. Gestión anónima
requiere un token aleatorio separado, almacenado hasheado; un código corto no autoriza
lectura ni cancelación. Una cuenta autenticada solo accede a sus propias reservas.

Nombre, teléfono, notas y preferencia de zona son privados. No aparecen en endpoints
públicos, logs, analytics ni HTML cacheable. La política del sitio define retención;
el default técnico propuesto anonimiza reservas terminales después de 90 días, salvo
obligación documentada distinta.

## 11. Zonas de entrega

- V1 usa selección explícita de una zona activa. No usa substring de dirección,
  geocoding ni polígonos.
- Cada zona tiene tarifa en la moneda del sitio y ETA informativa. El servidor resuelve
  el importe por ID, nunca por el precio enviado por el cliente.
- Una zona desactivada invalida carritos no confirmados y ofrece pickup u otra zona;
  no modifica pedidos existentes.
- Dirección e instrucciones se almacenan solo para pedidos delivery y se excluyen de
  respuestas públicas y logs.
- Un mapa interactivo puede visualizar zonas, pero no constituye autoridad de tarifa.
  Los dos mapas ausentes del baseline no bloquean el selector accesible de texto.

## 12. Autenticación, autorización y permisos

### Público y clientes

- Catálogo, opciones de pizza, zonas activas y disponibilidad agregada son lecturas
  públicas con `permission_callback` explícito.
- Carrito anónimo usa sesión opaca, CSRF ligado a sesión, `SameSite` y validación de
  origen. Un nonce WordPress anónimo por sí solo no es autorización suficiente.
- Usuarios autenticados usan cookies WordPress y `X-WP-Nonce`; además se verifica
  propiedad del recurso.
- APIs remotas administrativas, si se aprueban después, usan HTTPS y Application
  Passwords con capabilities, no credenciales compartidas dentro del código.
- Creación de pedidos, comprobantes y reservas tiene rate limiting, límites de tamaño
  y respuestas que no permiten enumerar recursos.

### Capabilities

El plugin define capabilities primitivas separadas, al menos para:

- administrar catálogo e ingredientes;
- cambiar disponibilidad;
- administrar descuentos y zonas;
- ver y operar pedidos;
- confirmar estados de cocina y entrega;
- ver evidencia privada;
- administrar reservas;
- modificar ajustes del restaurante;
- ejecutar reconciliación.

No se autoriza mediante `is_user_logged_in()` ni por rol nominal. Cada escritura exige
capability, nonce o credencial aplicable y revisión esperada. Administradores reciben
las capabilities al activar; no se conceden por defecto a editores, autores o clientes.

### Privacidad y seguridad

- Sanitizar y validar entrada; escapar por contexto de salida; preparar SQL directo.
- Nunca registrar tokens, nonces, referencias completas, teléfonos, direcciones,
  notas, instrucciones bancarias o payloads de evidencia.
- Las páginas de carrito, checkout, pedido, cuenta y reservas usan `Cache-Control:
  no-store`.
- Los secretos se comparan por hash seguro. Los tokens se rotan al asociar una sesión
  anónima a una cuenta cuando corresponda.
- Exportación y borrado de datos personales deben integrarse con las herramientas de
  privacidad de WordPress antes de publicar v1.

## 13. API REST

El namespace es `/wp-json/vicu/v1/restaurante`. Las rutas se registran mediante
`Vicu\Core\Rest::register_route()` y cada una declara schema, validación,
`permission_callback` y respuesta. La versión mayor vive en el namespace compartido;
cambios incompatibles requieren otra versión coordinada.

### Endpoints v1

| Método y ruta | Acceso | Función |
| --- | --- | --- |
| `GET /menu` | Público | Categorías e items visibles, con revisión y filtros acotados |
| `GET /menu/{public_id}` | Público | Detalle visible de un item |
| `GET /pizza/options` | Público | Tamaños, masas, salsas, quesos, toppings y revisión |
| `GET /ingredients/availability` | Público | Estado liviano y revisión de disponibilidad |
| `POST /pizza/quote` | Público limitado | Valida configuración y devuelve desglose autoritativo temporal |
| `GET /delivery-zones` | Público | Zonas activas, tarifa y ETA informativa |
| `POST /carts` | Público limitado | Crea sesión de carrito |
| `GET /cart` | Sesión o usuario | Devuelve carrito y revisión actuales |
| `POST /cart/items` | Sesión o usuario | Añade línea validada |
| `PATCH /cart/items/{line_id}` | Sesión o usuario | Sustituye selección o cantidad con compare-and-swap |
| `DELETE /cart/items/{line_id}` | Sesión o usuario | Elimina línea con compare-and-swap |
| `PUT /cart/discount` | Sesión o usuario | Aplica un código validado |
| `DELETE /cart/discount` | Sesión o usuario | Retira el código |
| `PUT /cart/fulfillment` | Sesión o usuario | Define pickup o delivery y zona |
| `PUT /cart/tip` | Sesión o usuario | Define una opción de propina válida |
| `POST /orders` | Sesión o usuario | Checkout idempotente y solicitud de pago |
| `GET /orders/{public_id}` | Token o propietario | Estado agregado del pedido sin datos administrativos |
| `POST /orders/{public_id}/payment-evidence` | Token o propietario | Entrega idempotente al proveedor manual |
| `GET /reservations/availability` | Público limitado | Slots por fecha y tamaño de grupo |
| `POST /reservations` | Público limitado | Crea reserva con capacidad transaccional |
| `GET /reservations/{public_id}` | Token o propietario | Consulta privada |
| `POST /reservations/{public_id}/cancel` | Token o propietario | Cancelación idempotente |
| `GET /saved-pizzas` | Usuario | Lista pizzas propias |
| `POST /saved-pizzas` | Usuario | Guarda configuración validada |
| `PATCH /saved-pizzas/{public_id}` | Propietario | Renombra o actualiza |
| `DELETE /saved-pizzas/{public_id}` | Propietario | Elimina |

El token secreto de pedido o reserva viaja en header, no en query string ni URL. Las
rutas administrativas usan wp-admin o endpoints separados protegidos; no amplían las
respuestas públicas.

### Errores

Los errores usan `WP_Error` y la forma REST estándar:

```json
{
  "code": "vicu_restaurante_stale_revision",
  "message": "El recurso cambió. Actualiza e intenta nuevamente.",
  "data": {
    "status": 409,
    "current_revision": 7,
    "retryable": true
  }
}
```

Códigos mínimos:

| Código | HTTP | Uso |
| --- | ---: | --- |
| `vicu_restaurante_invalid_request` | 400 | Schema o valor inválido |
| `vicu_restaurante_authentication_required` | 401 | Falta identidad requerida |
| `vicu_restaurante_forbidden` | 403 | Capability o propiedad insuficiente |
| `vicu_restaurante_not_found` | 404 | Recurso ausente o no revelable |
| `vicu_restaurante_unavailable` | 409 | Item, ingrediente, zona o slot ya no disponible |
| `vicu_restaurante_stale_revision` | 409 | Compare-and-swap falló |
| `vicu_restaurante_idempotency_collision` | 409 | Clave reutilizada con otra huella |
| `vicu_restaurante_invalid_transition` | 409 | Transición no permitida |
| `vicu_restaurante_payment_mismatch` | 409 | Referencia, monto o moneda no coincide |
| `vicu_restaurante_rate_limited` | 429 | Límite público excedido |
| `vicu_restaurante_storage_error` | 500 | Escritura atómica falló |
| `vicu_restaurante_dependency_unavailable` | 503 | Core o pagos no disponible |

Los errores de campos pueden añadir `fields` sin repetir valores privados. Las
escrituras no devuelven éxito parcial.

### Idempotencia y concurrencia

- `POST /orders`, `POST /payment-evidence` y `POST /reservations` requieren
  `Idempotency-Key` de alta entropía entre 16 y 191 caracteres.
- Scope incluye operación e identidad de sesión/usuario. Se guarda hash, nunca la
  clave en claro.
- La huella usa el request canónico después de sanitizar. Un retry idéntico recibe el
  mismo status y recurso.
- Los registros de operaciones financieras no expiran antes de la política de
  retención del pedido. Reservas se conservan al menos mientras puedan reintentarse.
- Mutaciones de carrito, pedido y reserva usan revisión monotónica obligatoria.
- Respuestas de lectura publican `revision` y `ETag` cuando sean estables.

## 14. Superficies wp-admin

Todas viven bajo el menú Vicunav mediante `Settings::register_tab()` o subpáginas
propias con capabilities específicas.

1. **Menú:** lista y edición de items, categorías, precios, media, alérgenos,
   personalizaciones y disponibilidad.
2. **Ingredientes:** lista filtrable, disponibilidad rápida, modificadores, dieta,
   alérgenos y relaciones. El cambio muestra impacto antes de guardar.
3. **Pizzas:** tamaños, masas, salsas, límites y orden. La UI impide borrar opciones
   referenciadas y ofrece desactivar.
4. **Pedidos:** lista por estado/tipo/fecha; detalle con snapshots, eventos, revisión de
   pago y acciones válidas. No permite editar importes confirmados.
5. **Pagos manuales:** evidencia privada, estado sincronizado, discrepancias y retry de
   reconciliación. No replica la máquina de pagos.
6. **Reservas:** lista y vista temporal, capacidad, creación interna, confirmación,
   cancelación y no asistencia.
7. **Delivery:** zonas, tarifas, ETA y estado.
8. **Descuentos:** códigos, reglas, vigencia y uso.
9. **Ajustes:** moneda, tasas, propinas, horarios, excepciones, capacidad, aviso,
   duración, expiraciones e instrucciones manuales del sitio.
10. **Salud:** versiones de schema y contrato, dependencias, jobs pendientes y última
    reconciliación, sin secretos.

Acciones masivas que cambien disponibilidad, estados o privacidad requieren nonce,
capability, confirmación y resultado por registro. No se construye un panel React si
las tablas y formularios nativos cubren el flujo.

## 15. FSE, bloques y límites editoriales

### `vicunav-theme-core`

- Conserva sus defaults globales. La identidad Bonasera solo puede entrar como style
  variation seleccionable y explícita, nunca sustituyendo valores generales.
- Reutiliza o añade patterns únicamente cuando sean agnósticos del restaurante:
  hero, CTA, historia, FAQ, testimonios, contacto y secciones editoriales.
- Header y footer son template parts. La variante de restaurante debe corregir
  overflow, navegación, foco y responsive antes de promoverse como reutilizable.

### `vicunav-restaurante`

Registra bloques dinámicos con `block.json`, render de servidor y assets condicionales:

- menú y filtros;
- constructor de pizzas;
- carrito;
- checkout manual;
- estado de pedido;
- selector informativo de zonas;
- formulario y resultado de reservas;
- pizzas guardadas.

La Interactivity API es la opción preferida para estado cliente y comunicación entre
bloques. PHP sigue siendo autoridad de markup inicial, permisos, precios y datos. El
plugin no serializa lógica de negocio dentro de atributos de bloques.

En WordPress 6.7 o superior puede registrar plantillas predeterminadas mediante
`register_block_template()`. Mientras 6.6 siga soportado, comprueba la función y usa la
jerarquía normal sin polyfill privado.

### `vicunav-demo-restaurante`

Construye páginas reales para inicio, menú, crea tu pizza, carrito, checkout, estado
de pedido, reservas y pizzas guardadas. No reproduce el enrutador de estado de la SPA.
Conserva copy y contenido Bonasera aprobados, selecciona la variación visual y aporta
media licenciada. Un template o pattern no debe imprimir de nuevo el título si la
página o bloque ya aporta su H1.

FSE permite editar composición, copy, media, tokens autorizados y ubicación de
bloques. No permite editar fórmulas, estados, capabilities, schemas, disponibilidad
transaccional o endpoints.

## 16. Accesibilidad y responsive

La línea base es WCAG 2.1 AA y el flujo transaccional debe permitir revisión,
corrección y confirmación antes de crear una obligación.

- Un solo H1 por vista, headings jerárquicos y landmarks reales.
- Labels visibles, instrucciones, errores asociados, `autocomplete` y conservación de
  valores válidos.
- Constructor, filtros, steppers, dialogs y selectores operables por teclado con foco
  visible. Los grupos exclusivos exponen semántica radio.
- Mensajes loading, error, cambios de precio y estados usan regiones de estado sin
  mover el foco de forma inesperada.
- Modales atrapan y restauran foco; `Escape` cierra cuando no pierde una operación.
- Targets táctiles de al menos 44 por 44 CSS px.
- Imágenes con alt contextual o vacío decorativo. No se usa texto incrustado en mapas
  como única fuente de información.
- Contraste de texto y controles medido en estados normal, hover, focus, active, error
  y disabled.
- `prefers-reduced-motion` elimina desplazamientos y animaciones no esenciales.
- Orden DOM lógico aunque el layout cambie.

Viewports mínimos de validación: 1440, 1024, 768, 390 y 375 px. No debe existir scroll
horizontal a 320 px salvo una región explícita, etiquetada y operable. El header, campo
de zona, sticky bars, drawer y checkout se prueban especialmente a 768 y 390 px por los
defectos del baseline.

## 17. Rendimiento y caché

- No se porta React, Tailwind, Vite ni el runtime del prototipo al frontend.
- Los bloques registran assets por metadata y los cargan solo cuando están presentes.
- Menú y opciones admiten object cache, `ETag` y TTL corto con invalidación por
  revisión. Disponibilidad usa TTL de segundos o `no-cache`.
- Carrito, quote, checkout, pedidos y reservas usan `no-store`.
- Listados wp-admin se paginan; ninguna pantalla carga todos los pedidos o reservas.
- Imágenes son locales, responsive, dimensionadas y lazy salvo el recurso LCP. Fuentes
  son locales y con licencia.
- No se realizan llamadas por ingrediente o línea. Las respuestas agrupan datos y los
  queries evitan N+1.
- Un PR que introduzca un bloque registra tamaños gzip propios y compara Lighthouse.
  Objetivos de demo: Performance 90 o más, Accessibility 100, CLS máximo 0.1 y ausencia
  de errores de consola en los viewports acordados. Una desviación documenta causa y
  aprobación, no se oculta.

## 18. Estrategia de pruebas

### Automatizadas

- PHPUnit con WordPress real para registro, migraciones, capabilities, REST y hooks.
- Pruebas unitarias por cada regla de precio, redondeo, descuento, disponibilidad,
  pizza, estado, idempotencia y autorización.
- Pruebas de integración MySQL/InnoDB para compare-and-swap, rollback, migraciones y
  dos procesos compitiendo por cupo, descuento, checkout o transición.
- Contratos contra `vicunav-plugin-core` mayor 1 y `vicunav-pagos` 0.3.0: creación
  repetida, colisión, comprobante repetido, confirmación, rechazo, expiración, evento
  duplicado y reconciliación después de un hook perdido.
- Pruebas JavaScript del store de Interactivity API sin duplicar reglas de servidor.
- E2E de navegador para menú, pizza, carrito, checkout manual, estado, reserva y pizza
  guardada, incluidas rutas de error y refresh.
- Tests de schema que congelen nombres de campos, status y errores públicos v1.

### Manuales

- Frontend y Site Editor en WordPress 6.6 y en la versión estable objetivo.
- Teclado completo, lector de pantalla de muestra, axe/Lighthouse y contraste medido.
- Comparación visual contra el commit auditado en los cinco viewports, preservando
  decisiones y corrigiendo defectos conocidos.
- Revisión de permisos con administrador, operador delegado, cliente y anónimo.
- Verificación de logs, exports de privacidad, caché y ausencia de datos personales.
- Build sin 404, recursos remotos inesperados ni dependencias del servidor de diseño.

LocalWP es evidencia de integración, no fuente de verdad. REST-01 no lo inicia ni lo
modifica.

## 19. Alcance

### Incluido en v1

- Menú estructurado, categorías, ingredientes, disponibilidad y personalizaciones.
- Constructor mitad y mitad con pricing autoritativo.
- Carrito anónimo/autenticado, descuentos, propina, pickup y delivery por zona.
- Pedido, estados operativos y snapshots.
- Checkout con un único proveedor manual real de `vicunav-pagos`.
- Evidencia textual privada, eventos de pago y reconciliación.
- Reservas con horarios, excepciones, capacidad concurrente y cancelación segura.
- Pizzas guardadas para usuarios autenticados.
- wp-admin, REST, bloques dinámicos, privacidad, accesibilidad y pruebas descritas.

### Posterior a v1

- WooCommerce o migración hacia él.
- Proveedores bancarios automáticos, webhooks externos, devoluciones y conciliación
  contable.
- Archivos de comprobante.
- Geocoding, polígonos, tracking de repartidor y asignación logística.
- Inventario cuantitativo por ingrediente, recetas y descuento automático de stock.
- Mesas físicas, combinación de mesas, depósitos o pagos de reservas.
- Incompatibilidades de toppings, precio parcial por mitad y extras reservados.
- Checkout multimoneda o tasa de cambio autoritativa.
- Programa de lealtad, order again, notificaciones push/SMS y analytics de terceros.
- Cualquier asset no entregado o sin licencia clara.

## 20. Referencias oficiales de WordPress

- [Agregar endpoints REST](https://developer.wordpress.org/rest-api/extending-the-rest-api/adding-custom-endpoints/)
- [Autenticación REST](https://developer.wordpress.org/rest-api/using-the-rest-api/authentication/)
- [Metadata de bloques](https://developer.wordpress.org/block-editor/reference-guides/block-api/block-metadata/)
- [Interactivity API](https://developer.wordpress.org/block-editor/reference-guides/interactivity-api/)
- [Global Settings y `theme.json`](https://developer.wordpress.org/block-editor/how-to-guides/themes/global-settings-and-styles/)
- [`register_block_template()`](https://developer.wordpress.org/reference/functions/register_block_template/)
- [Crear tablas desde plugins](https://developer.wordpress.org/plugins/creating-tables-with-plugins/)
- [Settings API](https://developer.wordpress.org/plugins/settings/settings-api/)
