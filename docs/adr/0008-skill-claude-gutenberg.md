# ADR 0008: Skill de migración de Claude Code a Gutenberg

## Contexto

Los diseños de los demos pueden comenzar como prototipos React, Next.js, Vite, HTML o
CSS creados y refinados fuera de WordPress. Convertirlos mediante prompts puntuales no
garantiza una arquitectura FSE editable, una separación correcta de la lógica de
negocio ni una validación consistente de fidelidad visual, accesibilidad y LocalWP.

El flujo se repetirá en el demo de restaurante y puede reutilizarse en otros proyectos.
Por ello necesita una fuente de verdad pública y versionada, separada tanto del código
de runtime como del contenido privado usado durante una migración concreta.

## Alternativas consideradas

1. Mantener instrucciones personales no versionadas en el directorio local de skills.
2. Copiar el flujo dentro de cada repositorio de demo.
3. Crear un repositorio público propio para el skill y consumirlo como herramienta.

La primera alternativa no aporta trazabilidad, CI ni una superficie pública de
aprendizaje. La segunda duplicaría el contrato y mezclaría tooling con composición del
demo. La tercera permite evolución independiente y mantiene claros los límites.

## Decisión

Crear
[`vicunav-transform-claude-to-gutenberg`](https://github.com/vicunav/vicunav-transform-claude-to-gutenberg)
como fuente de verdad del skill `transform-claude-to-gutenberg`.

El repositorio debe:

- publicar el skill, sus referencias, validadores y pruebas;
- traducir prototipos aprobados a `theme.json`, templates, template parts, patterns,
  bloques core y, solo cuando el contrato lo exija, bloques custom o plugins;
- interoperar con los skills oficiales de WordPress como dependencias separadas;
- ejecutar el trabajo en LocalWP sin versionar bases de datos, credenciales, rutas
  personales, capturas privadas ni contenido de clientes;
- conservar frontend y Site Editor como superficies obligatorias de validación;
- formar parte de las herramientas de desarrollo coordinadas por el hub, sin ser una
  dependencia de runtime de themes, plugins, verticales o demos.

El repositorio propietario mantiene su propio README, licencia, CI, historial y
contrato. El hub registra únicamente su papel en la arquitectura y en las pistas de
diseño.

## Consecuencias

- El skill puede instalarse en Codex, Claude Code u otros agentes compatibles sin
  duplicar instrucciones entre proyectos.
- La migración del demo de restaurante cuenta con un proceso verificable antes de que
  exista su repositorio canónico.
- Los cambios del skill no obligan a publicar una nueva versión de los paquetes de
  runtime.
- Los consumidores deben instalar por separado las bases oficiales de WordPress y
  comprobar compatibilidad con su versión local de core.
- Cada migración conserva su contrato, evidencia y contenido en el repositorio o
  entorno autorizado correspondiente, nunca en el repositorio del skill.

## Estado

Aplicada y verificada el 2026-08-13 mediante el repositorio público, su CI inicial y la
integración documental del hub.
