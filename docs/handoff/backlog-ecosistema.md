# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-16.

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
| `vicunav-hub` | Nueve ADRs, spec durable de restaurante, gobierno, estado y backlog consolidados |
| `vicunav-transform-claude-to-gutenberg` | Skill 0.1.0 público con auditor de proyectos frontend, validador FSE, pruebas, CI y flujo seguro para LocalWP |
| `vicunav-theme-core` | Base 0.1.0 completa; issues 1 al 29 cerrados y sin PRs abiertos |
| `vicunav-plugin-core` | Fase fundacional CORE-01 a CORE-09 completa; contrato 1.0.0, plugin 0.1.0 y release `v0.1.0` publicados |
| `vicunav-pagos` | PAGOS-01 a PAGOS-03 completos; plugin y contrato 0.3.0 con persistencia transaccional, proveedor manual idempotente, estados, expiración, eventos versionados, pruebas, E2E real y CI |
| `vicunav-restaurante` | REST-02A a REST-02M completos; plugin 0.13.0, contrato 1.0.0, dominio backend y bloque dinámico de menú completos, sin bloques transaccionales ni contenido de demo |
| Referencia de diseño Bonasera | DESIGN-REST-01 auditó el commit `1e1f62787e088c0ca9701500e764802499d1b253`, sus siete pantallas, reglas, contratos propuestos, tokens y defectos; REST-01 incorporó el resultado sin aceptar su mapeo legacy a WooCommerce |
| Referencia privada de `vicunav-demo-informativo` | Dra. Fortul conserva estrategia y contenido; WordPress local quedó limpio y consume `vicunav-theme-core` |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | REST-02Q a REST-02R | `vicunav-restaurante` | Implementar cuenta y la validación de release en 2 issues dependientes | REST-02P | REST-02Q siguiente |
| 2 | DEMO-REST-01A a DEMO-REST-01D | `vicunav-demo-restaurante` | Crear repo, contenido licenciado, composición FSE y QA del demo | REST-02R, THEME-REST-03 | Planificado |
| 3 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | DEMO-REST-01D | Diferido por ADR 0006 |
| 4 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

La secuencia, aceptación y propietario de cada unidad están en el
[plan atómico de restaurante](plan-restaurante.md).

## Pista paralela de diseño

Esta pista no bloqueó REST-01. Ahora puede avanzar cuando cada paquete visual haya
sido aprobado y adjuntado a su repositorio o proyecto de referencia. Las auditorías
de prototipos se ejecutan con `transform-claude-to-gutenberg`, sin convertir el skill
en una dependencia de los repositorios resultantes.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| INFO-01 | `vicunav-demo-informativo` | Auditar el HTML aprobado de Dra. Fortul y clasificar tokens, patterns, capacidades compartidas, composición y requisitos médicos | Handoff aprobado | Esperando diseño |
| INFO-02 | `vicunav-demo-informativo` | Decidir saneamiento, nombre, privacidad y transferencia del repositorio de referencia | INFO-01 | Requiere decisión humana |
| INFO-03 | Varios | Crear Issues atómicos en cada repositorio propietario e implementar el demo informativo | INFO-01 | Por descomponer |
| THEME-REST-01 | `vicunav-theme-core` | Crear una style variation Bonasera sin cambiar defaults globales | DESIGN-REST-01 | Ejecutable en paralelo |
| THEME-REST-02 | `vicunav-theme-core` | Corregir o añadir header/footer reutilizable con responsive y accesibilidad verificados | DESIGN-REST-01 | Planificado |
| THEME-REST-03 | `vicunav-theme-core` | Incorporar solo los patterns editoriales reutilizables que falten | THEME-REST-01 | Planificado |
| DESIGN-HOTEL-01 | Varios | Auditar el diseño aprobado de hotel y separar presentación, plugin core, pagos, dominio y composición | Handoff aprobado | Esperando diseño |

## Pendientes y riesgos

- REST-01 fijó el dominio sin WooCommerce, estados, totales, disponibilidad, permisos,
  endpoints y pruebas. REST-02A a REST-02L implementaron el dominio backend y
  REST-02M añadió el bloque de menú, REST-02N el constructor de pizzas y REST-02O
  carrito, checkout manual y estado de pedido; REST-02P incorporó reservas públicas.
  Permanecen pendientes el bloque REST-02Q y el gate integral REST-02R; no se marcan
  como completados antes de sus issues propios.
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
