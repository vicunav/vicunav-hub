# Handoff del vertical restaurante a Claude

Fecha: 2026-08-29.

Estado: el proyecto continúa, pero su rework visual y funcional deja de estar a cargo
de Codex. Claude debe retomar la migración desde la fuente aprobada de Claude Design,
no desde el intento local retirado.

## Decisión y alcance de la limpieza

- Se conservan los repositorios, su historial remoto, las definiciones de producto,
  los ADR, la especificación durable y los issues del vertical.
- Se conserva sin cambios la fuente aprobada
  `vicunav-design-to-claude-demo-restaurante`.
- Se eliminan de los checkouts de trabajo los cambios locales no confirmados del
  rework fallido en `vicunav-demo-restaurante` y `vicunav-restaurante`.
- Ambos repositorios se restauran desde sus ramas `main` remotas. La copia retirada
  se conserva temporalmente en la Papelera local para recuperación manual.
- LocalWP vuelve al theme base confirmado antes del intento. El child theme local
  creado durante el rework se retira. El sitio, la base de datos y los enlaces de los
  plugins compartidos no se eliminan.
- No se modifica ningún otro repositorio. En particular, la limpieza histórica que
  pueda requerir `vicunav-theme-core` debe ejecutarse como trabajo separado y
  explícitamente revisado.

## Evidencia del fallo

El cierre anterior no demostró paridad visual 1 a 1 y no debe tratarse como una
implementación aceptada:

1. Se etiquetaron 35 comparaciones como diferencias aprobadas aunque las capturas
   mostraban divergencias visibles y no existía aprobación humana página por página.
2. El reporte histórico registró diferencias perceptuales promedio aproximadas entre
   47,86 % y 64,45 % por superficie, con diferencias puntuales superiores a 94 %.
3. Se sustituyeron geometría, densidad, headers, acciones, tarjetas, footer y estados
   del diseño por composiciones genéricas. La página de pizzas incluso omitió el
   catálogo que precede al constructor.
4. Las pruebas comprobaban existencia de rutas, bloques y ausencia de overflow, pero
   no equivalencia geométrica ni visual. Aun así se cerró el gate final.
5. Antes de detenerlo, el nuevo intento volvió a dispersarse: modificó 151 archivos
   del demo y 17 del plugin, añadió piezas parciales y regeneró evidencia sin cerrar
   de punta a punta una sola página contra todos sus estados y viewports.
6. Se empezó a endurecer presentación Bonasera dentro del theme core y luego mediante
   un child theme, sin resolver primero un contrato completo y verificable de
   propiedad, editabilidad y paridad.
7. El trabajo no se condujo desde el inicio mediante issues atómicos, página por
   página y sección por sección. La descomposición se creó tarde, después de acumular
   cambios transversales difíciles de validar y aislar.

Consecuencia: ninguna captura, manifiesto o etiqueta previa acredita paridad 1 a 1.
Claude debe verificar cada resultado directamente contra el commit fuente congelado.

## Fuentes de verdad que se conservan

- Diseño y comportamiento: repositorio
  `vicunav-design-to-claude-demo-restaurante`, branch `main`, commit
  `1e1f62787e088c0ca9701500e764802499d1b253`.
- Arquitectura del vertical: ADR 0009 y especificación durable restaurante v1.
- Reglas visuales: ADR 0010 y el funnel de fidelidad visual del hub.
- Theme compartido: contratos neutrales publicados por `vicunav-theme-core`.
- Dominio: contratos públicos de `vicunav-restaurante`, `vicunav-plugin-core` y
  `vicunav-pagos`.
- Entorno objetivo: LocalWP en `https://vicunav-demo-restaurante.local`.

El código fuente de Claude Design gobierna la apariencia, la composición responsive
y las interacciones. Los contratos WordPress gobiernan seguridad, persistencia,
dominio y separación de responsabilidades. Un documento legacy que contradiga un ADR
vigente es evidencia histórica, no autoridad.

## Trabajo esperado de Claude

Claude debe ejecutar una migración directa de Claude Design a Gutenberg y completar
el vertical entero. No se espera una reinterpretación visual ni una página aislada.

- Congelar el baseline del commit aprobado y documentar rutas, estados, fixtures,
  fuentes, assets y viewports antes de implementar.
- Trabajar mediante los issues coordinadores y atómicos existentes, uno por uno,
  sección por sección y página por página.
- Crear primero tokens semánticos y presets reutilizables en `theme.json`; evitar
  literales repetidos y CSS acumulativo sin propietario.
