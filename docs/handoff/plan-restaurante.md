# Plan atómico de restaurante

Actualizado: 2026-08-20.

## Propósito

Este documento descompone `REST-02` y la pista visual descubierta por
`DESIGN-REST-01`. Los IDs son referencias de planificación hasta que exista el issue
correspondiente en GitHub. Cada fila se convierte en un issue, una rama, un PR y un
squash-merge del repositorio propietario.

La arquitectura normativa está en el [ADR 0009](../adr/0009-restaurante-sin-woocommerce.md)
y en la [especificación v1](../especificaciones/restaurante-v1.md). Ninguna tarea
autoriza a crear los repositorios futuros antes de llegar a su unidad.

## Resultado de descubrimiento

`DESIGN-REST-01` queda completado como auditoría, no como implementación. Verificó el
commit `1e1f62787e088c0ca9701500e764802499d1b253` de Bonasera, clasificó siete pantallas,
reglas de dominio, componentes, tokens y defectos. REST-01 reemplaza el mapeo legacy a
WooCommerce por dominio propio y conserva el resto como evidencia con las correcciones
documentadas.

## REST-02: plugin de dominio

### Fundación y contrato

| Orden | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| 1 | REST-02A | `vicunav-restaurante` | Crear el repositorio desde `vicunav-repo-template`, con submódulo de estándares, plugin instalable vacío, toolchain, CI y mínimos WordPress 6.6/PHP 8.1 | REST-01 |
| 2 | REST-02B | `vicunav-restaurante` | Publicar contrato 1.0.0, bootstrap, versiones, autoload y comprobación de dependencias sobre core mayor 1 y pagos 0.3.0 | REST-02A |
| 3 | REST-02C | `vicunav-restaurante` | Implementar capabilities, mecanismo de migraciones InnoDB versionadas y base idempotente de instalación, sin crear todavía datos de dominio | REST-02B |

Aceptación específica:

- REST-02A no contiene dominio, bloques ni contenido Bonasera.
- REST-02B fija namespace, carga, errores, entidades y rutas antes de que otros issues
  expongan comportamiento.
- REST-02C prueba activación, upgrade, reactivación, rollback de fallos y prefijos de
  sitio. Cada issue de dominio añade después su propio schema y migración; ninguno
  crea datos de demo.

### Catálogo y pricing

| Orden | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| 4 | REST-02D | `vicunav-restaurante` | Registrar `vicu_menu_item`, categorías, meta contractual, relaciones y wp-admin del menú; exponer lecturas REST cacheables | REST-02C |
| 5 | REST-02E | `vicunav-restaurante` | Implementar catálogo canónico de ingredientes, opciones de pizza, disponibilidad revisada y pantallas administrativas | REST-02C |
| 6 | REST-02F | `vicunav-restaurante` | Implementar validación y cálculo de precios en servidor para pizzas, endpoint `pizza/quote` y pruebas de todas las reglas del baseline | REST-02D, REST-02E |
| 7 | REST-02G | `vicunav-restaurante` | Implementar zonas, descuentos, propinas y servicio puro de totales con redondeo en unidad menor | REST-02C |

Aceptación específica:

- REST-02D diferencia copy editorial de campos operativos y no requiere ACF en runtime.
- REST-02E ofrece una sola disponibilidad para builder y administración. Un cambio
  aumenta la revisión exactamente una vez.
- REST-02F cubre seis toppings globales, zonas exclusivas, full-price por mitad,
  referencias obsoletas y fallo cerrado.
- REST-02G cubre descuento, impuesto, propina y delivery en el orden normativo, además
  de límites concurrentes de códigos.

### Carrito, pedidos y pagos

| Orden | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| 8 | REST-02H | `vicunav-restaurante` | Implementar sesiones seguras, carritos, líneas, expiración, REST, CSRF e idempotencia por revisión | REST-02F, REST-02G |
| 9 | REST-02I | `vicunav-restaurante` | Implementar checkout transaccional, snapshots, `vicu_order`, máquina de estados, eventos y wp-admin de pedidos | REST-02H |
| 10 | REST-02J | `vicunav-restaurante` | Integrar creación idempotente con `PaymentRequests`, evidencia textual, proveedor manual, hooks, reconciliación y salud administrativa | REST-02I, PAGOS-03 |

