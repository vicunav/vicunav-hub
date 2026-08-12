# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-12.

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
| `vicunav-hub` | Siete ADRs, gobierno, estado y backlog consolidados |
| `vicunav-theme-core` | Base 0.1.0 completa; issues 1 al 29 cerrados y sin PRs abiertos |
| `vicunav-plugin-core` | Fase fundacional CORE-01 a CORE-09 completa; contrato 1.0.0 y plugin 0.1.0 verificados |
| Referencia privada de `vicunav-demo-informativo` | Dra. Fortul conserva estrategia y contenido; WordPress local quedó limpio y consume `vicunav-theme-core` |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | PAGOS-01 | `vicunav-pagos` | Crear repositorio, contrato y CPT `vicu_payment_req` | CORE-03, CORE-08, CORE-09 | Siguiente |
| 2 | PAGOS-02 | `vicunav-pagos` | Implementar estados, expiración y eventos públicos | PAGOS-01 | Por crear |
| 3 | PAGOS-03 | `vicunav-pagos` | Implementar proveedor manual v1 | PAGOS-02 | Por crear |
| 4 | REST-01 | `vicunav-restaurante` | Escribir spec durable y descomponerlo en issues | CORE-02, PAGOS-02 | Por crear |
| 5 | REST-02 | `vicunav-restaurante` | Crear repo e implementar menú y pedidos | REST-01 | Por descomponer |
| 6 | DEMO-REST-01 | `vicunav-demo-restaurante` | Versionar la composición del demo LocalWP | REST-02, PAGOS-03 | Por descomponer |
| 7 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | DEMO-REST-01 | Diferido por ADR 0006 |
| 8 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

## Pista paralela de diseño

Esta pista no bloquea PAGOS-01. Comienza cuando cada paquete visual haya sido aprobado
y adjuntado a su repositorio o proyecto de referencia.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| INFO-01 | `vicunav-demo-informativo` | Auditar el HTML aprobado de Dra. Fortul y clasificar tokens, patterns, capacidades compartidas, composición y requisitos médicos | Handoff aprobado | Esperando diseño |
| INFO-02 | `vicunav-demo-informativo` | Decidir saneamiento, nombre, privacidad y transferencia del repositorio de referencia | INFO-01 | Requiere decisión humana |
| INFO-03 | Varios | Crear Issues atómicos en cada repositorio propietario e implementar el demo informativo | INFO-01 | Por descomponer |
| DESIGN-REST-01 | Varios | Auditar el diseño aprobado de restaurante y separar presentación, plugin core, pagos, dominio y composición | Handoff aprobado | Esperando diseño |
| DESIGN-HOTEL-01 | Varios | Auditar el diseño aprobado de hotel y separar presentación, plugin core, pagos, dominio y composición | Handoff aprobado | Esperando diseño |

## Siguiente issue: PAGOS-01

### Objetivo

Crear el repositorio público `vicunav-pagos`, versionar su contrato inicial e
implementar el CPT `vicu_payment_req` sobre las capacidades publicadas por
`vicunav-plugin-core`.

### Alcance

- Crear el repositorio bajo la organización Vicunav usando el template vigente.
- Clonarlo en `~/Documents/Codex/vicunav/vicunav-pagos`.
- Definir el contrato del CPT `vicu_payment_req` y su referencia externa polimórfica.
- Definir las fronteras con `vicunav-plugin-core` y los futuros verticales.
- Incorporar bootstrap, pruebas y CI reproducibles antes de implementar transiciones.
- Registrar el CPT con capabilities y exposición REST coherentes con el contrato.

### Fuera de alcance

- Implementar la máquina de estados completa.
- Implementar expiración o idempotencia operativa.
- Implementar eventos de confirmación, rechazo o expiración.
- Implementar el proveedor manual o la integración Mercantil.
- Implementar lógica de pedidos o reservas.

### Criterios de aceptación

- [ ] El repo existe en `github.com/vicunav/vicunav-pagos` y es público.
- [ ] No quedan placeholders del template.
- [ ] El README público está en inglés y explica responsabilidades y límites.
- [ ] La documentación interna está en español.
- [ ] El submódulo de estándares apunta a su versión vigente.
- [ ] El contrato documenta persistencia, referencia externa, permisos y REST.
- [ ] `vicu_payment_req` se registra y cuenta con pruebas.
- [ ] El plugin se activa y desactiva sin errores en WordPress.
- [ ] La rama `main` solo admite squash-merge mediante pull request.
- [ ] El working tree queda limpio después del merge.

### Validación

- Ejecutar lint, WPCS, PHPCompatibilityWP y pruebas automatizadas.
- Verificar activación real y registro del CPT en WordPress.
- Validar REST y permisos contra el contrato aprobado.
- Inspeccionar la estructura local, el submódulo y la configuración pública del repo.

## Pendientes que todavía requieren especificación

- El contrato de pagos debe cerrar persistencia, transiciones, payloads de hooks,
  expiración e idempotencia antes de PAGOS-02.
- El dominio de restaurante todavía no tiene un spec durable versionado. REST-01 debe
  definir estados del pedido, totales, disponibilidad, permisos, endpoints y pruebas
  antes de crear issues de implementación.
- La paleta final de marca sigue pendiente, pero no bloquea `plugin-core` ni pagos.
- Los diseños de restaurante, hotel y Dra. Fortul pueden descubrir funcionalidades,
  pero un elemento visual no define por sí solo un contrato de backend. Antes de crear
  lógica se deben precisar estado, datos, permisos, errores y repositorio propietario.
- `vicunav-plugin-core` declara compatibilidad mínima con WordPress 6.6 y PHP 8.1. Su
  matriz CI específica para esas versiones es una mejora futura y no bloquea pagos.

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
