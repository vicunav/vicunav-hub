# ADR 0003: Contratos frontera y eventos

## Contexto

Se observó que, durante el desarrollo asistido por IA, la ausencia de una frontera
explícita llevaba a que un vertical terminara acoplado a los detalles internos de otro
plugin.

## Decisión

Se definieron tres contratos frontera: `theme-core`, `plugin-core` y pagos. Cada contrato
debía documentarse antes de escribir el código que dependiera de él. Sus documentos se
referenciaron provisionalmente por nombre y sus enlaces se completarían cuando
existieran los repositorios correspondientes:

- `vicunav-theme-core-contract.md`
- `vicunav-plugin-core-contract.md`
- `vicunav-pagos-contract.md`

Se decidió que los verticales reaccionarían a hooks públicos, como
`vicu_pagos_confirmado`, y nunca leerían directamente la base de datos de otro plugin.

## Consecuencias

Un contrato podría evolucionar mediante versiones, por ejemplo `v1.1`, sin romper a
quienes lo consumieran, siempre que su forma pública no cambiara.
