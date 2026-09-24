# ADR 0002: Pagos como motor independiente

Estado: la parte de motor compartido entre verticales está sustituida por el [ADR 0015](0015-un-theme-y-un-plugin-por-proyecto.md) (2026-09-24).


## Contexto

Se observó que no todos los proyectos Vicunav necesitaban cobrar; por ejemplo, un sitio
de servicios podía operar sin transacciones. Al mismo tiempo, ubicar la lógica de
checkout dentro de cada vertical habría repetido su implementación entre hotel y
restaurante.

## Decisión

Se decidió implementar `vicunav-pagos` como un plugin independiente, sin conocimiento
de reservas ni pedidos. La relación con cada operación se resolvió mediante una
`referencia_externa` polimórfica. Los plugins verticales declararon `vicunav-pagos` como
dependencia mediante el mecanismo `Requires Plugins`.

## Consecuencias

Un tercer vertical futuro podría reutilizar `vicunav-pagos` sin modificarlo. Un sitio
sin transacciones simplemente no instalaría el plugin.