Aceptación específica:

- REST-02H conserva una línea original mientras se edita, rechaza revisiones obsoletas
  y nunca acepta importes del cliente.
- REST-02I prueba cada transición y crea un pedido una sola vez ante retries o
  concurrencia. Los importes confirmados no son editables y la proyección `vicu_order`
  puede reconstruirse sin cambiar el pedido autoritativo.
- REST-02J usa `external_type = vicu_order`, no lee persistencia de pagos y recupera un
  hook perdido mediante reconciliación. Rechazo, expiración y eventos duplicados no
  producen transiciones inválidas.

### Reservas y cuenta

| Orden | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| 11 | REST-02K | `vicunav-restaurante` | Implementar horarios, excepciones, disponibilidad por intervalos, reserva/cancelación concurrente, privacidad, REST y wp-admin | REST-02C |
| 12 | REST-02L | `vicunav-restaurante` | Implementar pizzas guardadas con ownership, schema versionado, CRUD REST y enlaces compartibles seguros | REST-02F |

Aceptación específica:

- REST-02K prueba solapamientos de 90 minutos, dos procesos por los últimos asientos,
  timezone, aviso mínimo, cierre, cancelación y token de gestión no enumerable.
- REST-02L no expone registros de otro usuario, rechaza versiones desconocidas y no
  usa el token compartido como precio autoritativo.

### Bloques e integración pública

| Orden | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| 13 | REST-02M | `vicunav-restaurante` | Crear bloque dinámico de menú y filtros, con estados vacío, loading, error y disponibilidad | REST-02D |
| 14 | REST-02N | `vicunav-restaurante` | Crear bloque dinámico del constructor de pizzas con Interactivity API, quote y add-to-cart | REST-02F, REST-02H |
| 15 | REST-02O | `vicunav-restaurante` | Crear bloques coordinados de carrito, checkout manual y estado de pedido | REST-02J |
| 16 | REST-02P | `vicunav-restaurante` | Crear bloque de reservas con disponibilidad, alternativas, confirmación y cancelación | REST-02K |
| 17 | REST-02Q | `vicunav-restaurante` | Crear bloque de pizzas guardadas para cuenta autenticada | REST-02L |
| 18 | REST-02R | `vicunav-restaurante` | Completar E2E, accesibilidad, privacidad, rendimiento, matriz de compatibilidad y release candidata 1.0.0 | REST-02M, REST-02N, REST-02O, REST-02P, REST-02Q |

Aceptación común de bloques:

- Registro en servidor por `block.json`, versión de API compatible con WordPress 6.6 y
  assets condicionales.
- Frontend y editor sin bloques inválidos, sin `core/html`, sin runtime del prototipo y
  sin lógica de negocio duplicada en JavaScript.
- Teclado, foco, mensajes de estado, reduced motion, responsive y errores completos.
- REST-02R ejecuta los flujos reales contra core, pagos y MySQL, no solo mocks.

## Pista visual y demo

La pista puede avanzar en paralelo solo donde su dependencia esté satisfecha. No
promueve un detalle Bonasera a `theme-core` sin demostrar reutilización.

### Theme compartido

| Orden visual | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| V1 | THEME-REST-01 | `vicunav-theme-core` | Crear una style variation Bonasera seleccionable con tokens auditados, fuentes locales licenciadas y contraste medido, sin cambiar defaults globales | DESIGN-REST-01 |
| V2 | THEME-REST-02 | `vicunav-theme-core` | Corregir o añadir la variante reusable de header/footer restaurante, incluido overflow a 768/390, navegación, foco y target táctil | DESIGN-REST-01 |
| V3 | THEME-REST-03 | `vicunav-theme-core` | Añadir únicamente patterns editoriales reutilizables que falten después de comparar hero, historia, categorías, ubicación, FAQ, testimonios y CTA con el contrato actual | THEME-REST-01 |

