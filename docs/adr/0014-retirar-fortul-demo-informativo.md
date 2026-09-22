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
Además, `vicunav-demo-informativo` se retira por completo del mapa del
ecosistema — no queda como concepto planeado a la espera de otra referencia.

## Consecuencias

- `~/Documents/Codex/drafortul/` deja de estar vinculado al ecosistema. El
  intento de adopción de `vicunav-theme-core` (sin commitear desde
  2026-08-06) se descartó localmente mediante `git stash` — recuperable, no
  se borró del historial — y el proyecto vuelve a su propio theme.
- `vicunav-demo-informativo` se elimina del diagrama de arquitectura, la
  tabla de repositorios y el estado canónico. INFO-01 a INFO-03 se retiran
  del backlog: no hay origen de HTML aprobado y ninguna decisión pendiente
  depende de ellos.
- Si el usuario retoma esta idea más adelante, será como un concepto nuevo
  — otro naming, otro contexto — y no como continuación de este; no debe
  asumirse ninguna relación con lo descrito aquí o en ADR 0007.
- ADR 0007 queda superado por este documento y se conserva sin editar como
  registro histórico.
