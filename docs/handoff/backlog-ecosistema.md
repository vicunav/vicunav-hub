# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-20.

## Propósito

Este archivo ordena únicamente el trabajo pendiente que cruza repositorios. Cuando
exista un issue en GitHub, el issue será la fuente de su estado, alcance y aceptación.
Los identificadores de esta tabla son referencias de planificación, no números de
issue.

## Trabajo completado

| Repositorio | Resultado vigente |
| --- | --- |
| `vicunav-standards` | Siete estándares compartidos publicados; sin issues abiertos |
| `vicunav-repo-template` | Template utilizable con submódulo, AGENTS, contribución, issue atómico y CI |
| `vicunav-hub` | Diez ADRs, spec durable de restaurante, gobierno, estado y backlog consolidados; HUB-VIS-01 reabre el checkpoint visual |
| `vicunav-transform-claude-to-gutenberg` | Skill 0.1.0 público con requisitos visuales documentados; falta cumplimiento mediante manifiesto, evidencia y herramientas de comparación |
| `vicunav-theme-core` | Base 0.1.0 y entregas históricas THEME-REST-01 a 03 fusionadas; su integración Bonasera requiere recuperación y gate visual 1:1 |
| `vicunav-plugin-core` | Fase fundacional CORE-01 a CORE-09 completa; contrato 1.0.0, plugin 0.1.0 y release `v0.1.0` publicados |
| `vicunav-pagos` | PAGOS-01 a PAGOS-03 completos; plugin 0.3.1 y contrato 0.3.0 con persistencia transaccional, proveedor manual idempotente y lectura normalizada de su opción, estados, expiración, eventos versionados, pruebas, E2E real y CI |
| `vicunav-restaurante` | REST-02A a REST-02R completos; plugin y contrato 1.0.0, siete bloques públicos, privacidad nativa, matriz WordPress/PHP y prerelease `v1.0.0-rc.1`; el gate integrado corrigió menú y checkout sin añadir contenido Bonasera ni WooCommerce |
| `vicunav-demo-restaurante` | Ensamblaje, contenido, rutas y flujos reales disponibles; el cierre histórico de DEMO-REST-01D no acreditó fidelidad visual y el checkpoint queda reabierto |
| Referencia de diseño Bonasera | DESIGN-REST-01 auditó el commit `1e1f62787e088c0ca9701500e764802499d1b253`, sus siete pantallas, reglas, contratos propuestos, tokens y defectos; REST-01 incorporó el resultado sin aceptar su mapeo legacy a WooCommerce |
| Referencia privada de `vicunav-demo-informativo` | Dra. Fortul conserva estrategia y contenido; WordPress local quedó limpio y consume `vicunav-theme-core` |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | HUB-VIS-01 | `vicunav-hub` | Registrar decisión, funnel y reapertura del checkpoint visual | Auditoría posterior a DEMO-REST-01D | Documentado mediante issue 87 y PR 88 |
| 2 | STANDARDS-VIS-01 a HUB-VIS-02 | Varios | Endurecer estándar, skill, tooling, plantilla y adopción canónica antes de otra migración | HUB-VIS-01 | Planificado |
| 3 | DESIGN-REST-02 a HUB-VIS-03 | Varios | Recuperar Bonasera 1:1 en theme, vertical y demo, y cerrar el gate con aprobación humana | HUB-VIS-02 | Planificado |
| 4 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | HUB-VIS-03 | Bloqueado; no autorizado en ejecución |
| 5 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

La recuperación, aceptación y propietario de cada unidad están en el
[plan atómico de fidelidad visual](plan-fidelidad-visual.md). La historia del runtime
permanece en el [plan de restaurante](plan-restaurante.md).

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
- La paleta global final de Vicunav sigue pendiente, pero no bloquea `plugin-core`,
  pagos ni la variación Bonasera aislada.
- Los diseños de restaurante, hotel y Dra. Fortul pueden descubrir funcionalidades,
  pero un elemento visual no define por sí solo un contrato de backend. Antes de crear
  lógica se deben precisar estado, datos, permisos, errores y repositorio propietario.
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
