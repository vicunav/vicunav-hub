# Prompt para Claude: migración directa de Bonasera a Gutenberg

Copia desde la siguiente línea y entrégalo a Claude en una sesión con acceso a los
repositorios locales y a LocalWP.

---

Quiero que completes de punta a punta la migración del diseño Bonasera creado en
Claude Design a un sitio WordPress nativo de Gutenberg y Full Site Editing. No quiero
una reinterpretación ni una muestra parcial: necesito el rework completo de todas las
páginas, sección por sección, con paridad visual y funcional verificable.

## Fuente inmutable

- Repositorio fuente:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-design-to-claude-demo-restaurante`
- Branch: `main`
- Commit aprobado: `1e1f62787e088c0ca9701500e764802499d1b253`
- Instalación: `npm ci`
- Runtime de referencia:
  `npm run dev -- --host 127.0.0.1 --port 4173`

Antes de modificar WordPress, ejecuta y verifica lint, las 66 pruebas y el build de
la fuente. Congela capturas, rutas, estados, fixtures, fuentes, assets y reglas
responsive desde ese commit. No uses memoria visual ni el intento WordPress anterior
como autoridad.

## Repositorios y responsabilidades

- Demo y composición Bonasera:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-demo-restaurante`
- Dominio y bloques dinámicos:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-restaurante`
- Theme base neutral:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-theme-core`
- Core de plugins:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-plugin-core`
- Pagos:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-pagos`
- Gobierno, ADR, especificaciones y handoff:
  `/Users/vicunav/Documents/Codex/vicunav/vicunav-hub`

Lee primero los `AGENTS.md` aplicables, los estándares del hub, ADR 0009, ADR 0010,
la especificación durable restaurante v1 y
`docs/handoff/restaurante-handoff-claude.md`. Si un documento legacy propone
WooCommerce, ignóralo: la arquitectura vigente usa el dominio propio de
`vicunav-restaurante` y `vicunav-pagos`.

No modifiques otros verticales o demos. No conviertas el theme core en Bonasera ni
introduzcas en él copy, assets, slugs, clases o lógica de restaurante. Si el core
actual no permite configurar toda la identidad desde el administrador, implementa un
child theme versionado dentro del demo como solución transitoria. Ese child theme
puede poseer `theme.json`, fuentes, estilos, templates, template parts y patterns de
Bonasera, pero nunca dominio, pricing, persistencia ni APIs del restaurante.

## Entorno WordPress objetivo

- URL: `https://vicunav-demo-restaurante.local`
- Site root:
  `/Users/vicunav/Local Sites/vicunav-demo-restaurante/app/public`
- WordPress: 7.1
- PHP: 8.2.29
- PHP LocalWP:
  `/Users/vicunav/Library/Application Support/Local/lightning-services/php-8.2.29+0/bin/darwin/bin/php`
- Socket MySQL:
  `/Users/vicunav/Library/Application Support/Local/run/aNGgUb3VB/mysql/mysqld.sock`

Usa WP-CLI con el binario y socket de LocalWP. No diagnostiques un fallo de conexión
como bug del plugin sin verificar primero el socket y `wp_options.home` o `siteurl`.
No borres la base de datos ni contenido ajeno al demo.

## Alcance completo

Implementa y compara estas superficies de la fuente:

1. Inicio completo, incluido chrome, hero y todas las secciones editoriales.
2. Menú, filtros, tarjetas, estados y acciones de carrito.
3. Pizzas, incluido el catálogo tipo ticket previo al constructor y todo el builder.
4. Carrito vacío y con productos.
5. Checkout con fixtures equivalentes y validaciones reales.
6. Reservas, disponibilidad, alternativas, confirmación y cancelación.
7. Pizzas guardadas, estados autenticado, vacío y con contenido.
8. Estado de pedido requerido por el contrato WordPress.
9. Privacidad requerida por WordPress, coherente con el sistema visual.
10. Header, navegación, acciones compactas, footer y variantes entre portada e
    interiores.

Incluye estados default, hover, focus, active, selected, expanded, loading, empty,
error, success y disabled cuando existan en la fuente o sean necesarios para un flujo
real. Mantén copy, orden, jerarquía, geometría, espaciado, tipografía, iconografía,
colores, bordes, radios, sombras, capas, recortes y comportamiento responsive.

Viewports obligatorios:

- 1440 x 1000
- 1024 x 900
- 768 x 1024
- 390 x 844
- 375 x 812

