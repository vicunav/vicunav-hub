# ADR 0003: Contratos frontera y eventos

## Contexto

Se observó que, durante el desarrollo asistido por IA, la ausencia de una frontera
explícita llevaba a que un vertical terminara acoplado a los detalles internos de otro
plugin.

## Decisión

Se definieron tres contratos frontera: `vicunav-theme-core`, `vicunav-plugin-core` y
`vicunav-pagos`. Cada contrato debe versionarse en el repositorio propietario antes de
escribir el código que dependa de él.

El contrato vigente de theme está en
[`vicunav-theme-core/docs/contrato-publico.md`](https://github.com/vicunav/vicunav-theme-core/blob/main/docs/contrato-publico.md).
Mientras `vicunav-plugin-core` y `vicunav-pagos` no existan, sus decisiones confirmadas
se mantienen en el
[estado canónico](../handoff/estado-ecosistema.md) y su formalización aparece como paso
obligatorio en el [backlog](../handoff/backlog-ecosistema.md). Esos resúmenes no
sustituyen los futuros contratos propietarios.

Se decidió que los verticales reaccionarían a hooks públicos, como
`vicu_pagos_confirmado`, y nunca leerían directamente la base de datos de otro plugin.

## Consecuencias

Un contrato puede evolucionar mediante versiones, por ejemplo `v1.1`, sin romper a
quienes lo consumen mientras su forma pública se mantenga compatible. El hub conserva
la decisión y las dependencias; cada repo conserva la definición técnica ejecutable.
