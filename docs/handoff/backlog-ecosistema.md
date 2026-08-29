# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-26.

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
| `vicunav-hub` | Once ADRs, spec durable de restaurante, gobierno, estado y backlog consolidados; HUB-VIS-01 y HUB-VIS-02 completan el funnel preventivo |
| `vicunav-transform-claude-to-gutenberg` | Skill 0.1.0 con manifiesto, captura Chromium, comparación, reportes, hashes y gate visual publicados hasta `a55cfe447f8ba72098cf940c75605482236d2b35` |
| `vicunav-theme-core` | Base 0.1.0; THEME-REST-04 completó la variación Bonasera verificable en `8628097f024ccb9214d82caf8d87c5ece9de162f` y THEME-REST-05 recuperó chrome y patterns 1:1 en `7c30b2ce250bb85572dae4a4cd51841921c4e98a` |
| `vicunav-plugin-core` | Fase fundacional CORE-01 a CORE-09 completa; contrato 1.0.0, plugin 0.1.0 y release `v0.1.0` publicados |
| `vicunav-pagos` | PAGOS-01 a PAGOS-03 completos; plugin 0.3.1 y contrato 0.3.0 con persistencia transaccional, proveedor manual idempotente y lectura normalizada de su opción, estados, expiración, eventos versionados, pruebas, E2E real y CI |
| `vicunav-restaurante` | REST-02A a REST-02R completos; plugin y contrato 1.0.0, siete bloques públicos, privacidad nativa, matriz WordPress/PHP y prerelease `v1.0.0-rc.1`; el gate integrado corrigió menú y checkout sin añadir contenido Bonasera ni WooCommerce |
| `vicunav-demo-restaurante` | Ensamblaje, contenido, rutas y flujos reales disponibles; DESIGN-REST-02 fija 35 diferencias abiertas y DEMO-REST-02A clasifica ocho originales recuperados, seis grupos pendientes y 41 bloqueos finales en `488f21521abf1d723ab3f4eebd1d90c83bcb3af8` |
| Referencia de diseño Bonasera | DESIGN-REST-01 auditó el commit `1e1f62787e088c0ca9701500e764802499d1b253`, sus siete pantallas, reglas, contratos propuestos, tokens y defectos; REST-01 incorporó el resultado sin aceptar su mapeo legacy a WooCommerce |
| Referencia privada de `vicunav-demo-informativo` | Dra. Fortul conserva estrategia y contenido; WordPress local quedó limpio y consume `vicunav-theme-core` |
| `vicunav-yoga` | YOGA-00 a YOGA-02 publicaron el plugin neutral 0.1.0, el contrato público 1.0.0, prompts, CI y validación |
| `vicunav-bhoga-yoga` | BHO-00 y BHO-01 publicaron la implementación privada, brief, inventario preliminar, prompts, QA y contrato de rollback; WordPress y producción no se modificaron |
| `vicunav-demo-yoga` | DEMO-YOGA-01 publicó la fundación del website demo público saneado, su configuración, prompts, CI y validación; todavía no tiene LocalWP ni contenido |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | HUB-VIS-01 | `vicunav-hub` | Registrar decisión, funnel y reapertura del checkpoint visual | Auditoría posterior a DEMO-REST-01D | Documentado mediante issue 87 y PR 88 |
| 2 | STANDARDS-VIS-01 a HUB-VIS-02 | Varios | Endurecer estándar, skill, tooling, plantilla y adopción canónica antes de otra migración | HUB-VIS-01 | Completo; commits fijados en el plan visual |
| 3 | DESIGN-REST-02 a HUB-VIS-03 | Varios | Recuperar Bonasera 1:1 en theme, vertical y demo, y cerrar el gate con aprobación humana | HUB-VIS-02 | DESIGN-REST-02, DEMO-REST-02A, THEME-REST-04, THEME-REST-05 y REST-02S completos; siguiente: DEMO-REST-02B |
| 4 | YOGA-03 a DEMO-YOGA-05 y BHO-09 | `vicunav-yoga`, `vicunav-bhoga-yoga`, `vicunav-demo-yoga` y `vicunav-theme-core` | Implementar el contrato Yoga aprobado, migrar Bhoga 1:1 y construir una demo pública saneada sobre los paquetes compartidos | HUB-VIS-03, gate del cliente y aprobaciones de corte | Contrato aprobado; dominio y superficies visuales bloqueados |
| 5 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | HUB-VIS-03 | Bloqueado; no autorizado en ejecución |
| 6 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

La recuperación, aceptación y propietario de cada unidad están en el
[plan atómico de fidelidad visual](plan-fidelidad-visual.md). La historia del runtime
permanece en el [plan de restaurante](plan-restaurante.md). La migración del cliente
Bhoga Yoga está desglosada en el [plan específico](plan-bhoga-yoga.md).

## Pista Yoga y Bhoga