Además, verifica que no exista overflow horizontal a 360 px.

## Método obligatorio

1. Audita los issues coordinadores y atómicos existentes en GitHub. Ajusta su texto
   si el baseline demuestra una omisión, pero no dupliques issues equivalentes.
2. Trabaja un issue atómico a la vez, de punta a punta: branch `claude/<issue>-...`,
   implementación, pruebas dirigidas, QA visual, PR, squash merge e issue cerrado.
3. Dentro de cada página, avanza sección por sección. No abras cambios transversales
   en varias páginas sin cerrar y comparar la unidad actual.
4. Extrae primero un sistema de tokens semánticos: color, tipografía, escala,
   spacing, layout, radios, sombras, bordes, capas y motion. Publícalos como presets y
   variables de `theme.json` siempre que sea posible. No hardcodees el mismo valor en
   múltiples componentes.
5. Traduce composición editorial a bloques core, patterns, templates y template
   parts. No uses `core/html` como escape y no lleves React, Vite o Tailwind al
   runtime de WordPress.
6. Crea o corrige bloques custom solo para comportamiento dinámico o dominio real.
   Prefiere `block.json`, render dinámico cuando corresponda, Interactivity API y
   contratos públicos del plugin. El contenido editorial debe seguir editable en el
   Site Editor.
7. Mantén CSS por propietario y componente. No construyas una hoja global acumulativa
   de excepciones. Evita selectores frágiles dependientes del contenido persistido.
8. Usa assets locales y licenciados. Si falta un original, usa un placeholder con la
   misma geometría, ratio, recorte y posición, regístralo como deuda y no lo presentes
   como paridad final aprobada.
9. Captura fuente y target con el mismo viewport, datos, estado, scroll, fuentes y
   animaciones estabilizadas. Genera lado a lado y overlay para cada caso.
10. Corrige divergencias en ciclos pequeños. No etiquetes automáticamente ninguna
    diferencia como aprobada. Una excepción requiere causa específica, evidencia y
    aceptación humana explícita.

## Arquitectura esperada

- `theme.json` y el child theme del demo, si hace falta: identidad y tokens Bonasera.
- Templates, template parts y patterns del demo: composición editorial y chrome.
- `vicunav-restaurante`: menú, constructor, pricing, carrito, checkout, pedidos,
  reservas, pizzas guardadas, seguridad, persistencia y bloques dinámicos neutrales.
- `vicunav-pagos`: integración de pagos conforme al contrato vigente.
- `vicunav-theme-core`: solo capacidades neutrales reutilizables ya existentes o una
  mejora demostrablemente transversal mediante issue separado. No introduzcas código
  Bonasera para resolver este demo.

No inventes endpoints, estados o fixtures si ya existe un contrato. No sustituyas
funcionalidad real con controles decorativos. No aceptes que la acción principal
funcione si la página sigue mostrando defectos visuales claros.

## Gates de aceptación

Cada issue atómico debe demostrar:

- paridad de estructura, escala, densidad, alineación y responsive contra la fuente;
- edición válida en Gutenberg y Site Editor, sin errores de validación de bloques;
- comportamiento real del flujo y persistencia correcta cuando corresponda;
- teclado completo, foco visible, nombres accesibles, contraste y targets táctiles;
- ausencia de overflow a todos los viewports;
- pruebas PHP, JavaScript, build, lint y E2E relevantes en verde;
- comparación visual revisable con fuente, target, lado a lado y overlay;
- cero identidad Bonasera en el plugin y cero lógica del vertical en el theme;
- documentación del issue, commit y evidencia exacta, sin afirmaciones genéricas.

Antes del cierre global, realiza un corte representativo temprano: home, menú,
constructor, carrito y checkout en desktop, tablet y móvil. Muéstralo para revisión
humana antes de extender la solución a todas las variantes. Después completa la
matriz total, prueba frontend y editor, y entrega una tabla final por ruta, estado y
viewport con resultado, evidencia y cualquier diferencia todavía abierta.

No declares terminado el proyecto hasta que todas las páginas y funcionalidades estén
completas. Si encuentras un defecto preexistente ajeno, crea un issue atómico separado;
si bloquea una validación obligatoria, resuélvelo antes de continuar. Detente solo por
una contradicción real con un ADR, una decisión de producto que no pueda inferirse de
la fuente o una acción destructiva que requiera aprobación.

---