Cada issue del theme valida frontend y Site Editor. THEME-REST-03 debe reutilizar los
patterns existentes cuando cubran el contrato y documentar por qué cualquier patrón
nuevo pertenece al núcleo.

### Demo Bonasera

| Orden visual | ID | Repositorio | Resultado atómico | Depende de |
| ---: | --- | --- | --- | --- |
| V4 | DEMO-REST-01A | `vicunav-demo-restaurante` | Crear el repositorio de composición FSE y su flujo idempotente de instalación, sin copiar paquetes | REST-02R, THEME-REST-03 |
| V5 | DEMO-REST-01B | `vicunav-demo-restaurante` | Versionar copy Bonasera, inventario y media licenciada; registrar como ausentes el video y mapas no entregados | DEMO-REST-01A |
| V6 | DEMO-REST-01C | `vicunav-demo-restaurante` | Crear páginas y composición FSE con rutas reales para los siete flujos, style variation seleccionada y sin H1 duplicados | DEMO-REST-01B, REST-02R |
| V7 | DEMO-REST-01D | `vicunav-demo-restaurante` | Ejecutar QA visual, editorial, responsive, accesible, funcional y de rendimiento en frontend y Site Editor; corregir defectos del baseline | DEMO-REST-01C |

DEMO-REST-01D compara 1440, 1024, 768, 390 y 375 px, revisa especialmente header,
overflow, selector de zona y sticky UI, y adjunta evidencia del commit exacto. No
declara paridad si faltan assets; documenta la diferencia sin inventar sustitutos.

## Dependencias resumidas

```text
REST-01
  -> REST-02A..C
     -> catálogo/pricing/totales
        -> carrito -> pedidos -> pagos
        -> bloques públicos -> REST-02R
     -> reservas -> bloque de reservas -> REST-02R
     -> pizzas guardadas -> bloque de cuenta -> REST-02R

DESIGN-REST-01
  -> THEME-REST-01..03

REST-02R + THEME-REST-03
  -> DEMO-REST-01A..D
```

## Registro de ejecución

