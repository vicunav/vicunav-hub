# Registros de decisiones de arquitectura

Esta carpeta conserva las decisiones arquitectónicas aceptadas del ecosistema Vicunav.
Un ADR explica por qué existe una decisión y sus consecuencias; el estado actual y el
trabajo pendiente se mantienen en `docs/handoff/`.

## Decisiones vigentes

| ADR | Decisión |
| --- | --- |
| [0001](0001-separacion-theme-plugins.md) | Separar presentación y lógica de negocio |
| [0002](0002-pagos-motor-independiente.md) | Implementar pagos como motor independiente |
| [0003](0003-contratos-y-eventos.md) | Integrar paquetes mediante contratos y eventos |
| [0004](0004-estructura-de-repos.md) | Mantener repositorios y prefijos independientes |
| [0005](0005-acf-genuino-solo-campos.md) | Usar ACF genuino únicamente para campos editoriales |
| [0006](0006-restaurante-primero.md) | Construir restaurante antes que hotel |
| [0009](0009-restaurante-sin-woocommerce.md) | Implementar comercio de restaurante sin WooCommerce |
| [0010](0010-fidelidad-visual-bloqueante.md) | Bloquear migraciones hasta demostrar fidelidad visual 1:1 |
| [0011](0011-bhoga-yoga-cliente-privado.md) | Mantener Bhoga Yoga como implementación privada de cliente |
| [0013](0013-theme-core-dinamico-agnostico.md) | Compartir un theme-core dinámico, agnóstico y sin child themes por defecto (sustituido por 0015) |
| [0015](0015-un-theme-y-un-plugin-por-proyecto.md) | Un theme y un plugin propios por proyecto; sin theme, pagos ni core compartidos |

Los números 0007, 0008, 0012 y 0014 no se reutilizan.

## Cuándo crear otro ADR

Se crea un ADR cuando una decisión cambia límites entre paquetes, dependencias,
contratos públicos, estructura del ecosistema o una restricción técnica difícil de
revertir. Prioridades operativas, tareas y defectos se registran en el backlog o en el
issue propietario, no como ADR.

Un ADR nuevo debe incluir contexto, decisión y consecuencias. Si reemplaza otro ADR,
debe enlazarlo y declarar explícitamente que lo sustituye; el documento anterior no se
borra.