- Mantener identidad, contenido y composición Bonasera en el demo o en un child theme
  del demo si el core actual no ofrece controles suficientes. El plugin no contiene
  identidad Bonasera y el theme core no recibe lógica de restaurante.
- Usar bloques core, patterns, templates y template parts para composición editable.
  Crear bloques custom únicamente cuando exista comportamiento dinámico o dominio que
  no pueda expresarse de forma mantenible con bloques core.
- Conservar en `vicunav-restaurante` menú, constructor, carrito, checkout, pedidos,
  reservas y pizzas guardadas, con seguridad y persistencia reales.
- Validar frontend y Site Editor, responsive, navegación por teclado, foco, contraste,
  reducción de movimiento, ausencia de overflow, rendimiento y flujos funcionales.
- No aprobar una diferencia visible automáticamente. Toda desviación debe corregirse
  o quedar registrada con causa concreta y aceptación humana explícita.

### Backlog GitHub vigente

Los issues coordinadores permanecen abiertos porque el proyecto no está cancelado:

- hub: [#111](https://github.com/vicunav/vicunav-hub/issues/111);
- theme core: [#49](https://github.com/vicunav/vicunav-theme-core/issues/49);
- plugin restaurante:
  [#43](https://github.com/vicunav/vicunav-restaurante/issues/43);
- demo restaurante:
  [#21](https://github.com/vicunav/vicunav-demo-restaurante/issues/21).

La ejecución atómica creada para el relevo es:

| Orden | Repositorio | Issue | Unidad verificable |
| ---: | --- | --- | --- |
| 1 | `vicunav-restaurante` | [#44](https://github.com/vicunav/vicunav-restaurante/issues/44) | Primitivas visuales neutrales |
| 2 | `vicunav-restaurante` | [#45](https://github.com/vicunav/vicunav-restaurante/issues/45) | Cobertura de delivery |
| 3 | `vicunav-restaurante` | [#46](https://github.com/vicunav/vicunav-restaurante/issues/46) | Acciones compactas del header |
| 4 | `vicunav-restaurante` | [#47](https://github.com/vicunav/vicunav-restaurante/issues/47) | Menú completo |
| 5 | `vicunav-restaurante` | [#48](https://github.com/vicunav/vicunav-restaurante/issues/48) | Pizzas y constructor |
| 6 | `vicunav-restaurante` | [#49](https://github.com/vicunav/vicunav-restaurante/issues/49) | Carrito y checkout |
| 7 | `vicunav-restaurante` | [#50](https://github.com/vicunav/vicunav-restaurante/issues/50) | Reservas y pizzas guardadas |
| 8 | `vicunav-restaurante` | [#51](https://github.com/vicunav/vicunav-restaurante/issues/51) | Estados de pedido |
| 9 | `vicunav-demo-restaurante` | [#22](https://github.com/vicunav/vicunav-demo-restaurante/issues/22) | Child theme y arquitectura FSE |
| 10 | `vicunav-demo-restaurante` | [#23](https://github.com/vicunav/vicunav-demo-restaurante/issues/23) | Chrome, hero y cobertura |
| 11 | `vicunav-demo-restaurante` | [#24](https://github.com/vicunav/vicunav-demo-restaurante/issues/24) | Secciones editoriales de inicio |
| 12 | `vicunav-demo-restaurante` | [#25](https://github.com/vicunav/vicunav-demo-restaurante/issues/25) | Páginas internas y rutas |
| 13 | `vicunav-demo-restaurante` | [#26](https://github.com/vicunav/vicunav-demo-restaurante/issues/26) | Gate visual y funcional final |
| 14 | `vicunav-theme-core` | [#50](https://github.com/vicunav/vicunav-theme-core/issues/50) | Retirar contaminación Bonasera del core |

Claude debe confirmar dependencias y criterios contra el baseline antes de seguir el
orden. No debe cerrar los coordinadores hasta completar y validar sus hijos.

El prompt ejecutable está en
[`prompt-claude-restaurante-gutenberg.md`](prompt-claude-restaurante-gutenberg.md).

## Criterio de cierre

El trabajo solo termina cuando todas las páginas y estados representativos están
implementados y editables en Gutenberg, sus flujos funcionan sobre el dominio real y
el gate visual comparativo es revisable. Una suite verde sin evidencia visual no es
suficiente, y una captura parecida sin funcionalidad real tampoco lo es.
