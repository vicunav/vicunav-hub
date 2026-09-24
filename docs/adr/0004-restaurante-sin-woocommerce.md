# ADR 0004: Comercio de restaurante sin WooCommerce

## Contexto

El proyecto de restaurante modela menú, carrito, pedidos, pizzas personalizadas y
reservas. WooCommerce ofrecería un motor de comercio general, pero delegaría las
entidades principales a un runtime externo y obligaría a adaptar el constructor de
pizzas, las zonas de entrega y los totales a su modelo.

## Alternativas consideradas

1. Usar WooCommerce para productos, carrito, checkout y pedidos.
2. Implementar el comercio dentro del plugin del propio proyecto, con el ciclo de vida
   del pago como módulo interno.

## Decisión

Se adopta la segunda alternativa. El plugin de `vicunav-restaurante` es propietario de
menú estructurado, ingredientes, disponibilidad, carrito, pedidos, estados, totales,
delivery, reservas y pagos manuales, organizados en módulos internos (dominio
restaurante, pagos y capacidades compartidas).

- El servidor es la única autoridad de precios, disponibilidad, capacidad y estados; los
  importes son enteros en unidad menor y se congelan como snapshots del pedido.
- El checkout usa el proveedor de pago manual del propio plugin. Las instrucciones del
  comercio y la evidencia externa son privadas del plugin.
- WooCommerce no es dependencia. Incorporarlo exigiría un nuevo ADR.

## Consecuencias

- El plugin implementa y prueba su propia persistencia transaccional, idempotencia y
  recuperación ante fallos.
- El theme no recibe lógica de comercio; los bloques interactivos viven en el plugin.
- Las reglas de negocio detalladas y la API pública se documentan y prueban en
  `vicunav-restaurante`, que es su fuente de verdad.
