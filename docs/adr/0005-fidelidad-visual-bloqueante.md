# ADR 0005: Fidelidad visual 1:1 bloqueante en migraciones a Gutenberg

## Contexto

Toda migración de un diseño aprobado a Gutenberg tiene una fuente visual y funcional
concreta. Comprobar rutas, estructura, responsive, accesibilidad básica, flujos y
rendimiento no acredita fidelidad visual: un checkpoint puede pasar todas esas pruebas
con una composición visiblemente distinta de la fuente. Además, un estilo guardado en
`wp_global_styles` sin el marcador de seguridad requerido por WordPress no llega al CSS
efectivo, aunque el registro exista.

## Alternativas consideradas

1. Tratar la fidelidad visual como revisión manual recomendada al final.
2. Aceptar equivalencia de intención cuando contenido y flujos son correctos.
3. Separar los gates funcional y visual y bloquear el cierre hasta demostrar paridad 1:1
   contra un baseline inmutable.

## Decisión

Toda migración usa la tercera alternativa. La norma completa vive en
`docs/standards/docs/visual-fidelity.md`.

### Criterio 1:1

La fuente se fija por repositorio, commit, datos, navegador, fuentes y viewports.
WordPress reproduce sin diferencias visibles no aprobadas: jerarquía, orden y geometría;
tipografía y saltos de línea; colores, espaciados, bordes, radios y sombras; media y
recortes; header, navegación, footer y llamadas a la acción; estados (hover, focus,
active, expanded, loading, empty, error, success) y comportamiento responsive.

Las diferencias técnicas de DOM son válidas si mantienen semántica, editabilidad y
resultado visual. Los defectos responsive o de accesibilidad de la fuente se corrigen y
se registran como desviaciones deliberadas. Cualquier otra diferencia requiere
aprobación humana explícita, con causa y evidencia.

### Propiedad dentro del proyecto

| Responsabilidad | Ubicación |
| --- | --- |
| Paleta, tipografías, escala, anchos, espaciados, radios, sombras, estilos globales, templates, parts y patterns | `theme/` |
| Markup semántico, interacción y estados funcionales de un bloque de dominio | `plugin/` |
| Copy, media y composición de páginas | `content/` y `assets/` |

El plugin puede publicar el CSS que su bloque necesita para funcionar, pero consume
presets públicos del theme con fallbacks neutrales; no define la identidad de marca.

### Gates obligatorios

1. **Fuente:** commit inmutable, inventario, datos, assets, estados y capturas baseline.
2. **Propiedad:** cada token, componente e interacción asignado al theme, al plugin o al
   contenido.
3. **Corte representativo:** una sección editorial y un estado funcional aprobados en
   frontend y Site Editor antes de escalar.
4. **Migración incremental:** cierre página por página y estado por estado con evidencia
   del mismo commit probado.
5. **Integración:** estilos efectivos, flujos, accesibilidad, responsive, rendimiento y
   editabilidad verificados en el entorno local.
6. **Paridad final:** matriz completa de capturas lado a lado y overlays, diferencias
   aceptadas y aprobación humana del checkpoint.

Un archivo de variación o un registro en la base de datos no demuestra estilos
efectivos: las pruebas inspeccionan el CSS generado y el render en frontend y Site
Editor. Rutas 200, ausencia de overflow, bloques válidos, Lighthouse y auditorías de
accesibilidad siguen siendo obligatorias, pero no sustituyen la comparación visual.

## Consecuencias

- Los estados funcional y visual se registran por separado; un proyecto no se declara
  completo hasta aprobar ambos.
- Un asset ausente bloquea la paridad hasta recuperar el original o recibir una
  sustitución aprobada; no se oculta con un placeholder no declarado.
- Se exige más evidencia intermedia a cambio de descubrir pronto una divergencia de
  diseño generalizada.
