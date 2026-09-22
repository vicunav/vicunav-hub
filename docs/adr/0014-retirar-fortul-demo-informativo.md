# ADR 0014: Retirar a Dra. Fortul como referencia del demo informativo

## Contexto

ADR 0007 adoptó el proyecto privado de Dra. Fortul como implementación de
referencia de `vicunav-demo-informativo`, con su LocalWP consumiendo
`vicunav-theme-core` en lugar de un theme propio. Esa migración nunca se
completó: los cambios quedaron sin commitear en el repositorio del proyecto
desde el 2026-08-06 y no se retomaron en seis semanas. El usuario decidió el
2026-09-22 discontinuar esa relación.

## Decisión

Se revierte ADR 0007. El proyecto de Dra. Fortul deja de ser la
implementación de referencia de `vicunav-demo-informativo` y vuelve a ser un
proyecto de cliente independiente, sin relación con el ecosistema Vicunav.
`vicunav-demo-informativo` queda sin implementación de referencia asignada.

## Consecuencias

- `~/Documents/Codex/drafortul/` deja de estar vinculado al ecosistema. El
  intento de adopción de `vicunav-theme-core` (sin commitear desde
  2026-08-06) se descartó localmente mediante `git stash` — recuperable, no
  se borró del historial — y el proyecto vuelve a su propio theme.
- `vicunav-demo-informativo` permanece en el mapa del ecosistema como
  concepto planeado, pero sin implementación de referencia. INFO-01 a
  INFO-03 se retiran del backlog: no hay origen de HTML aprobado y ninguna
  decisión pendiente depende de ellos.
- ADR 0007 queda superado por este documento y se conserva sin editar como
  registro histórico.
