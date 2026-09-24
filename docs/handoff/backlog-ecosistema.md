# Backlog multirrepositorio de Vicunav

Actualizado: 2026-09-24.

## Propósito

Este archivo ordena únicamente el trabajo pendiente que cruza repositorios. Cuando
exista un issue en GitHub, el issue será la fuente de su estado, alcance y aceptación.
Los identificadores de esta tabla son referencias de planificación, no números de
issue.

## Trabajo completado

| Repositorio | Resultado vigente |
| --- | --- |
| `vicunav-standards` | Ocho estándares compartidos publicados; fidelidad visual vigente en `5c5af785ae7d157af876da8367c2d30f992f0319` |
| `vicunav-repo-template` | Template con submódulo, AGENTS, contribución, issue atómico, clasificación visual, checklist de PR y CI; revisión `34179579367d89c6b6c7d1510fd24163c25b4ca2` |
| `vicunav-hub` | Diez ADRs vigentes, spec durable de restaurante, gobierno, estado y backlog consolidados; HUB-VIS-01 y HUB-VIS-02 completan el funnel preventivo |
| `vicunav-theme-core` | Base 0.1.0; THEME-REST-04 completó la variación Bonasera verificable en `8628097f024ccb9214d82caf8d87c5ece9de162f` y THEME-REST-05 recuperó chrome y patterns 1:1 en `7c30b2ce250bb85572dae4a4cd51841921c4e98a` |
| `vicunav-pagos` | Capas base `Vicu\Core` (contrato 1.0.0) incluidas en `core/`; PAGOS-01 a PAGOS-03 completos; plugin 0.4.0 y contrato 0.3.0 con persistencia transaccional, proveedor manual idempotente y lectura normalizada de su opción, estados, expiración, eventos versionados, pruebas, E2E real y CI |
| `vicunav-restaurante` | REST-02A a REST-02S completos; plugin y contrato 1.0.0, siete bloques públicos con contrato visual neutral, privacidad nativa, matriz WordPress/PHP y prerelease `v1.0.0-rc.1`; REST-02S cerró en `a46d1d746e0b880dca949a875d2dceb4b9207c61` |
| `vicunav-demo-restaurante` | Migración Bonasera integrada y aprobada con diferencias explícitas en `9a5776837cf36c6707bd44199bc77b3eeb930851`: nueve rutas FSE, siete flujos, Global Styles efectivo, 35 comparaciones revisadas y placeholders autorizados |
| Referencia de diseño Bonasera | DESIGN-REST-01 auditó el commit `1e1f62787e088c0ca9701500e764802499d1b253`, sus siete pantallas, reglas, contratos propuestos, tokens y defectos; REST-01 incorporó el resultado sin aceptar su mapeo legacy a WooCommerce |
| `vicunav-bhoga-yoga` | BHO-00 y BHO-01 publicaron la implementación privada del cliente, brief, inventario preliminar, prompts, QA y contrato de rollback; WordPress y producción no se modificaron |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | HUB-VIS-01 | `vicunav-hub` | Registrar decisión, funnel y reapertura del checkpoint visual | Auditoría posterior a DEMO-REST-01D | Documentado mediante issue 87 y PR 88 |
| 2 | STANDARDS-VIS-01 a HUB-VIS-02 | Varios | Endurecer estándar, plantilla y adopción canónica antes de otra migración | HUB-VIS-01 | Completo; commits fijados en el plan visual |
| 3 | DESIGN-REST-02 a HUB-VIS-03 | Varios | Recuperar Bonasera 1:1 en theme, vertical y demo, y cerrar el gate con aprobación humana | HUB-VIS-02 | Transferido a Claude el 2026-08-29; continuar desde el [handoff específico](restaurante-handoff-claude.md), sin reutilizar el rework local retirado |
| 4 | BHO-02 a BHO-09 | `vicunav-bhoga-yoga` e infraestructura autorizada | Migrar Bhoga 1:1 sobre theme-core y las capacidades base de pagos | Gate del cliente cerrado; BHO-02 desacoplado de HUB-VIS-03; aprobaciones de corte | BHO-02 listo para empezar; el corte live requiere aprobación humana |
| 5 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | HUB-VIS-03 | Bloqueado; no autorizado en ejecución |
| 6 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

La iniciativa `THEME-DYN-01` queda registrada para una fase posterior, sin alterar el
orden vigente: `vicunav-theme-core` debe publicar el contrato neutral de configuración
dinámica, exportable e idempotente definido por el ADR 0013. Su aceptación exige dos
identidades de verticales sobre el mismo theme, ausencia de trazas específicas en el
core y paridad verificada en frontend y Site Editor. La implementación debe coordinar
la migración y retirada de cualquier child theme transitorio.

