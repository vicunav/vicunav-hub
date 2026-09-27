# ADR 0006: Stack de estilos por proyecto (nativo o Tailwind CSS)

## Contexto

Cada proyecto define su identidad visual en `theme/` mediante `theme.json`, tokens y
CSS propio por patrón o bloque ([ADR 0001](0001-separacion-theme-plugins.md)). Un
proyecto puede preferir escribir esa capa con Tailwind CSS y clases utilitarias en vez
de una hoja de estilo dedicada por patrón. WordPress sigue generando sus propias clases
de bloque (`wp-block-*`, `has-*-color`) sin importar el stack elegido; ninguno de los
dos enfoques las sustituye.

## Alternativas consideradas

1. Mantener el CSS nativo por patrón como único stack permitido.
2. Adoptar Tailwind CSS como default para todo proyecto nuevo.
3. Permitir ambos stacks, elegidos por proyecto en el momento de crearlo.

## Decisión

Se adopta la tercera alternativa. Al crear un proyecto se elige uno de dos stacks de
estilos, y la elección queda registrada en la documentación de arquitectura de ese
proyecto (por ejemplo `docs/architecture.md`):

1. **Nativo** (default): `theme.json`, tokens y CSS propio por patrón o bloque, como en
   `vicunav-restaurante`.
2. **Tailwind CSS**: compilado localmente y comprometido en el repositorio. Cubre el
   markup propio de patterns y bloques dinámicos del proyecto; no sustituye las clases
   que WordPress genera para los bloques core.

La elección es por proyecto, no un default del conjunto, y no crea ninguna dependencia
compartida: un proyecto con Tailwind define sus propios tokens en su propio
`tailwind.config.js`, sin referenciar ni consumir el de otro repositorio. Las reglas
técnicas de esta opción viven en `docs/standards/docs/tailwind.md`.

Esta decisión es independiente de quién ejecuta la implementación
([ADR 0007](0007-modelo-de-ejecucion-por-proyecto.md)); un proyecto elige cada eje por
separado.

## Consecuencias

- Dos proyectos pueden usar stacks distintos sin contradecir la separación theme/plugin
  del ADR 0001.
- Ningún paquete, config ni preset de Tailwind se comparte entre repositorios; adoptar
  Tailwind en un proyecto no obliga a los demás ni crea un punto de fallo compartido.
- Un proyecto que elige Tailwind sigue sujeto a fidelidad visual
  ([ADR 0005](0005-fidelidad-visual-bloqueante.md)), accesibilidad y presupuestos de
  rendimiento existentes, sin excepciones por el stack.
