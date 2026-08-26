# Plan atómico de fidelidad visual

Actualizado: 2026-08-26.

## Propósito

Este documento convierte el incidente visual de Bonasera en dos funnels verificables:
uno endurece el proceso reutilizable y otro recupera la paridad 1:1 del restaurante.
Los IDs son referencias de planificación hasta crear el issue correspondiente. Cada
fila produce un issue, una rama, un PR, validaciones y un squash-merge en el repositorio
propietario.

La decisión normativa está en el
[ADR 0010](../adr/0010-fidelidad-visual-bloqueante.md). Este plan no autoriza
implementación de hotel, demo informativo ni otro vertical.

## Diagnóstico preservado

- La fuente aprobada es el commit
  `1e1f62787e088c0ca9701500e764802499d1b253` de
  `vicunav-design-to-claude-demo-restaurante`.
- La variación Bonasera existe, pero el aplicador del demo omitió
  `isGlobalStylesUserThemeJSON: true`; WordPress ignora su contenido y emite la paleta
  azul y las fuentes del theme base.
- La portada WordPress sustituyó la composición fuente por un Cover genérico, tres
  columnas y patterns compartidos. No trasladó la geometría ni los estados visuales
  específicos de las secciones auditadas.
- Las 45 combinaciones registradas en el gate anterior comprobaron estructura,
  overflow, H1, imágenes y controles. No compararon visualmente fuente y WordPress.
- El dominio, los contratos, los flujos y las correcciones de accesibilidad ya
  verificadas no se descartan. El estado visual se separa del funcional.

## Resultado obligatorio

El producto integrado no vuelve a estado completo hasta que:

1. el flujo impida cerrar una migración sin baseline y evidencia visual;
2. cada token y estilo tenga un propietario y se compruebe en la salida efectiva;
3. las nueve rutas y sus estados relevantes reproduzcan la fuente 1:1;
4. cualquier diferencia por accesibilidad, responsive o assets esté registrada y
   aprobada;
5. el usuario apruebe el checkpoint visual final.

## Funnel A: prevención transversal

| Orden | ID | Repositorio | Resultado atómico | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| A1 | HUB-VIS-01 | `vicunav-hub` | Registrar el ADR 0010, este funnel y la reapertura del checkpoint visual sin implementar runtime | Auditoría posterior a DEMO-REST-01D | Completo: issue 87, PR 88, `56cf70a00e52f43bc7fdc96e0289f20bde385b5c` |
| A2 | STANDARDS-VIS-01 | `vicunav-standards` | Publicar un estándar transversal de fidelidad visual con clasificación de impacto, baseline, evidencia mínima, gates de PR y criterio de bloqueo | HUB-VIS-01 | Completo: issue 11, PR 12, `5c5af785ae7d157af876da8367c2d30f992f0319` |
| A3 | TOOL-VIS-01 | `vicunav-transform-claude-to-gutenberg` | Endurecer el skill con un manifiesto obligatorio de migración, inventario por página y estado, mapa de propiedad y un índice de evidencia validable | STANDARDS-VIS-01 | Completo: issue 1, PR 2, `3e35e14796006ac2d3868bbee7147610f96d6633` |
| A4 | TOOL-VIS-02 | `vicunav-transform-claude-to-gutenberg` | Añadir comandos y pruebas para capturar fuente y WordPress con entorno equivalente, generar lado a lado, overlay y reporte de diferencias, y fallar si falta evidencia | TOOL-VIS-01 | Completo: issue 3, PR 4, `a55cfe447f8ba72098cf940c75605482236d2b35` |
| A5 | TEMPLATE-VIS-01 | `vicunav-repo-template` | Añadir a issues y PRs la clasificación de impacto visual, enlaces al baseline, viewports, estados, diferencias y aprobación requerida | STANDARDS-VIS-01 | Completo: issue 18, PR 19, `34179579367d89c6b6c7d1510fd24163c25b4ca2` |
| A6 | HUB-VIS-02 | `vicunav-hub` | Adoptar las revisiones publicadas de estándar, skill y plantilla; registrar sus commits y habilitar el flujo para futuros proyectos | TOOL-VIS-02, TEMPLATE-VIS-01 | Completo: issue 89 y PR 90 |

### Resultado publicado del funnel A

