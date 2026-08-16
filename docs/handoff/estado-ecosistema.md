# Estado canónico del ecosistema Vicunav

Actualizado: 2026-08-16.

## Cómo usar este documento

Este archivo conserva la fotografía vigente del ecosistema. Las decisiones de
arquitectura viven en [`docs/adr/`](../adr/), el trabajo pendiente vive en el
[`backlog`](backlog-ecosistema.md) y las reglas compartidas viven en
[`vicunav-standards`](../standards/).

- Responsable operativo: Codex.
- Autoridad final de producto y acciones irreversibles: usuario.
- Fuente del estado de ejecución: issues y pull requests de GitHub.
- Fuente de arquitectura y prioridades: este hub.

## Resumen actual

- Los ocho repositorios públicos existentes del ecosistema están disponibles:
  `vicunav-hub`, `vicunav-standards`, `vicunav-repo-template`,
  `vicunav-transform-claude-to-gutenberg`, `vicunav-theme-core`,
  `vicunav-plugin-core`, `vicunav-pagos` y `vicunav-restaurante`.
- El repositorio privado de Dra. Fortul funciona como implementación de referencia
  local del futuro `vicunav-demo-informativo`; todavía no pertenece a la organización
  ni debe publicarse sin una decisión separada.
- La base funcional de `vicunav-theme-core` está terminada. Los issues 1 al 29 están
  cerrados, incluidas las correcciones del color secundario y del CPT usado en la
  documentación de plantillas verticales.
- La fase fundacional CORE-01 a CORE-09 de `vicunav-plugin-core` está terminada. El
  plugin 0.1.0 implementa el contrato público 1.0.0, tiene publicada la release
  `v0.1.0` y no tiene issues ni pull requests abiertos.
- PAGOS-01 a PAGOS-03 están terminados. `vicunav-pagos` 0.3.0 aporta contrato público
  0.3.0, persistencia transaccional, creación y entregas manuales idempotentes,
  estados, concurrencia, expiración y eventos versionados, además del CPT y REST
  administrativos protegidos.
- DESIGN-REST-01 auditó el prototipo Bonasera en el commit
  `1e1f62787e088c0ca9701500e764802499d1b253`. REST-01 incorporó esa evidencia en un
  spec durable, decidió comercio propio sin WooCommerce y descompuso runtime, theme y
  demo en issues atómicos. No se implementó WordPress ni se modificó LocalWP.
- REST-02A creó `vicunav-restaurante` desde la plantilla canónica. REST-02B publicó el
  contrato 1.0.0 y REST-02C elevó el plugin a 0.3.0 con capabilities, instalación
  idempotente y un ledger InnoDB versionado. REST-02D publicó el plugin 0.4.0 con menú
  estructurado, administración nativa, IDs opacos y REST cacheable. No contiene datos
  Bonasera y no se instaló en LocalWP.
- `vicunav-hotel` y los tres repositorios canónicos de demo todavía no existen en la
  organización.
- `vicunav-gutenberg` pertenece a la organización Vicunav, pero es una migración
  independiente de `vicunav.com` y no forma parte de este ecosistema modular.
- `vicunav-transform-claude-to-gutenberg` publica el skill y los validadores que
  traducen prototipos aprobados a block themes FSE. Es tooling de desarrollo y no una
  dependencia de runtime.

## Arquitectura vigente

1. **Foundation:** `vicunav-theme-core` aporta presentación compartida y
   `vicunav-plugin-core` aporta contenido estructurado, ajustes, seguridad y REST. La
   lógica de negocio no vive en el theme.
2. **Pagos:** `vicunav-pagos` es un motor opcional, independiente de reservas y
   pedidos. Su ciclo de vida transaccional y el proveedor manual v1 están
   implementados detrás de servicios y eventos públicos.
3. **Verticales:** `vicunav-restaurante` será propietario de menú, carrito, pedidos,
   totales, delivery, pizzas y reservas. Consumirá el contrato público de pagos con
   `external_type = vicu_order`, sin WooCommerce. `vicunav-hotel` será propietario de
   su dominio cuando llegue su etapa.
4. **Demos:** `vicunav-demo-informativo` validará la base sin capas transaccionales;
   `vicunav-demo-restaurante` y `vicunav-demo-hotel` compondrán sus verticales. Los
   demos conservan contenido y composición, pero no introducen lógica reutilizable
   propia.

