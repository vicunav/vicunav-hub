# ADR 0009: Comercio de restaurante sin WooCommerce

## Estado

Decidida el 2026-08-16. Su aplicación comienza con `REST-02`.

## Contexto

El prototipo Bonasera modeló menú, carrito, pedidos, pizzas y reservas como estado de
una SPA. Su handoff posterior recomendó trasladar comercio a WooCommerce, pero esa
recomendación precede la decisión de producto y los contratos reales ya publicados
por el ecosistema.

`vicunav-pagos` 0.3.0 ya ofrece solicitudes de pago idempotentes, estados, proveedor
manual y eventos públicos sin conocer el dominio que origina el cobro. Convertirlo en
propietario de pedidos rompería esa independencia. Adoptar WooCommerce en v1, por su
parte, delegaría las entidades principales del vertical a un runtime que no forma
parte de los contratos Vicunav y obligaría a adaptar el constructor de pizzas, las
zonas de entrega y los totales a su modelo.

## Alternativas consideradas

1. Usar WooCommerce para productos, carrito, checkout y pedidos, con un adaptador hacia
   `vicunav-pagos`.
2. Implementar menú, carrito y pedidos en `vicunav-restaurante`, consumiendo el
   contrato público de `vicunav-pagos`.
3. Ampliar `vicunav-pagos` para que también posea pedidos y checkout del restaurante.

La primera alternativa aporta un motor de comercio general, pero añade una dependencia
amplia y obliga a representar reglas específicas del vertical mediante extensiones. La
tercera mezcla el origen comercial con el ciclo de vida del pago e impide que pagos
siga siendo reutilizable por otros verticales.

## Decisión

La v1 adopta la segunda alternativa.

La decisión de producto fue confirmada por el usuario, que conserva la autoridad
final sobre producto y cambios externos.

- `vicunav-restaurante` será propietario del menú estructurado, ingredientes,
  disponibilidad, carrito, pedidos, estados, totales, delivery, reservas y reacción a
  eventos de pago.
- `vicunav-pagos` seguirá siendo propietario únicamente de solicitudes de pago y su
  ciclo de vida. Cada pedido se enlazará mediante `external_type = vicu_order` y un
  `external_id` opaco.
- El checkout v1 usará el proveedor manual real publicado por `vicunav-pagos`. La
  presentación, las instrucciones del comercio y la evidencia externa pertenecerán al
  vertical o al sitio, nunca al motor de pagos.
- No se portarán los cuatro métodos de pago simulados ni la línea de estado teatral del
  prototipo legacy.
- WooCommerce no será dependencia de v1. Una incorporación posterior requerirá otro
  ADR, un contrato de coexistencia o migración y un adaptador explícito.

La especificación coordinadora de esta decisión está en
[`docs/especificaciones/restaurante-v1.md`](../especificaciones/restaurante-v1.md).
Cuando exista `vicunav-restaurante`, ese repositorio será la fuente de verdad de su
contrato público y su comportamiento ejecutable.

## Consecuencias

- El vertical debe implementar y probar su propia persistencia transaccional para
  carrito, pedidos, reservas e idempotencia.
- Los importes se calculan en servidor y se congelan como snapshots del pedido. El
  cliente nunca es autoridad de precio.
- La integración con pagos debe tolerar reintentos, eventos duplicados y fallos entre
  transacciones de plugins distintos. Eventos públicos y reconciliación periódica
  forman parte del contrato.
- `vicunav-theme-core` no recibe lógica de comercio. Los bloques interactivos viven en
  el plugin del vertical y la composición Bonasera vive en el futuro demo.
- La recomendación WooCommerce de los documentos del prototipo queda conservada como
  evidencia histórica, pero no gobierna la implementación Vicunav.
- La decisión aumenta el alcance de `REST-02`, a cambio de mantener un dominio pequeño,
  explícito y reutilizable sin acoplarlo a un motor de comercio general.

## Propagación y verificación

1. Crear `vicunav-restaurante` desde la plantilla canónica.
2. Publicar allí el contrato v1 antes de exponer endpoints o hooks consumibles.
3. Implementar los issues atómicos definidos en el
   [plan de ejecución](../handoff/plan-restaurante.md).
4. Verificar la integración contra las versiones reales de `vicunav-plugin-core` y
   `vicunav-pagos`, incluida la recuperación por reconciliación.
5. Crear `vicunav-demo-restaurante` solo cuando existan las capacidades que debe
   componer.
