# ADR 0006: Restaurante primero

## Contexto

Ambos verticales estaban listos para comenzar y se debía elegir su orden. Por un lado,
`vicunav-restaurante` ofrecía un ingreso local más rápido y un modelo de datos más
simple. Por otro, `vicunav-hotel` aportaba mayor peso al portafolio por la exigencia de
gestionar disponibilidad y prevenir reservas dobles.

## Decisión

Se priorizó `vicunav-restaurante` para validar el negocio local cuanto antes.
`vicunav-hotel` quedó como el vertical de mayor exigencia técnica para una etapa
posterior.

## Consecuencias

El `spec` interno de `vicunav-hotel` se escribiría cuando llegara su turno, no antes.
Evitar trabajo especulativo mantuvo una regla que ya se aplicaba desde
`vicunav-plugin-core`.

Se dejó explícito que la decisión era revisitable sin costo. No comprometía los
contratos frontera de `vicunav-theme-core`, `vicunav-plugin-core` ni `vicunav-pagos`,
que eran agnósticos al orden de los verticales.