Los paquetes no leen directamente la base de datos interna de otro paquete. Cada repo
mantiene versión e historial propios. Las relaciones se expresan mediante dependencias
y contratos públicos.

## Estado por repositorio

| Repositorio | Estado | Qué contiene o debe contener | Próximo paso |
| --- | --- | --- | --- |
| `vicunav-standards` | Completo | Seguridad, nombres, Git, accesibilidad, compatibilidad, pruebas e idioma | Mantener las normas cuando una decisión transversal cambie |
| `vicunav-repo-template` | Completo | Plantilla base, guía de agentes, contribución, issue atómico, CI y submódulo de estándares | Usarlo para crear los repositorios restantes |
| `vicunav-hub` | Activo | Nueve ADRs, spec durable de restaurante, arquitectura, gobierno, estado y backlog | Mantenerlo sincronizado al cerrar cada etapa |
| `vicunav-transform-claude-to-gutenberg` | Base 0.1.0 publicada | Skill portable, auditor de proyectos frontend, validador FSE, referencias de LocalWP y QA, pruebas y CI | Usarlo cuando exista un diseño aprobado y refinarlo con evidencia de migraciones reales |
| `vicunav-theme-core` | Base 0.1.0 completa | Tokens, templates, partes, patrones y contrato de integración | Sustituir la identidad visual placeholder cuando exista la paleta final |
| `vicunav-plugin-core` | Base 0.1.0 publicada | Release `v0.1.0`, contrato 1.0.0, CPT compartidos, ajustes, administración, seguridad, REST y pruebas | Añadir en el futuro una matriz runtime para WordPress 6.6 y PHP 8.1 |
| `vicunav-pagos` | Motor 0.3.0 completo | Contrato 0.3.0, CPT y REST protegidos, persistencia InnoDB versionada, servicios idempotentes, proveedor manual v1, máquina de estados atómica, expiración y hooks con payload 1.0.0 | Mantener su contrato; integrar consumidores mediante servicios y eventos públicos |
| `vicunav-restaurante` | Menú 0.4.0 y contrato 1.0.0; REST-02A a REST-02D completos | Plugin con instalación versionada, `vicu_menu_item`, categorías, meta operativo, wp-admin y REST público cacheable; será propietario de ingredientes, pizzas, carrito, pedidos, totales, delivery, reservas y reacción idempotente a pagos, sin WooCommerce | Ejecutar REST-02E: ingredientes, opciones de pizza y disponibilidad revisada |
| `vicunav-hotel` | Diferido | Reservas y disponibilidad | Mantener diferido hasta completar restaurante, según ADR 0006 |
| `vicunav-demo-restaurante` | Sitio local detenido, sin repo | Contenido Bonasera y composición FSE sobre theme, core, pagos y restaurante | Crear el repo en DEMO-REST-01A cuando REST-02R y THEME-REST-03 terminen |
| `vicunav-demo-hotel` | No existe | Demostración del vertical hotelero | Esperar la implementación de hotel |
| `vicunav-demo-informativo` | Referencia privada en LocalWP | Sitio profesional no transaccional sobre `vicunav-theme-core`; contenido y estrategia de Dra. Fortul | Recibir el HTML aprobado, auditarlo y clasificar trabajo por repositorio |

## Lo que ya existe en `vicunav-theme-core`

- Compatibilidad mínima: WordPress 6.6 y PHP 8.1.
- Tokens públicos de color, espaciado y tipografía en `theme.json`.
- Templates generales: `index`, `front-page`, `page`, `single` y `archive`.
- Dos headers: predeterminado y orientado a contacto.
- Dos footers: completo y mínimo.
- Patrones `hero-centered`, `hero-split-image`, `cta-simple`,
  `testimonials-grid`, `faq-accordion` y `contact-info`.
- Integración opcional con `Vicu\Core\Settings::get()` mediante las claves `phone`,
  `address` y `business_hours`.
- Consultas preparadas para los CPT compartidos `vicu_faq` y `vicu_testimonial`.
- Documentación para que un vertical registre plantillas con
  `register_block_template()` sin acoplarse al theme.

