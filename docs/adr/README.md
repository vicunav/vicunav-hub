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
| [0007](0007-demo-informativo-theme-base.md) | Validar el theme base con un demo informativo |

## Cuándo crear otro ADR

Se crea un ADR cuando una decisión cambia límites entre paquetes, dependencias,
contratos públicos, estructura del ecosistema o una restricción técnica difícil de
revertir. Prioridades operativas, tareas y defectos se registran en el backlog o en el
issue propietario, no como ADR.

Un ADR nuevo debe incluir contexto, decisión y consecuencias. Si reemplaza otro ADR,
debe enlazarlo y declarar explícitamente que lo sustituye; el documento anterior no se
borra.
