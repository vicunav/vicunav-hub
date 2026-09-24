# ADR 0003: Contratos frontera y eventos

## Contexto

Se observó que, durante el desarrollo asistido por IA, la ausencia de una frontera
explícita llevaba a que un vertical terminara acoplado a los detalles internos de otro
plugin.

## Decisión

Se definieron dos contratos frontera: `vicunav-theme-core` y `vicunav-pagos`. Este
último distribuye también las capacidades base compartidas `Vicu\Core` (contrato
1.0.0), con su propio contrato versionado. Cada contrato debe versionarse en el repositorio propietario antes de
escribir el código que dependa de él.

El contrato vigente de theme está en
[`vicunav-theme-core/docs/contrato-publico.md`](https://github.com/vicunav/vicunav-theme-core/blob/main/docs/contrato-publico.md).
El contrato vigente de `vicunav-pagos` y de `Vicu\Core` está en
[`vicunav-pagos/docs/contrato-publico.md`](https://github.com/vicunav/vicunav-pagos/blob/main/docs/contrato-publico.md);
el [estado canónico](../handoff/estado-ecosistema.md) resume sus capacidades sin
sustituir ese contrato propietario.

Se decidió que los verticales reaccionarían a hooks públicos, como
`vicu_pagos_confirmado`, y nunca leerían directamente la base de datos de otro plugin.

## Consecuencias

Un contrato puede evolucionar mediante versiones, por ejemplo `v1.1`, sin romper a
quienes lo consumen mientras su forma pública se mantenga compatible. El hub conserva
la decisión y las dependencias; cada repo conserva la definición técnica ejecutable.
