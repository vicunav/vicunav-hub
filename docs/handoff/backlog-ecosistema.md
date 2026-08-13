# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-13.

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
| `vicunav-hub` | Ocho ADRs, gobierno, estado y backlog consolidados |
| `vicunav-transform-claude-to-gutenberg` | Skill 0.1.0 público con auditor de proyectos frontend, validador FSE, pruebas, CI y flujo seguro para LocalWP |
| `vicunav-theme-core` | Base 0.1.0 completa; issues 1 al 29 cerrados y sin PRs abiertos |
| `vicunav-plugin-core` | Fase fundacional CORE-01 a CORE-09 completa; contrato 1.0.0, plugin 0.1.0 y release `v0.1.0` publicados |
| `vicunav-pagos` | PAGOS-01 completo; repo público, contrato 0.1.0, CPT `vicu_payment_req`, metadatos, capabilities, REST protegido, pruebas y CI |
| Referencia privada de `vicunav-demo-informativo` | Dra. Fortul conserva estrategia y contenido; WordPress local quedó limpio y consume `vicunav-theme-core` |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | PAGOS-02 | `vicunav-pagos` | Implementar estados, idempotencia, expiración y eventos públicos | PAGOS-01 | Siguiente |
| 2 | PAGOS-03 | `vicunav-pagos` | Implementar proveedor manual v1 | PAGOS-02 | Por crear |
| 3 | REST-01 | `vicunav-restaurante` | Escribir spec durable y descomponerlo en issues | CORE-02, PAGOS-02 | Por crear |
| 4 | REST-02 | `vicunav-restaurante` | Crear repo e implementar menú y pedidos | REST-01 | Por descomponer |
| 5 | DEMO-REST-01 | `vicunav-demo-restaurante` | Versionar la composición del demo LocalWP | REST-02, PAGOS-03 | Por descomponer |
| 6 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | DEMO-REST-01 | Diferido por ADR 0006 |
| 7 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

## Pista paralela de diseño

Esta pista no bloquea PAGOS-02. Comienza cuando cada paquete visual haya sido aprobado
y adjuntado a su repositorio o proyecto de referencia. Las auditorías de prototipos se
ejecutan con `transform-claude-to-gutenberg`, sin convertir el skill en una dependencia
de los repositorios resultantes.

| ID | Repositorio | Trabajo | Depende de | Estado |
| --- | --- | --- | --- | --- |
| INFO-01 | `vicunav-demo-informativo` | Auditar el HTML aprobado de Dra. Fortul y clasificar tokens, patterns, capacidades compartidas, composición y requisitos médicos | Handoff aprobado | Esperando diseño |
| INFO-02 | `vicunav-demo-informativo` | Decidir saneamiento, nombre, privacidad y transferencia del repositorio de referencia | INFO-01 | Requiere decisión humana |
| INFO-03 | Varios | Crear Issues atómicos en cada repositorio propietario e implementar el demo informativo | INFO-01 | Por descomponer |
| DESIGN-REST-01 | Varios | Auditar el diseño aprobado de restaurante y separar presentación, plugin core, pagos, dominio y composición | Handoff aprobado | Esperando diseño |
| DESIGN-HOTEL-01 | Varios | Auditar el diseño aprobado de hotel y separar presentación, plugin core, pagos, dominio y composición | Handoff aprobado | Esperando diseño |

## Siguiente issue: PAGOS-02

### Objetivo

Implementar el ciclo de vida de las solicitudes de pago sobre la base publicada por
PAGOS-01, con creación idempotente, transiciones atómicas, expiración determinista y
eventos públicos emitidos después de persistir cada cambio.

### Alcance

- Versionar la persistencia de estados y las reglas de transición ya reservadas.
- Publicar un servicio de creación idempotente basado en la referencia externa.
- Definir el error contractual para una colisión de clave con datos incompatibles.
- Persistir transiciones de forma atómica y rechazar cambios inválidos o concurrentes.
- Implementar expiración repetible sin duplicar efectos.
- Versionar el payload de los hooks de creación, confirmación, rechazo y expiración.
- Emitir cada hook únicamente después de confirmar la persistencia.
- Mantener el REST administrativo protegido y la frontera entre plugins en servicios
  y eventos públicos.

### Fuera de alcance

- Implementar el proveedor manual o la integración Mercantil.
- Implementar lógica de pedidos o reservas.
- Implementar checkout, subida de comprobantes o interfaz visual de un vertical.
- Modificar `vicunav-plugin-core` salvo que aparezca un bloqueo contractual separado.

### Criterios de aceptación

- [ ] Repetir una creación con la misma referencia y los mismos datos devuelve la
  misma solicitud sin duplicarla.
- [ ] Reutilizar la referencia con datos incompatibles produce un error explícito sin
  mutación parcial.
- [ ] Una transición permitida persiste el estado una sola vez y emite un único evento
  con payload versionado.
- [ ] Una transición inválida, terminal o concurrente no modifica estado ni emite
  eventos.
- [ ] La expiración puede reintentarse sin duplicar efectos.
- [ ] Los consumidores no necesitan leer post meta ni clases internas.
- [ ] Roles sin capabilities siguen sin acceder a la colección REST administrativa.
- [ ] El contrato, README, pruebas y versión del plugin describen la misma superficie.
- [ ] El plugin se activa y desactiva sin errores en WordPress real.
- [ ] CI pasa y el working tree queda limpio después del squash-merge.

### Validación

- Ejecutar lint, WPCS, PHPCompatibilityWP y la suite completa.
- Cubrir cada transición, estados terminales, colisiones, reintentos y orden de eventos.
- Verificar activación real y un flujo E2E de creación y transición en WordPress.
- Validar que los fallos no dejen persistencia parcial ni hooks duplicados.

## Pendientes que todavía requieren especificación

- PAGOS-02 debe cerrar persistencia, transiciones, payloads de hooks, expiración e
  idempotencia antes de que un vertical consuma pagos.
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
