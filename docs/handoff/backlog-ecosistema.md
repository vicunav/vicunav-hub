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
- Antes de crear una entrada, buscar issues abiertos y cerrados para evitar duplicados.
- Usar `por crear` únicamente cuando el repositorio o los permisos todavía no permitan
  abrir el issue.

## Índice priorizado

Pendiente de completar por Claude durante el traspaso. Debe ordenar primero el trabajo
sin bloqueos y respetar el ADR 0006 sobre la prioridad de restaurante.

| Orden | Repositorio | Issue | Título | Estado | Depende de |
| --- | --- | --- | --- | --- | --- |
| 1 | Pendiente | Por crear | Pendiente de auditoría | Bloqueado por traspaso | Ninguna |

## Ficha obligatoria por issue

```md
### propietario/repo#N — Título atómico

- Estado: abierto | bloqueado | cerrado | por crear
- Objetivo:
- Motivo:
- Dependencias:
- Fuente del requisito:
- ADR o contrato aplicable:
- Riesgos:

#### Alcance

- ...

#### Fuera de alcance

- ...

#### Criterios de aceptación

- [ ] ...

#### Validación

- `comando real`, o explicación explícita de la validación manual si aún no existe
  automatización.
```

## Trabajo descartado o sustituido

Claude debe registrar aquí planes anteriores que ya no sean vigentes, con el enlace y
la razón. No cerrar ni borrar issues ambiguos durante el traspaso.