REST-02A quedó completado en `vicunav-restaurante`: el
[issue 1](https://github.com/vicunav/vicunav-restaurante/issues/1) se cerró mediante
el [PR 2](https://github.com/vicunav/vicunav-restaurante/pull/2), fusionado por squash
después de pasar CI. El resultado es un plugin 0.1.0 instalable y vacío, con toolchain
y protección de `main`, sin lógica de dominio.

REST-02B también quedó completado: el
[issue 3](https://github.com/vicunav/vicunav-restaurante/issues/3) se cerró mediante
el [PR 4](https://github.com/vicunav/vicunav-restaurante/pull/4), después de validar
CI y los contratos reales de core 1.0.0 y pagos 0.3.0. El resultado es el plugin 0.2.0,
contrato 1.0.0, autoload, comprobación de dependencias y hook de carga, todavía sin
lógica de dominio.

REST-02C quedó completado: el
[issue 5](https://github.com/vicunav/vicunav-restaurante/issues/5) se cerró mediante
el [PR 6](https://github.com/vicunav/vicunav-restaurante/pull/6), después de pasar CI.
El resultado es el plugin 0.3.0 con capabilities, ledger InnoDB, migraciones
versionadas e instalación idempotente, todavía sin datos de dominio.

REST-02D quedó completado: el
[issue 7](https://github.com/vicunav/vicunav-restaurante/issues/7) se cerró mediante
el [PR 8](https://github.com/vicunav/vicunav-restaurante/pull/8), después de pasar CI.
El resultado es el plugin 0.4.0 con menú estructurado, administración nativa, IDs
opacos y REST cacheable, sin ingredientes ni contenido de demo.

REST-02E quedó completado: el
[issue 9](https://github.com/vicunav/vicunav-restaurante/issues/9) se cerró mediante
el [PR 10](https://github.com/vicunav/vicunav-restaurante/pull/10), después de pasar
CI. El resultado es el plugin 0.5.0 con schema 3, catálogo canónico de ingredientes,
opciones de pizza, relaciones transaccionales, compare-and-swap, una sola revisión de
disponibilidad y dos lecturas REST con `ETag`, sin pricing ni contenido de demo.

REST-02F quedó completado: el
[issue 11](https://github.com/vicunav/vicunav-restaurante/issues/11) se cerró mediante
el [PR 12](https://github.com/vicunav/vicunav-restaurante/pull/12), después de pasar
CI. El resultado es el plugin 0.6.0 con `pizza_configuration` v1, quote autoritativo,
moneda propia, máximo global de seis toppings, zonas exclusivas y precio completo por
mitad, sin carrito ni totales de pedido.

REST-02G quedó completado: el
[issue 13](https://github.com/vicunav/vicunav-restaurante/issues/13) se cerró mediante
el [PR 14](https://github.com/vicunav/vicunav-restaurante/pull/14), después de pasar
CI. El resultado es el plugin 0.7.0 con schema 4, zonas, descuentos, propinas,
revisión de pricing, compare-and-swap y totales autoritativos con redondeo en unidad
menor, sin carrito ni contenido de demo.

REST-02H quedó completado: el
[issue 15](https://github.com/vicunav/vicunav-restaurante/issues/15) se cerró mediante
el [PR 16](https://github.com/vicunav/vicunav-restaurante/pull/16), después de pasar
CI. El resultado es el plugin 0.8.0 con schema 5, sesiones hasheadas, ownership
anónimo o autenticado, líneas de menú y pizza, expiración, REST privado, CSRF,
compare-and-swap y recálculo completo, sin pedidos ni contenido de demo.

REST-02I quedó completado: el
[issue 17](https://github.com/vicunav/vicunav-restaurante/issues/17) se cerró mediante
el [PR 18](https://github.com/vicunav/vicunav-restaurante/pull/18), después de pasar
CI. El resultado es el plugin 0.9.0 con schema 6, checkout transaccional, snapshots
inmutables, conversión idempotente del carrito, ownership privado, máquina de estados,
eventos append-only y proyección administrativa reconstruible, sin integración
operativa con pagos ni contenido de demo.

REST-02J quedó completado: el
[issue 19](https://github.com/vicunav/vicunav-restaurante/issues/19) se cerró mediante
el [PR 20](https://github.com/vicunav/vicunav-restaurante/pull/20), después de pasar
CI. El resultado es el plugin 0.10.0 con schema 7, solicitudes recuperables mediante
el contrato público, evidencia textual privada, proveedor manual real, hooks 1.0.0,
reconciliación de eventos perdidos y salud administrativa, sin leer persistencia de
pagos ni incorporar archivos o bancos.

REST-02K quedó completado: el
[issue 21](https://github.com/vicunav/vicunav-restaurante/issues/21) se cerró mediante
el [PR 22](https://github.com/vicunav/vicunav-restaurante/pull/22), después de pasar
CI. El resultado es el plugin 0.11.0 con schema 8, horarios IANA, excepciones,
disponibilidad por todos los intervalos solapados, capacidad transaccional, creación y
cancelación idempotentes, ownership por cuenta o token, estados, REST privado y
proyección administrativa reconstruible, sin contenido Bonasera ni cambios en
LocalWP.

REST-02L quedó completado: el
[issue 23](https://github.com/vicunav/vicunav-restaurante/issues/23) se cerró mediante
el [PR 24](https://github.com/vicunav/vicunav-restaurante/pull/24), después de pasar
CI. El resultado es el plugin 0.12.0 con schema 9, pizzas guardadas de cuenta,
configuración versionada sin importes, ownership opaco, compare-and-swap, enlaces
rotables almacenados por hash y quote autoritativo al compartir, sin bloques,
contenido Bonasera ni cambios en LocalWP.

REST-02M quedó completado: el
[issue 25](https://github.com/vicunav/vicunav-restaurante/issues/25) se cerró mediante
el [PR 26](https://github.com/vicunav/vicunav-restaurante/pull/26), después de pasar
CI. El resultado es el plugin 0.13.0 con `vicunav/restaurante-menu`, API 3, render
dinámico, preview de editor, fallback sin JavaScript, refresh REST progresivo, filtros,
estados de carga, error, vacío y disponibilidad, assets condicionales y pruebas PHP y
JavaScript, sin contenido Bonasera ni cambios en LocalWP.

REST-02N quedó completado: el
[issue 27](https://github.com/vicunav/vicunav-restaurante/issues/27) se cerró mediante
el [PR 28](https://github.com/vicunav/vicunav-restaurante/pull/28), después de pasar
CI. El resultado es el plugin 0.14.0 con `vicunav/restaurante-pizza-builder`, API 3,
render dinámico, Interactivity API, catálogo vivo, zonas exclusivas, máximo global de
seis toppings, quote autoritativo y alta segura en el carrito con revisión y
nonce/CSRF, sin vistas de checkout, contenido Bonasera ni cambios en LocalWP.

REST-02O quedó completado: el
[issue 29](https://github.com/vicunav/vicunav-restaurante/issues/29) se cerró mediante
el [PR 30](https://github.com/vicunav/vicunav-restaurante/pull/30), después de pasar
CI. El resultado es el plugin 0.15.0 con `vicunav/restaurante-cart`,
`vicunav/restaurante-checkout` y `vicunav/restaurante-order-status`, SSR seguro, un
store y estilos compartidos, mutaciones por revisión, checkout idempotente, proveedor
manual real y token invitado fuera de URL y markup, sin reservas públicas, contenido
Bonasera ni cambios en LocalWP.

REST-02P quedó completado: el
[issue 31](https://github.com/vicunav/vicunav-restaurante/issues/31) se cerró mediante
el [PR 32](https://github.com/vicunav/vicunav-restaurante/pull/32), después de pasar
CI. El resultado es el plugin 0.16.0 con `vicunav/restaurante-reservations`, SSR
seguro, disponibilidad autoritativa, creación idempotente, alternativas cercanas,
confirmación, recuperación privada y cancelación por revisión, sin tokens en URL o
markup, contenido Bonasera ni cambios en LocalWP.

REST-02Q quedó completado: el
[issue 33](https://github.com/vicunav/vicunav-restaurante/issues/33) se cerró mediante
el [PR 34](https://github.com/vicunav/vicunav-restaurante/pull/34), después de pasar
CI. El resultado es el plugin 0.17.0 con `vicunav/restaurante-saved-pizzas`, estado de
login, colección privada, renombrado, eliminación confirmada, enlaces rotables,
revalidación antes de añadir al carrito y guardado desde el builder, sin datos de
cuenta en SSR, contenido Bonasera ni cambios en LocalWP.

REST-02R quedó completado: el
[issue 35](https://github.com/vicunav/vicunav-restaurante/issues/35) se cerró mediante
el [PR 36](https://github.com/vicunav/vicunav-restaurante/pull/36), después de pasar
la matriz WordPress 6.6/PHP 8.1 y WordPress 6.9/PHP 8.4. El squash
`a687e76f6ab0bf3de0e75cb7a392fb775be16e7a` publica plugin 1.0.0, privacidad nativa,
fixture y evidencia E2E, sin modificar LocalWP. La prerelease verificable es
[`v1.0.0-rc.1`](https://github.com/vicunav/vicunav-restaurante/releases/tag/v1.0.0-rc.1).

THEME-REST-01 quedó completado mediante el
[issue 33](https://github.com/vicunav/vicunav-theme-core/issues/33) y el
[PR 36](https://github.com/vicunav/vicunav-theme-core/pull/36). Publica la variación
Bonasera seleccionable sin cambiar los defaults globales.

THEME-REST-02 quedó completado mediante el
[issue 37](https://github.com/vicunav/vicunav-theme-core/issues/37) y el
[PR 38](https://github.com/vicunav/vicunav-theme-core/pull/38). Publica cuatro parts
de restaurante con navegación core, responsive y accesibilidad verificados.

THEME-REST-03 quedó completado mediante el
[issue 39](https://github.com/vicunav/vicunav-theme-core/issues/39) y el
[PR 42](https://github.com/vicunav/vicunav-theme-core/pull/42). Reutiliza hero,
contacto, FAQ, testimonios y CTA, y añade únicamente historia editorial y enlaces
visuales. Los correctivos descubiertos se aislaron en los
[PR 35](https://github.com/vicunav/vicunav-theme-core/pull/35) y
[PR 41](https://github.com/vicunav/vicunav-theme-core/pull/41). El gate final corrigió
el estado inicial del acordeón en el
[issue 43](https://github.com/vicunav/vicunav-theme-core/issues/43) y el
[PR 44](https://github.com/vicunav/vicunav-theme-core/pull/44).

DEMO-REST-01A quedó completado mediante el
[issue 1](https://github.com/vicunav/vicunav-demo-restaurante/issues/1) y el
[PR 2](https://github.com/vicunav/vicunav-demo-restaurante/pull/2). El squash
`b1942cb3138669bd475e43dab4aceb0828be21ab` crea el repositorio de composición,
fija revisiones exactas e instala theme, core, pagos y restaurante mediante symlinks
idempotentes en el LocalWP existente. CI y la segunda ejecución real pasaron; el
sitio todavía no contiene copy, media ni páginas Bonasera.

DEMO-REST-01B quedó completado mediante el
[issue 3](https://github.com/vicunav/vicunav-demo-restaurante/issues/3) y el
[PR 4](https://github.com/vicunav/vicunav-demo-restaurante/pull/4). El squash
`32b4e211179b68ec6df136812bd2ad5c7cf84a1a` conserva 37 platos, ocho categorías,
copy y datos ficticios, y nueve imágenes WebP locales con licencia, alt, dimensiones,
peso y checksum. Excluye retratos y marcas, y registra el video y los dos mapas como
no entregados. No modifica todavía el contenido del sitio WordPress.

DEMO-REST-01C quedó completado mediante el
[issue 5](https://github.com/vicunav/vicunav-demo-restaurante/issues/5) y el
[PR 6](https://github.com/vicunav/vicunav-demo-restaurante/pull/6). El squash
`bdc0a1536c8cd7f80a85a1084dfa6c7194c57580` aporta un aplicador WP-CLI idempotente,
importa la media local y aplica datos mediante contratos públicos. Compone portada,
siete rutas funcionales y privacidad con templates y parts FSE editables, selecciona
Bonasera y conserva un único H1 por vista. Dos aplicaciones consecutivas no crearon
duplicados ni revisiones nuevas; CI, las nueve rutas y la consola del navegador
pasaron antes del reinicio del equipo.

DEMO-REST-01D quedó completado mediante el
[issue 7](https://github.com/vicunav/vicunav-demo-restaurante/issues/7) y el
[PR 8](https://github.com/vicunav/vicunav-demo-restaurante/pull/8), después de pasar
CI y el gate real en LocalWP. El squash
`737d027f78ad301b4e0c80c2b316e131a1b807a5` versiona 45 comprobaciones responsive,
evidencia visual y validación de frontend, Site Editor, accesibilidad, consola,
media, menú, pizza, carrito, checkout manual y reservas. El gate aisló y fusionó los
correctivos de menú y checkout en `vicunav-restaurante`, y el estado inicial de FAQ
en `vicunav-theme-core`, sin incorporar lógica reutilizable al demo.

Checkpoint: REST-02A a REST-02R, THEME-REST-01 a THEME-REST-03 y DEMO-REST-01A a
DEMO-REST-01D están completos. No hay otra unidad autorizada en ejecución; hotel y
demo informativo permanecen fuera de este checkpoint.
