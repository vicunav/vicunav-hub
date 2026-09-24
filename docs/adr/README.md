# Registros de decisiones de arquitectura

Esta carpeta contiene las decisiones arquitectónicas vigentes de Vicunav. Un ADR explica
por qué existe una decisión y sus consecuencias; el estado actual y el trabajo pendiente
están en [`../estado.md`](../estado.md) y [`../backlog.md`](../backlog.md).

## Decisiones vigentes

| ADR | Decisión |
| --- | --- |
| [0001](0001-separacion-theme-plugins.md) | Separar presentación (theme) y lógica (plugin) dentro de cada proyecto |
| [0002](0002-estructura-de-repos.md) | Un repositorio por proyecto, prefijo `vicunav-` e ids internos `vicu_` |
| [0003](0003-acf-genuino-solo-campos.md) | Usar ACF genuino únicamente para campos editoriales |
| [0004](0004-restaurante-sin-woocommerce.md) | Implementar comercio de restaurante sin WooCommerce |
| [0005](0005-fidelidad-visual-bloqueante.md) | Bloquear migraciones hasta demostrar fidelidad visual 1:1 |
| [0006](0006-bhoga-yoga-cliente-privado.md) | Mantener Bhoga Yoga como implementación privada de cliente |

## Cuándo crear otro ADR

Se crea un ADR cuando una decisión cambia límites entre theme y plugin, la estructura de
repositorios o una restricción técnica difícil de revertir. Prioridades operativas,
tareas y defectos se registran en el backlog o en el issue propietario.

Un ADR nuevo incluye contexto, decisión y consecuencias, y toma el siguiente número
consecutivo. Si una decisión cambia, se reescribe el ADR correspondiente para describir
la decisión actual.
