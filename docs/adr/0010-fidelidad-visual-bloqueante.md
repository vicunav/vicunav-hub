# ADR 0010: Fidelidad visual bloqueante en migraciones a Gutenberg

## Contexto

Los demos de Vicunav parten de una secuencia explícita: diseño aprobado en Claude
Design, refinamiento ejecutable en Claude Code y transformación a WordPress Gutenberg
mediante Codex. El commit aprobado de Claude Code es tanto una especificación
funcional como una especificación visual.

El checkpoint de Bonasera cerró `DEMO-REST-01D` después de comprobar rutas, estructura,
responsive, accesibilidad básica, flujos y rendimiento. Una revisión posterior contra
el commit auditado `1e1f62787e088c0ca9701500e764802499d1b253` demostró que esas
comprobaciones no acreditaban fidelidad visual 1:1. También reveló que la variación
Bonasera se había escrito en `wp_global_styles` sin el marcador de seguridad requerido
por WordPress, por lo que el frontend continuaba usando los estilos predeterminados.

El skill `transform-claude-to-gutenberg` ya exigía baseline, comparación lado a lado y
evidencia visual. El fallo fue de ejecución y de cumplimiento: los issues, pruebas y
documentos de cierre permitieron sustituir la comparación visual por métricas
estructurales.

## Alternativas consideradas

1. Mantener la fidelidad visual como revisión manual recomendada al final de cada demo.
2. Aceptar equivalencia de intención cuando contenido y flujos sean correctos.
3. Separar los gates funcional y visual, y bloquear el cierre de una migración hasta
   demostrar paridad 1:1 contra un baseline inmutable.

La primera alternativa repite el mecanismo que permitió el cierre incorrecto. La
segunda contradice el propósito del pipeline de diseño. La tercera hace observable la
calidad esperada y permite conservar por separado el progreso funcional válido.

## Decisión

Toda transformación de un diseño aprobado a Gutenberg usa la tercera alternativa.

### Contrato de fidelidad 1:1

La fuente aprobada se fija por repositorio, commit, datos, navegador, fuentes y
viewports. WordPress debe reproducir sin diferencias visibles no aprobadas:

- jerarquía, orden y geometría de secciones;
- tipografía, saltos de línea, tamaños y alturas de línea;
- colores, espaciados, bordes, radios, sombras y capas;
- media, recortes, ratios y focos visuales;
- header, navegación, footer y llamadas a la acción;
- estados hover, focus, active, expanded, loading, empty, error y success;
- comportamiento responsive y reordenamiento por breakpoint.

Las diferencias técnicas de DOM o implementación son válidas si mantienen semántica,
editabilidad y resultado visual. Los defectos responsive o de accesibilidad del
prototipo se corrigen y se registran como desviaciones deliberadas. Cualquier otra
diferencia necesita aprobación humana explícita.

Cada página y estado se compara con capturas lado a lado y overlay en los mismos
viewports. Una herramienta de diferencia de píxeles puede apoyar la revisión, pero no
reemplaza la inspección visual ni convierte una diferencia clara en aceptable.

### Propiedad de presentación y comportamiento

| Responsabilidad | Repositorio propietario |
| --- | --- |
| Paleta, tipografías, escala, anchos, espaciados, radios, sombras y estilos globales | `vicunav-theme-core` |
| Templates, template parts, patterns y estilos editoriales reutilizables | `vicunav-theme-core` |
| Markup semántico, interacción, estados funcionales y composición intrínseca de un bloque de dominio | Plugin vertical propietario |
| Valores de marca consumidos por un bloque de dominio | Theme mediante presets y propiedades públicas, nunca literales Bonasera en el plugin |
| Copy, media, selección de variación y composición específica de una marca | Repositorio del demo |
| Reglas compartidas, manifiesto de evidencia y herramientas de comparación | `vicunav-standards` y `vicunav-transform-claude-to-gutenberg` |

Un plugin puede publicar CSS necesario para que su bloque funcione, incluidos estados
de loading, error, selección o disponibilidad. Ese CSS consume tokens públicos del
theme y mantiene fallbacks neutrales; no define la identidad Bonasera. `vicunav-plugin-core`
no recibe estilos de un vertical salvo que una reutilización transversal se demuestre
y se apruebe en una unidad separada.

### Gates obligatorios

1. **Fuente:** commit inmutable, inventario, datos, assets, estados y capturas baseline.
2. **Propiedad:** mapa completo de cada token, componente, interacción y composición a
   su repositorio propietario.
3. **Corte representativo:** una sección editorial y un estado funcional aprobados en
   frontend y Site Editor antes de escalar.
4. **Migración incremental:** cierre página por página y estado por estado con evidencia
   del mismo commit probado.
5. **Integración:** estilos efectivos, flujos reales, accesibilidad, responsive,
   rendimiento y editabilidad verificados en LocalWP.
6. **Paridad final:** matriz completa de capturas, overlays, diferencias aceptadas y
   aprobación humana del checkpoint.

La existencia de un archivo de variación o de un registro en la base de datos no
demuestra que sus estilos sean efectivos. Las pruebas deben inspeccionar el CSS
generado y el resultado renderizado en frontend y Site Editor.

Los estados funcional y visual se registran por separado. Un runtime puede estar
completo mientras su demo permanece visualmente pendiente. Ningún demo ni vertical se
declara completo como producto integrado hasta aprobar ambos estados.

## Consecuencias

- El checkpoint visual de restaurante se reabre; el runtime de
  `vicunav-restaurante` conserva su estado verificado.
- El cierre histórico de issues y PRs permanece trazable, pero no constituye
  aprobación visual cuando su evidencia fue insuficiente.
- Hotel, demo informativo y futuras transformaciones quedan bloqueados hasta adoptar
  el flujo endurecido y cerrar la recuperación visual de restaurante.
- La migración requiere más evidencia y revisiones intermedias, pero evita completar
  toda la lógica antes de descubrir una divergencia de diseño generalizada.
- Los faltantes de assets bloquean paridad 1:1 hasta recuperar el original o recibir
  una sustitución aprobada; no se ocultan mediante placeholders.

## Propagación

El funnel atómico, sus propietarios y dependencias se mantienen en el
[plan de fidelidad visual](../handoff/plan-fidelidad-visual.md). Cada unidad se ejecuta
mediante issue, rama, pull request, CI y squash-merge en su repositorio propietario.

## Estado

Decidida y planificada el 2026-08-20. Su propagación y la recuperación Bonasera están
pendientes.