La arquitectura separa cuatro propietarios: `vicunav-theme-core` presenta,
`vicunav-yoga` implementa el dominio reusable, `vicunav-bhoga-yoga` conserva la
implementación privada del cliente y `vicunav-demo-yoga` compone una demostración
pública sin identidad ni datos reales de Bhoga.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| YOGA-00 a YOGA-01 | `vicunav-hub` y `vicunav-yoga` | Decidir ownership y crear el bootstrap neutral con contrato en borrador | Solicitud del usuario | Completado localmente |
| YOGA-02 | `vicunav-yoga` | Aprobar entidades, capacidades, bloques y exclusiones del contrato v1 | Decisión humana de producto | Completado y publicado en `vicunav-yoga` 1.0.0 |
| YOGA-03 a YOGA-05 | `vicunav-yoga` | Implementar, probar y documentar el dominio reusable sin marca Bhoga | YOGA-02; superficies visuales además dependen de HUB-VIS-03 | Pendiente |
| BHO-00 a BHO-01 | `vicunav-hub` y `vicunav-bhoga-yoga` | Crear la fundación privada, inventarios, prompts y validación | Solicitud del usuario | Completado localmente |
| BHO-02 | `vicunav-bhoga-yoga` | Congelar baseline de tres rutas, estados y viewports | HUB-VIS-03 y gate del cliente | Bloqueado |
| BHO-03 a BHO-07 | Varios | Inventariar, componer theme y plugins, migrar y aprobar QA integral | BHO-02 y YOGA-02 | Pendiente |
| BHO-08 | Proyecto e infraestructura autorizada | Crear respaldo Elementor privado y ensayar rollback | BHO-07 | Pendiente |
| BHO-09 | Infraestructura autorizada | Reemplazar producción y observar el corte | BHO-08 y aprobación humana explícita | Pendiente |
| DEMO-YOGA-01 | `vicunav-demo-yoga` | Crear la fundación local de la demo pública saneada | Decisión de arquitectura | Completado localmente |
| DEMO-YOGA-02 a DEMO-YOGA-05 | `vicunav-demo-yoga` | Crear LocalWP, contenido ficticio, composición y gate público | YOGA-02; superficies visuales además dependen de HUB-VIS-03 | Pendiente |
| TOOL-YOGA-01 | `vicunav-transform-claude-to-gutenberg` | Capturar aprendizajes de la migración Bhoga y optimizar prompts, validadores y consumo de contexto con regresiones | BHO-07 y evidencia aprobada | Pendiente |

## Pista paralela de diseño

Esta pista no bloqueó REST-01. Ahora puede avanzar cuando cada paquete visual haya
sido aprobado y adjuntado a su repositorio o proyecto de referencia. Las auditorías
de prototipos se ejecutan con `transform-claude-to-gutenberg`, sin convertir el skill
en una dependencia de los repositorios resultantes.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| INFO-01 | `vicunav-demo-informativo` | Auditar el HTML aprobado de Dra. Fortul y clasificar tokens, patterns, capacidades compartidas, composición y requisitos médicos | HUB-VIS-03 y handoff aprobado | Bloqueado por recuperación visual |
| INFO-02 | `vicunav-demo-informativo` | Decidir saneamiento, nombre, privacidad y transferencia del repositorio de referencia | INFO-01 | Requiere decisión humana |
| INFO-03 | Varios | Crear Issues atómicos en cada repositorio propietario e implementar el demo informativo | INFO-01 | Por descomponer |
| DESIGN-HOTEL-01 | Varios | Auditar el diseño aprobado de hotel y separar presentación, plugin core, pagos, dominio y composición | HUB-VIS-03 y handoff aprobado | Bloqueado por recuperación visual |

## Pendientes y riesgos

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
- La paleta global final de Vicunav sigue pendiente, pero no bloquea `plugin-core`,
  pagos ni la variación Bonasera aislada.
- Los diseños de restaurante, hotel y Dra. Fortul pueden descubrir funcionalidades,
  pero un elemento visual no define por sí solo un contrato de backend. Antes de crear
  lógica se deben precisar estado, datos, permisos, errores y repositorio propietario.
- Bhoga Yoga contiene identidad, fotografías y testimonios de personas reales. Los
  derechos de uso en el proyecto privado fueron confirmados; la media se incorporará
  solo dentro de ese alcance y con su procedencia documentada. `vicunav-yoga` no puede
  incorporar esos datos ni decisiones de marca, y `vicunav-demo-yoga` solo usará
  contenido ficticio o expresamente autorizado. El live no se modifica antes de un
  staging Elementor privado, un backup inmutable y un rollback ensayado.
- `vicunav-plugin-core` declara compatibilidad mínima con WordPress 6.6 y PHP 8.1. Su
  matriz CI específica para esas versiones es una mejora futura y no bloquea
  REST-01.

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
