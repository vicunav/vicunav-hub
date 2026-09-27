# Backlog

Actualizado: 2026-09-24.

Cada entrada pertenece a un repositorio y se ejecuta con un issue, una rama, un pull
request y un squash-merge. Cuando existe el issue, este es la fuente de su estado y
aceptación.

## `vicunav-restaurante`

- Cerrar el checkpoint de fidelidad visual 1:1 de sus nueve rutas (`/`, `/menu/`,
  `/pizzas/`, `/carrito/`, `/checkout/`, `/reservas/`, `/mis-pizzas/`, `/pedido/` y
  `/privacidad/`) según el [ADR 0005](adr/0005-fidelidad-visual-bloqueante.md):
  matriz por página, estado y viewport con capturas lado a lado y overlays, diferencias
  registradas y aprobación humana explícita.

## `vicunav-hub`, `vicunav-standards` y `vicunav-repo-template`

- Sincronización de submódulos: cuando `vicunav-standards` publique un cambio, avanzar
  `docs/standards` en el hub, la plantilla y los repositorios de proyecto, cada uno en
  su propio issue, y verificar con `git submodule status` (ver
  [gobernanza](gobernanza.md)).
- Mantener este backlog y el [estado](estado.md) al cerrar cada fase.

## Reglas de mantenimiento

- Añadir trabajo solo cuando tenga propietario, dependencia y aceptación observables.
- Eliminar el detalle de una tarea cuando su issue se cierre y reflejar el resultado en
  el estado.
