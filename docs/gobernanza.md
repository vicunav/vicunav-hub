# Gobernanza de decisiones

Este documento define cómo se decide y se propaga un cambio que afecta a uno o varios
repositorios Vicunav. El hub registra la decisión; cada repositorio es dueño de su
código, pruebas y documentación específica.

## Autoridad

- El usuario conserva la decisión final de producto y de cualquier acción destructiva,
  irreversible o con impacto externo.
- `vicunav-standards` es la única fuente normativa de las reglas técnicas compartidas.
- Cada repositorio es la fuente de verdad de su comportamiento ejecutable.

## Cómo se decide y se propaga un cambio

1. **Decidir:** una decisión de arquitectura o de límites se registra en un ADR
   ([`docs/adr/`](adr/)). Una prioridad operativa se registra en el
   [backlog](backlog.md) y el [estado](estado.md).
2. **Propagar:** cada repositorio afectado recibe un issue atómico, una rama, un pull
   request y un squash-merge. No se mezclan varios repositorios en un mismo commit.
3. **Verificar:** se comprueba el resultado en cada repositorio afectado y, cuando todos
   están cerrados, se actualizan el estado y el backlog del hub.

## Estándares y submódulo

Las reglas transversales viven en `vicunav-standards` y cada repositorio las consume como
submódulo en `docs/standards/`. Para actualizarlo:

1. Publicar el cambio en `vicunav-standards` mediante su propio issue y PR.
2. En cada repositorio, en un issue propio, avanzar el submódulo al nuevo commit:

   ```bash
   git submodule update --remote docs/standards
   git add docs/standards
   ```

3. Confirmar con `git submodule status` que todos apuntan al commit vigente de
   `vicunav-standards`.

No se copia el mismo Markdown entre repositorios: se enlaza o se usa el submódulo.

## Qué es fuente de verdad

| Cambio | Fuente de verdad |
| --- | --- |
| Arquitectura o límite entre theme y plugin | ADR del hub |
| Convención técnica transversal | `vicunav-standards` |
| Comportamiento, API pública o pruebas de un proyecto | Repositorio del proyecto |
| Estado y prioridades | Hub (`estado.md`, `backlog.md`) |

## Fidelidad visual

Estado funcional y estado visual se registran por separado. Una migración de diseño solo
se cierra con paridad 1:1 demostrada y aprobada por una persona, según el
[ADR 0005](adr/0005-fidelidad-visual-bloqueante.md) y
`docs/standards/docs/visual-fidelity.md`.

## Contenido mínimo de un ADR

Contexto, decisión, consecuencias, fuente de verdad y repositorios afectados. Se
numera de forma consecutiva y describe siempre la decisión vigente.