El detalle versionado del contrato está en
[`docs/contrato-publico.md`](https://github.com/vicunav/vicunav-theme-core/blob/main/docs/contrato-publico.md).

## Contratos del ecosistema

### `vicunav-restaurante`

- REST-01 aprobado en el hub y REST-02A fusionado mediante el
  [issue 1](https://github.com/vicunav/vicunav-restaurante/issues/1) y el
  [PR 2](https://github.com/vicunav/vicunav-restaurante/pull/2) del repositorio
  propietario.
- Scaffold 0.1.0 instalable y deliberadamente vacío, con WordPress 6.6, PHP 8.1,
  Composer, WPCS, PHPUnit y CI.
- REST-02B fusionado mediante el
  [issue 3](https://github.com/vicunav/vicunav-restaurante/issues/3) y el
  [PR 4](https://github.com/vicunav/vicunav-restaurante/pull/4). Publica plugin 0.2.0,
  contrato 1.0.0, autoload, rangos contractuales, aviso administrativo y
  `vicu_restaurante_loaded`.
- REST-02C fusionado mediante el
  [issue 5](https://github.com/vicunav/vicunav-restaurante/issues/5) y el
  [PR 6](https://github.com/vicunav/vicunav-restaurante/pull/6). Publica plugin 0.3.0,
  capabilities exclusivas, un ledger InnoDB versionado y una instalación idempotente
  con compensación de fallos, sin crear tablas ni registros de dominio.
- REST-02D fusionado mediante el
  [issue 7](https://github.com/vicunav/vicunav-restaurante/issues/7) y el
  [PR 8](https://github.com/vicunav/vicunav-restaurante/pull/8). Publica plugin 0.4.0,
  `vicu_menu_item`, categorías, meta contractual, administración nativa, IDs públicos
  opacos y lecturas REST con revisión, object cache y `ETag`, sin contenido Bonasera.
- Propietario futuro de menú, disponibilidad, pizza builder, carrito, pedidos, pricing,
  delivery, reservas, bloques dinámicos y administración del vertical.
- Dinero en unidad menor, totales calculados en servidor, estados y escrituras con
  revisión e idempotencia.
- Integración con `vicunav-pagos` mediante `external_type = vicu_order`, ID externo
  opaco, proveedor manual, eventos públicos y reconciliación.
- Sin WooCommerce en v1. Una adopción posterior requiere ADR y adaptador explícitos.

La definición coordinadora está en la
[especificación durable](../especificaciones/restaurante-v1.md). El contrato técnico
vigente está en
[`vicunav-restaurante`](https://github.com/vicunav/vicunav-restaurante/blob/main/docs/contrato-publico.md).

### `vicunav-plugin-core`

- Contrato público 1.0.0 y plugin 0.1.0.
- Bootstrap instalable, autoload de `Vicu\Core` y action `vicu_core_loaded`.
- Clase abstracta `Vicu\Core\PostType`.
- CPT públicos y REST `vicu_faq` y `vicu_testimonial`.
- Servicio `Vicu\Core\Settings` con `get()` y `register_tab()`.
- Menú administrativo "Vicunav" y ajustes `phone`, `address` y `business_hours`.
- Clase `Vicu\Core\Security` para texto, email, nonces y capabilities.
- Clase `Vicu\Core\Rest` y namespace REST `vicu/v1`.

El contrato vigente está en
[`docs/contrato-publico.md`](https://github.com/vicunav/vicunav-plugin-core/blob/main/docs/contrato-publico.md).

### `vicunav-pagos`

- Contrato público 0.3.0 y plugin 0.3.0.
- CPT privado `vicu_payment_req` con capabilities dedicadas.
- Referencia externa polimórfica mediante tipo e identificador opaco.
- Monto entero en unidad menor y moneda ISO 4217.
- Colección REST administrativa protegida; no es una API de negocio entre plugins.
- Tabla InnoDB interna versionada e índice único para la referencia externa.
- Servicio `Vicu\Pagos\PaymentRequests` para crear, consultar, transicionar y expirar
  sin leer post meta ni persistencia interna.
- Servicio `Vicu\Pagos\ManualPaymentProvider` para configurar el proveedor, entregar
  referencias opacas de comprobante con claves idempotentes y consultar el resultado
  público sin exponer la identidad idempotente ni el historial interno.
- Estados `pendiente`, `comprobante_subido`, `confirmado`, `rechazado` y `expirado`
  con revisión monotónica y protección ante concurrencia.
- Hooks `vicu_pagos_creado`, `vicu_pagos_confirmado`, `vicu_pagos_rechazado` y
  `vicu_pagos_expirado` emitidos después de persistir con payload 1.0.0.
- Hook `vicu_pagos_comprobante_recibido` emitido una sola vez después de confirmar la
  entrega manual y su transición, también con payload 1.0.0.
- Expiración horaria repetible sin revisiones ni eventos duplicados.
- Proveedor manual deshabilitado por defecto, sin cuentas, archivos, checkout ni
  presentación. La integración Mercantil queda para una versión posterior.

El contrato vigente está en
[`docs/contrato-publico.md`](https://github.com/vicunav/vicunav-pagos/blob/main/docs/contrato-publico.md).

## Decisiones vigentes

- La presentación y la lógica de negocio están separadas: ADR 0001.
- Pagos es un motor independiente y opcional: ADR 0002.
- Las fronteras se implementan mediante contratos y eventos: ADR 0003.
- Cada paquete vive en un repositorio independiente: ADR 0004.
- Solo se usa ACF genuino y gratuito para campos editoriales: ADR 0005.
- Restaurante se construye antes que hotel: ADR 0006.
- Dra. Fortul valida `vicunav-theme-core` como referencia del demo informativo: ADR
  0007.
- El skill de Claude Code a Gutenberg vive en un repositorio de tooling separado y no
  forma parte del runtime: ADR 0008.
- El comercio de restaurante pertenece al vertical y no usa WooCommerce en v1: ADR
  0009.
- Los README y superficies públicas se escriben en inglés; la documentación interna y
  los comentarios de código se escriben en español.
- Qwen es una herramienta global y opcional para trabajo mecánico verificable. No es
  una dependencia ni un agente propietario de ningún repositorio.

## Entorno local

- Raíz de repositorios: `~/Documents/Codex/vicunav/`.
- `vicunav-transform-claude-to-gutenberg` es la fuente local canónica del skill y su
  directorio se enlaza al inventario personal de Codex mediante symlink.
- LocalWP: `vicunav-demo-restaurante.local`, usado para verificar el theme y los
  paquetes futuros.
- `vicunav-theme-core` está enlazado al sitio LocalWP mediante symlink.
- `vicunav-plugin-core` y `vicunav-pagos` están enlazados y activos en
  `vicunav-demo-restaurante.local`; PAGOS-03 pasó allí creación idempotente, entrega
  manual repetida sin duplicados, confirmación, eventos, migración y reactivación.
- `vicunav-demo-restaurante.local` permaneció detenido y sin cambios durante REST-01.
- LocalWP: `drafortul.local`, referencia privada del demo informativo. Consume
  `vicunav-theme-core` mediante symlink y quedó sin contenido de ejemplo ni theme
  propio el 2026-08-06.
- Proyecto local de referencia: `~/Documents/Codex/drafortul/`.
- Los estándares compartidos se consumen como submódulo en `docs/standards/`.

## Qué falta

1. Ejecutar REST-02E a REST-02R en `vicunav-restaurante` según el
   [plan atómico](plan-restaurante.md).
2. Ejecutar THEME-REST-01 a THEME-REST-03 sin sustituir defaults globales del theme.
3. Crear y validar el demo mediante DEMO-REST-01A a DEMO-REST-01D.
4. Diseñar e implementar hotel y su demo después de validar restaurante.
5. Auditar el diseño aprobado de Dra. Fortul, clasificar los hallazgos por propietario
   y crear Issues atómicos.
6. Decidir si el proyecto privado se sanea, renombra y transfiere para convertirse en
   el repositorio público `vicunav-demo-informativo`.
7. Añadir una matriz runtime específica para WordPress 6.6 y PHP 8.1 a
   `vicunav-plugin-core`; la cobertura actual usa versiones más recientes y no bloquea
   `REST-01`.

La siguiente acción del camino principal es REST-02E. THEME-REST-01 puede avanzar como
pista paralela. Ambas están detalladas en el [`backlog`](backlog-ecosistema.md).
