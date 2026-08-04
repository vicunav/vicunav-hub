# Prompt de cierre y traspaso para Claude

Este prompt se envía una sola vez a Claude para convertir su contexto histórico en
memoria durable del proyecto. No transfiere permisos del sistema: formaliza que Codex
será el agente principal de ingeniería y que Claude deja de implementar.

## Prompt listo para copiar

```text
Esta es tu última tarea operativa como agente principal del ecosistema Vicunav. A
partir de este traspaso, Codex será el agente principal para análisis, decisiones
técnicas, implementación, validación, Git, issues y pull requests. Yo, el usuario,
conservo la autoridad final sobre producto y sobre cualquier acción destructiva,
irreversible, de producción, credenciales, dinero o comunicación externa. No intentes
alterar permisos del sistema ni credenciales.

Tu única misión ahora es entregar a Codex todo el contexto útil que acumulaste y dejar
el trabajo pendiente convertido en unidades ejecutables. No desarrolles nuevas
funcionalidades ni aproveches para corregir código. No descartes, sobrescribas ni
mezcles cambios locales existentes. No incluyas secretos, tokens, contraseñas, salts,
datos personales ni valores de producción.

Repositorio coordinador: `vicunav-hub` en el workspace actual.

Antes de escribir:

1. Lee el AGENTS.md aplicable y docs/standards/ de cada repositorio implicado.
2. Audita tus memorias, planes, conversaciones recientes, issues, PRs y archivos de
   contexto del proyecto.
3. Recorre todos los repositorios que gestionaste para Vicunav, incluidos los que aún
   no existan localmente. Como mínimo aclara la situación de vicunav-hub,
   vicunav-standards, vicunav-repo-template, vicunav-theme-core,
   vicunav-plugin-core, vicunav-pagos, vicunav-hotel, vicunav-restaurante,
   vicunav-demo-hotel, vicunav-demo-restaurante y vicunav-gutenberg. Clasifica también
   cualquier repo administrativo o experimental que encuentres.
4. Verifica Git y GitHub sin asumir que un reporte anterior sigue vigente: rama,
   commit, working tree, remotos, issues, PRs, merges, releases y CI. No imprimas
   credenciales.

Actualiza estos documentos canónicos del hub:

- docs/handoff/estado-ecosistema.md
- docs/handoff/backlog-ecosistema.md

En estado-ecosistema.md explica el hub de forma suficientemente completa para que
Codex pueda continuar sin preguntarte: propósito del producto, arquitectura por capas,
límites entre theme/plugins/pagos/verticales/demos, dependencias, contratos y hooks,
ADRs vigentes, orden de implementación, entornos LocalWP, decisiones descartadas,
supuestos, riesgos, deuda y estado real de cada repositorio. Distingue siempre hechos
verificados, decisiones vigentes e inferencias. Enlaza la fuente original en lugar de
duplicar normas o ADRs.

En backlog-ecosistema.md crea un índice único del desarrollo restante. Primero busca
issues existentes y elimina duplicados conceptuales del plan, sin cerrar ni borrar
nada que no esté inequívocamente reemplazado. Cada unidad debe pertenecer a un solo
repositorio y ser atómica: una rama, un PR y un squash-merge. Para cada unidad registra
repositorio, título, objetivo, motivo, dependencias, alcance, fuera de alcance,
criterios de aceptación verificables, validaciones o comandos reales, riesgos/ADR,
prioridad, orden y fuente del requisito.

Si tienes acceso suficiente a GitHub, crea los issues atómicos en el repositorio que
corresponda y enlázalos desde el backlog. No inventes comandos de prueba ni crees
issues vacíos. Si un repositorio todavía no existe o no puedes crear el issue, deja la
entrada con estado "por crear" y la razón exacta. El Markdown es el índice y contexto;
GitHub es la fuente del estado de ejecución cuando exista el issue.

Incluye una sección "Siguiente acción recomendada" con exactamente un próximo issue
sin bloqueos y explica por qué va primero. Incluye una sección "Preguntas abiertas"
solo para decisiones que realmente requieran al usuario; no conviertas en pregunta lo
que puedas verificar.

Haz este trabajo con el flujo Git requerido por vicunav-hub: issue atómico para el
traspaso, rama, validación documental, PR y squash-merge. Al terminar, entrega:

1. enlaces al issue, PR y commit final del hub;
2. lista de issues creados por repositorio;
3. archivos actualizados;
4. trabajo pendiente y bloqueos reales;
5. la única siguiente acción recomendada;
6. una despedida breve confirmando que el traspaso terminó y que no continuarás
   desarrollando Vicunav salvo que yo te reactive explícitamente.

Después de ese mensaje final, detén tu trabajo sobre Vicunav. No pidas que Codex te
envíe diffs y no mantengas un proceso paralelo de revisión.
```

## Resultado esperado

El traspaso termina cuando el hub contiene una explicación verificable del sistema,
el backlog enlaza trabajo atómico sin duplicados y existe una única próxima acción.
Los chats de Claude quedan como evidencia histórica, no como dependencia operativa.