La recuperación, aceptación y propietario de cada unidad están en el
[plan atómico de fidelidad visual](plan-fidelidad-visual.md). La historia del runtime
permanece en el [plan de restaurante](plan-restaurante.md). La migración del cliente
Bhoga Yoga está desglosada en el [plan específico](plan-bhoga-yoga.md).

## Pista Bhoga

`vicunav-bhoga-yoga` conserva en privado la implementación del cliente real. Consume
`vicunav-theme-core` y las capacidades base incluidas en `vicunav-pagos`, y no es una
demo pública ni un paquete reusable.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| BHO-00 a BHO-01 | `vicunav-hub` y `vicunav-bhoga-yoga` | Crear la fundación privada, inventarios, prompts y validación | Solicitud del usuario | Completado localmente |
| BHO-02 | `vicunav-bhoga-yoga` | Congelar baseline de tres rutas, estados y viewports | Gate del cliente (cerrado) | Listo para empezar; desacoplado de HUB-VIS-03 |
| BHO-03 a BHO-07 | Varios | Inventariar, componer con theme-core y las capacidades base, migrar y aprobar QA integral | BHO-02 | Pendiente |
| BHO-08 | Proyecto e infraestructura autorizada | Crear respaldo Elementor privado y ensayar rollback | BHO-07 | Pendiente |
| BHO-09 | Infraestructura autorizada | Reemplazar producción y observar el corte | BHO-08 y aprobación humana explícita | Pendiente |

## Pista paralela de diseño

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| DESIGN-HOTEL-01 | Varios | Auditar el diseño aprobado de hotel y separar presentación, capacidades base, pagos, dominio y composición | HUB-VIS-03 y handoff aprobado | Bloqueado por recuperación visual |

## Pendientes y riesgos

- **Corrección del 2026-08-29:** el PR #110 había registrado el checkpoint Bonasera
  como cerrado, con "35 diferencias revisadas y aprobadas". Una revisión posterior el
  mismo día encontró que esa aprobación no fue humana página por página: las capturas
  mostraban diferencias perceptuales visibles, con un promedio de 47,86 % a 64,45 % por
  superficie y picos superiores al 94 %. Ese cierre se revierte; el checkpoint queda
  reabierto y el trabajo se transfiere a Claude (ver
  [`restaurante-handoff-claude.md`](restaurante-handoff-claude.md)).
- El contrato dinámico del ADR 0013 todavía no está implementado. Hasta completar
  `THEME-DYN-01`, un child theme ya autorizado solo puede tratarse como excepción
  transitoria y no como patrón para demos o verticales nuevos.
- REST-02A a REST-02R conservan su estado funcional. Las entregas históricas de theme
  y demo permanecen fusionadas, pero el gate de DEMO-REST-01D confundió validación
  estructural con fidelidad visual. El producto integrado no está aprobado y su
  checkpoint visual queda reabierto mediante el ADR 0010.
- La variación Bonasera persistida no llega al CSS efectivo porque carece del marcador
  exigido por WordPress. Incluso después de corregirlo, la composición actual es una
  simplificación y requiere el funnel completo, no un parche aislado.
- El video hero y los dos mapas originales siguen ausentes. Su recuperación o una
  sustitución aprobada bloquean la paridad final de esos elementos.
- DESIGN-REST-02 comparó siete superficies en cinco viewports. Las 35 filas quedaron
  `different`, sin coincidencias ni aprobaciones implícitas. El resultado es baseline
  de deuda, no un gate visual aprobado.
- La paleta global final de Vicunav sigue pendiente, pero no bloquea pagos ni la
  variación Bonasera aislada.
- Los diseños de restaurante y hotel pueden descubrir funcionalidades,
  pero un elemento visual no define por sí solo un contrato de backend. Antes de crear
  lógica se deben precisar estado, datos, permisos, errores y repositorio propietario.
- Bhoga Yoga contiene identidad, fotografías y testimonios de personas reales. Los
  derechos de uso en el proyecto privado fueron confirmados; la media se incorporará
  solo dentro de ese alcance y con su procedencia documentada, y nunca en paquetes
  reusables. El live no se modifica antes de un staging Elementor privado, un backup
  inmutable y un rollback ensayado.
- `Vicu\Core` (dentro de `vicunav-pagos`) declara compatibilidad mínima con WordPress
  6.6 y PHP 8.1. Su matriz CI específica para esas versiones es una mejora futura y no
  bloquea ninguna unidad vigente.

## Reglas de mantenimiento

- Una entrada pertenece a un repositorio y se convierte en un issue, una rama, un PR y
  un squash-merge.
- No mezclar cambios de varios repositorios en el mismo commit.
- Eliminar del backlog el detalle de una tarea cuando su issue se cierre; conservar
  solo el resultado relevante en el estado canónico.
- Añadir trabajo nuevo únicamente cuando tenga propietario, dependencia y aceptación
  observables.
- Actualizar este archivo y el estado al finalizar cada fase, no después de cada commit
  interno.