- `vicunav-standards` añadió `docs/visual-fidelity.md` mediante el
  [issue 11](https://github.com/vicunav/vicunav-standards/issues/11) y el
  [PR 12](https://github.com/vicunav/vicunav-standards/pull/12). El hub fija el squash
  `5c5af785ae7d157af876da8367c2d30f992f0319` como norma transversal vigente.
- `vicunav-transform-claude-to-gutenberg` publicó el manifiesto mediante el
  [issue 1](https://github.com/vicunav/vicunav-transform-claude-to-gutenberg/issues/1)
  y el [PR 2](https://github.com/vicunav/vicunav-transform-claude-to-gutenberg/pull/2)
  en `3e35e14796006ac2d3868bbee7147610f96d6633`. La captura, comparación, reportes,
  hashes y gate final se publicaron mediante el
  [issue 3](https://github.com/vicunav/vicunav-transform-claude-to-gutenberg/issues/3)
  y el [PR 4](https://github.com/vicunav/vicunav-transform-claude-to-gutenberg/pull/4)
  en `a55cfe447f8ba72098cf940c75605482236d2b35`.
- `vicunav-repo-template` exige clasificación visual y evidencia desde el issue y el
  PR mediante el [issue 18](https://github.com/vicunav/vicunav-repo-template/issues/18)
  y el [PR 19](https://github.com/vicunav/vicunav-repo-template/pull/19), con squash
  `34179579367d89c6b6c7d1510fd24163c25b4ca2`.
- Estas entregas endurecen el proceso. No acreditan todavía la paridad Bonasera ni
  completan ninguna unidad B1 a B10.

### Aceptación del funnel A

- Un PR con impacto visual no puede declararse listo sin manifiesto y evidencia.
- El manifiesto enlaza fuente, commit, página, estado, viewport, captura objetivo y
  resultado; no acepta referencias genéricas a una carpeta de screenshots.
- La automatización detecta evidencia ausente y configuraciones de Global Styles que
  no llegan al CSS efectivo.
- El criterio manual exige revisar jerarquía, geometría, tipografía, color, media,
  estados y responsive. Métricas de DOM, Lighthouse o accesibilidad no sustituyen ese
  gate.
- Los consumidores actualizan el submódulo de estándares en issues propios después de
  publicar STANDARDS-VIS-01.

## Funnel B: recuperación Bonasera 1:1

| Orden | ID | Repositorio | Resultado atómico | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| B1 | DESIGN-REST-02 | `vicunav-demo-restaurante` | Congelar baseline visual completo del commit aprobado: nueve rutas o estados equivalentes, cinco viewports, estados interactivos, fuentes, datos y mapa componente-propietario | HUB-VIS-02 | Completo: issue 9, PR 10, `6789edd745887468afca7831fa158b53c78448f0` |
| B2 | DEMO-REST-02A | `vicunav-demo-restaurante` | Resolver inventario de assets 1:1: recuperar originales disponibles y bloquear video/mapas faltantes hasta recibir originales o sustitución humana aprobada | DESIGN-REST-02 | Completo: issue 11, PR 12, `488f21521abf1d723ab3f4eebd1d90c83bcb3af8` |
| B3 | THEME-REST-04 | `vicunav-theme-core` | Completar y probar la variación Bonasera: colores, tipografía, tamaños, alturas, espaciado, anchos, radios, sombras, estilos de elementos y coherencia frontend-editor | DESIGN-REST-02 | Siguiente |
| B4 | THEME-REST-05 | `vicunav-theme-core` | Reproducir 1:1 el chrome y los patterns realmente reutilizables, incluidos hero, categorías, historia, ubicación, testimonios, FAQ, contacto y CTA | THEME-REST-04 | Pendiente |
| B5 | REST-02S | `vicunav-restaurante` | Definir y aplicar el contrato visual neutral de los siete bloques: markup, composiciones intrínsecas y estados funcionales consumen presets públicos del theme sin literales Bonasera | DESIGN-REST-02, THEME-REST-04 | Pendiente |
| B6 | DEMO-REST-02B | `vicunav-demo-restaurante` | Corregir la selección idempotente de Global Styles y probar la paleta y fuentes efectivas en frontend y Site Editor, no solo el post persistido | THEME-REST-04 | Pendiente |
| B7 | DEMO-REST-02C | `vicunav-demo-restaurante` | Recomponer portada y páginas sección por sección con patterns, bloques core, assets y contenido 1:1, sin lógica reutilizable propia | DEMO-REST-02A, THEME-REST-05, DEMO-REST-02B | Pendiente |
| B8 | DEMO-REST-02D | `vicunav-demo-restaurante` | Integrar los siete flujos reales con la composición y los estados visuales aprobados; devolver cualquier defecto reusable a theme o vertical mediante issue separado | REST-02S, DEMO-REST-02C | Pendiente |
| B9 | DEMO-REST-02E | `vicunav-demo-restaurante` | Ejecutar gate final con lado a lado, overlays, Site Editor, accesibilidad, responsive, rendimiento y regresión funcional; registrar diferencias y obtener aprobación humana | TOOL-VIS-02, DEMO-REST-02D | Pendiente |
| B10 | HUB-VIS-03 | `vicunav-hub` | Registrar commits, evidencia y aprobación; cerrar el checkpoint visual solo si todos los gates pasan | DEMO-REST-02E | Pendiente |

### Baseline publicado de DESIGN-REST-02

`vicunav-demo-restaurante` cerró el
[issue 9](https://github.com/vicunav/vicunav-demo-restaurante/issues/9) mediante el
[PR 10](https://github.com/vicunav/vicunav-demo-restaurante/pull/10), con squash
`6789edd745887468afca7831fa158b53c78448f0` y CI verde. El manifiesto fija siete
superficies comparables por cinco viewports: 35 filas, todas `different`, cero
coincidencias y cero diferencias aprobadas. `/pedido/` y `/privacidad/` permanecen
como superficies WordPress sin equivalencia falsa en la SPA.

DEMO-REST-02A cerró el
[issue 11](https://github.com/vicunav/vicunav-demo-restaurante/issues/11) mediante el
[PR 12](https://github.com/vicunav/vicunav-demo-restaurante/pull/12), con squash
`488f21521abf1d723ab3f4eebd1d90c83bcb3af8` y CI verde. Identificó ocho originales
recuperados, un sustituto sin aprobación, cinco originales retenidos por seguridad o
representación responsable y tres activos nunca entregados. No modificó runtime,
contenido persistido, LocalWP ni base de datos.

El gate final bloquea correctamente las 35 diferencias y seis grupos de assets: video
hero, mapas de Zulia y Maracaibo, historia, avatares testimoniales y dolci original.
Son 41 bloqueos esperados, cero coincidencias y cero diferencias aprobadas.

### Aceptación por propietario

#### `vicunav-theme-core`

- `theme.json` y la variación son la fuente de colores, tipografías, tamaños,
  espaciado, anchos, radios y sombras compartidos.
- El CSS del theme cubre composición reusable, responsive, pseudo-elementos y estados
  editoriales que `theme.json` no expresa.
- La variación se prueba seleccionada, efectiva y editable en frontend y Site Editor.
- Los defaults globales de Vicunav no se convierten en Bonasera.

#### `vicunav-restaurante`

- Los bloques conservan semántica, interacción y estados funcionales accesibles.
- Su CSS define únicamente composición intrínseca y representación de estados; consume
  presets y custom properties públicos con fallbacks neutrales.
- No contiene copy, imágenes ni valores de identidad Bonasera.
- Cada estado visible del baseline tiene una fixture y una prueba reproducible.

#### `vicunav-demo-restaurante`

- Posee copy, media, selección de variación, páginas y composición específica.
- No duplica lógica de negocio ni copia internals de los paquetes.
- La aplicación idempotente valida el CSS efectivo de la variación.
- Conserva evidencia versionada y enlazada por página, estado y viewport.

## Gate de assets

El handoff declara ausentes `hero-video.mp4`, `mapa-zulia.png` y
`mapa-maracaibo.png`. La paridad de geometría puede avanzar, pero DEMO-REST-02E no
puede aprobar esos elementos hasta recuperar los originales o registrar una
sustitución elegida por el usuario. No se presenta una imagen genérica como paridad.

## Dependencias resumidas

```text
HUB-VIS-01
  -> STANDARDS-VIS-01
     -> TOOL-VIS-01 -> TOOL-VIS-02
     -> TEMPLATE-VIS-01
        -> HUB-VIS-02
           -> DESIGN-REST-02
              -> assets ------------------------------+
              -> theme tokens -> theme patterns ------+--> composición demo
              -> visual contract del vertical --------+         -> integración
                                                                  -> gate 1:1
                                                                     -> HUB-VIS-03
```

## Bloqueo de proyectos posteriores

`HOTEL-01`, `DESIGN-HOTEL-01`, `INFO-01` y cualquier nueva migración visual dependen
de HUB-VIS-02. Hotel y demo informativo permanecen además fuera del alcance hasta que
HUB-VIS-03 cierre correctamente el checkpoint restaurante.

`BHO-02`, `YOGA-04` y `DEMO-YOGA-04` son las primeras unidades visuales de la pista
Yoga y, por el [ADR 0011](../adr/0011-bhoga-yoga-cliente-privado.md), dependen
directamente de HUB-VIS-03. BHO-02 depende además de las decisiones previas del
cliente. Las fundaciones de los tres repositorios solo preparan gobierno, contratos,
documentación y configuración; no implementan superficies visuales ni evitan el gate.

## Siguiente unidad ejecutable

DESIGN-REST-02 y DEMO-REST-02A están completos. La siguiente unidad es THEME-REST-04
en `vicunav-theme-core`: cerrar la variación Bonasera contra el baseline 1:1. El resto
del funnel permanece pendiente. Hotel y demo informativo continúan fuera de alcance.
Las superficies visuales Yoga permanecen bloqueadas por HUB-VIS-03; Bhoga depende
además de su gate previo de cliente.
