# ADR 0007: Modelo de ejecución por proyecto (agente o propietario)

## Contexto

Hasta ahora el agente ha ejecutado la implementación completa de cada proyecto —
theme, plugin, contenido y migración visual—, como en `vicunav-restaurante`. El
propietario quiere, para ciertos proyectos, ejecutar él mismo la capa visual y de
código, usando al agente para estructura, documentación, revisión y validaciones, no
para implementarla.

## Decisión

Al crear un proyecto se elige uno de dos modelos de ejecución, independiente de la
elección de stack del [ADR 0006](0006-stack-de-estilos-por-proyecto.md), y también
registrado en la documentación de arquitectura de ese proyecto:

1. **Agente ejecuta** (default): el agente implementa theme, plugin, contenido y
   migración visual, como en `vicunav-restaurante`.
2. **Propietario ejecuta**: el propietario implementa la capa visual y de código. El
   agente se limita a:
   - crear y mantener la estructura del repositorio (scaffolding, submódulo de
     estándares, CI, plantillas de issue y PR);
   - redactar y mantener specs, ADR y documentación del proyecto;
   - revisar el trabajo del propietario contra los estándares vigentes;
   - ejecutar o preparar validaciones (lint, tests, checks de CI) sin escribir por su
     cuenta el código de la capa visual.

Se prueba primero en `vicunav-bhoga-yoga`.

## Consecuencias

- Un proyecto con "propietario ejecuta" no se considera atrasado ni incompleto por no
  tener theme o plugin implementados; su estado real es el que declare su propia
  documentación, no una expectativa de ritmo del agente.
- El agente no asume trabajo de migración visual o de código en un proyecto marcado
  "propietario ejecuta" aunque el pedido lo sugiera de forma implícita; aclara con el
  propietario si hay ambigüedad antes de escribir código de esa capa.
- Los gates de fidelidad visual ([ADR 0005](0005-fidelidad-visual-bloqueante.md)) siguen
  aplicando igual en ambos modelos: la aprobación final de paridad 1:1 sigue siendo
  humana.
