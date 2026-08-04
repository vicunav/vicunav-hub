# Backlog multirrepositorio de Vicunav

## Propósito

Este archivo indexa el desarrollo restante y conserva el contexto que cruza
repositorios. Cuando exista un issue en GitHub, ese issue es la fuente de su estado de
ejecución; aquí solo se mantiene el orden, las dependencias y el enlace.

## Reglas de atomicidad

- Una entrada pertenece a un repositorio y produce un issue, una rama, un PR y un
  squash-merge.
- No agrupar cambios solo porque se solicitaron en la misma conversación.
- Cada criterio de aceptación debe ser observable y cada validación debe existir.
- Dependencias entre repositorios se expresan con enlaces, no mezclando alcances.
- Antes de crear una entrada, buscar issues abiertos y cerrados para evitar duplicados
  (verificado: 0 issues/PRs abiertos en los 4 repos existentes al momento de este
  traspaso — no hay duplicados que evitar todavía).
- Usar `por crear` únicamente cuando el repositorio o los permisos todavía no permitan
  abrir el issue.

## Índice priorizado

| Orden | Repositorio | Issue | Título | Estado | Depende de |
| --- | --- | --- | --- | --- | --- |
| 1 | `vicunav-theme-core` | Por crear | Corregir color `vicunav-secondary` duplicado | Sin bloqueos | Ninguna |
| 2 | `vicunav-theme-core` | Por crear | Corregir CPT inventado en `plantillas-verticales.md` | Sin bloqueos | Ninguna |
| 3 | *(org, decisión de usuario)* | N/A | Resolver política de idioma (revertir a español o ratificar bilingüe) | Bloqueado por decisión del usuario | Pregunta abierta #2 del estado |
| 4 | `vicunav-plugin-core` | Por crear (repo no existe) | Bootstrap del repo desde el template | Sin bloqueos técnicos | 1 y 2 recomendado primero, no obligatorio |
| 5 | `vicunav-plugin-core` | Por crear | Clase abstracta `Vicu\Core\PostType` | Depende de 4 |
| 6 | `vicunav-plugin-core` | Por crear | CPT `vicu_faq` | Depende de 5 |
| 7 | `vicunav-plugin-core` | Por crear | CPT `vicu_testimonial` | Depende de 5 |
| 8 | `vicunav-plugin-core` | Por crear | `Vicu\Core\Settings` (usar claves ya fijadas: `phone`, `address`, `business_hours`) | Depende de 4 |
| 9 | `vicunav-plugin-core` | Por crear | Menú "Vicunav" + `Settings::register_tab()` | Depende de 8 |
| 10 | `vicunav-plugin-core` | Por crear | Helpers de seguridad | Depende de 4 |
| 11 | `vicunav-plugin-core` | Por crear | Scaffolding REST (`vicu/v1`) | Depende de 4 |
| 12 | `vicunav-pagos` | Por crear (repo no existe) | Bootstrap + CPT `vicu_payment_req` | Depende de 5, 10, 11 |
| 13 | `vicunav-pagos` | Por crear | Máquina de estados + eventos | Depende de 12 |
| 14 | `vicunav-restaurante` | Por crear (repo no existe) | Bootstrap + CPTs `vicu_menu_item`/`vicu_order` | Depende de 5, 13 |
| 15 | `vicunav-hotel` | N/A | Spec interno de hotel | Diferido a propósito (ADR 0006) — no crear todavía |

Detalle completo de las tareas 4-14 (criterios de aceptación, validación exacta,
riesgos): ya redactado en los documentos de planeación
(`vicunav-plugin-core-issues.md`, `vicunav-pagos-contract.md`,
`vicunav-restaurante-spec.md`) — Codex debe leerlos y convertirlos a issues reales de
GitHub uno por uno, no inventar contenido nuevo para ellos.

## Ficha obligatoria — entradas 1 y 2 (las únicas sin bloqueos ahora mismo)

### vicunav/vicunav-theme-core#N — Corregir color `vicunav-secondary` duplicado

- Estado: por crear
- Objetivo: que `vicunav-secondary` tenga un valor hexadecimal distinto de
  `vicunav-neutral-700` en `theme.json`.
- Motivo: ambos son actualmente `#475569` — el mismo pixel — lo cual anula el
  propósito de tener un color secundario de marca distinguible de la escala neutral.
- Dependencias: ninguna.
- Fuente del requisito: revisión de Claude sobre el resultado real del lote de
  issues 2-11 de `vicunav-theme-core`, nunca aplicada por Codex.
- ADR o contrato aplicable: contrato de `theme-core`, sección de tokens.
- Riesgos: ninguno, es placeholder.

#### Alcance

- Cambiar el valor hex de `vicunav-secondary` en `theme.json` a cualquier valor
  placeholder visualmente distinto de los 12 colores existentes.

#### Fuera de alcance

- Definir la paleta de marca final (eso ocurre en la sesión de diseño externa).

#### Criterios de aceptación

- [ ] `vicunav-secondary` no coincide en valor hex con ningún otro slug de la paleta.
- [ ] El README sigue marcando explícitamente que los colores son placeholder.

#### Validación

- Inspección directa de `theme.json` comparando los 12 valores hex.

---

### vicunav/vicunav-theme-core#N — Corregir CPT inventado en `plantillas-verticales.md`

- Estado: por crear
- Objetivo: que el documento use un CPT real del ecosistema como ejemplo.
- Motivo: usa `vicu_restaurant`, que no existe en ningún contrato ni spec; el real es
  `vicu_menu_item` (spec de `vicunav-restaurante`).
- Dependencias: ninguna.
- Fuente del requisito: misma revisión que la entrada anterior, tampoco aplicada.
- ADR o contrato aplicable: `naming.md` de `vicunav-standards` (cero nombres
  inventados en documentación pública).
- Riesgos: ninguno, es solo documentación.

#### Alcance

- Reemplazar todas las apariciones de `vicu_restaurant` por `vicu_menu_item` en el
  documento: nombre de plantilla, código de ejemplo y texto.

#### Fuera de alcance

- Registrar la plantilla real (`vicunav-restaurante` no existe todavía).

#### Criterios de aceptación

- [ ] Cero apariciones de `vicu_restaurant` en el archivo.
- [ ] El ejemplo sigue siendo técnicamente coherente con `register_block_template()`.

#### Validación

- `grep -c vicu_restaurant docs/plantillas-verticales.md` debe devolver `0`.

## Trabajo descartado o sustituido

- Orquestación Claude Code + Codex CLI (skill `codex-delegate`): explorada,
  abandonada por decisión del usuario antes de usarse en producción. No hay ningún
  issue ni código de esto en ningún repo — no hay nada que revertir.
