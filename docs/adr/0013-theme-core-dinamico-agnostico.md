# ADR 0013: Theme-core dinámico, agnóstico y compartido

## Contexto

El ecosistema necesita reproducir demos, verticales e implementaciones de cliente con
identidades visuales distintas sin bifurcar el theme base. Resolver cada identidad con
un child theme trasladaría tokens, tipografías, chrome y composición global a archivos
específicos, multiplicaría variantes y debilitaría la reproducibilidad del sistema.

La incorporación anterior de nombres, assets y estilos Bonasera en
`vicunav-theme-core` demostró además que promover una necesidad de un demo al theme
compartido contamina su frontera. La alternativa tampoco puede ser una configuración
manual imposible de reconstruir: el estado dinámico de WordPress debe poder
declararse, aplicarse de forma idempotente y verificarse.

## Decisión

`vicunav-theme-core` será el único theme base compartido por los demos, verticales e
implementaciones de cliente del ecosistema. Su capacidad pública debe permitir que
cada sitio defina desde la administración de WordPress y el Site Editor todo lo
variable de su presentación, sin editar archivos del theme ni crear un child theme.

La superficie dinámica debe cubrir, como mínimo:

- identidad visual, paletas, tipografías y escalas;
- espaciado, anchos, radios, bordes, sombras y otros tokens de estilo;
- selección y configuración de headers, footers y chrome global;
- composición de templates, template parts y patterns mediante capacidades
  neutrales;
- comportamiento responsive y estados visuales configurables que pertenezcan a la
  presentación compartida.

El código estable que implementa esas capacidades permanece versionado en
`vicunav-theme-core`. Los valores de una marca, demo o vertical se guardan como
configuración dinámica de la instancia y nunca se codifican en los archivos del
theme-core.

Cada vertical mantiene su propio plugin como propietario de dominio, datos, servicios,
bloques funcionales y presentación intrínseca de esas interfaces. Cada demo o
implementación consume `vicunav-theme-core` junto con el plugin de su vertical, o con
los plugins transversales que correspondan si no existe dominio vertical. El demo
conserva contenido, media, composición y valores de configuración, pero no crea una
bifurcación del theme.

Para que el estado dinámico sea reproducible, el contrato público del theme debe
permitir exportar o declarar la configuración específica de un sitio y aplicarla de
forma idempotente. Un repositorio de demo puede versionar ese manifiesto como entrada
de instalación, pero no puede usarlo para introducir overrides de PHP o CSS en
`vicunav-theme-core`.

No se crearán nuevos child themes como mecanismo normal de personalización. Una
excepción solo podrá aprobarse mediante una decisión explícita que demuestre una
limitación técnica no resoluble con el contrato público, defina su retirada y no
contamine el core. El child theme Bonasera autorizado para el rework actual se
considera una excepción transitoria y no un precedente arquitectónico; deberá
retirarse cuando la capacidad dinámica equivalente esté disponible y verificada.

`vicunav-theme-core` no puede contener trazas de un demo, cliente o vertical. Esto
incluye nombres, copy, slugs, rutas, assets, fuentes de marca, presets, variaciones,
clases, templates, parts, patterns, CSS, fixtures o documentación específica. Una
capacidad descubierta en un demo solo entra al core cuando se abstrae como contrato
neutral y se demuestra su utilidad transversal.

## Fuente de verdad y propietarios

| Responsabilidad | Propietario |
| --- | --- |
| Capacidades neutrales, controles dinámicos y contrato de presentación | `vicunav-theme-core` |
| Dominio, datos y bloques funcionales reusables | Plugin del vertical |
| Contenido, media, composición y valores de identidad | Demo o implementación de cliente |
| Estado aplicado en una instancia | WordPress, mediante el contrato público del theme |
| Manifiesto reproducible de esa configuración | Repositorio del demo o implementación |

## Consecuencias

- Todas las verticales comparten el mismo theme y evolucionan sobre un único contrato
  mantenible.
- Añadir una demo exige configurar capacidades públicas y activar su plugin vertical,
  no copiar el theme ni escribir una variante estática dentro del core.
- `vicunav-theme-core` necesita una iniciativa transversal para inventariar valores
  hoy estáticos, diseñar controles y APIs, migrar consumidores y probar exportación,
  aplicación idempotente, Site Editor y frontend.
- La configuración persistida no se considera reproducible hasta que una instalación
  limpia pueda reconstruirla desde una entrada versionada y producir el mismo render.
- La fidelidad visual sigue siendo bloqueante: dinamismo y neutralidad no justifican
  diferencias respecto del baseline aprobado.
- La separación entre presentación y lógica de negocio del ADR 0001 permanece vigente;
  este ADR precisa cómo se personaliza la presentación sin child themes ni rastros de
  verticales en el theme compartido.

## Propagación y validación

1. Inventariar toda traza específica y todo valor visual estático existente en
   `vicunav-theme-core`.
2. Definir el contrato de controles, almacenamiento, exportación y aplicación
   idempotente sin depender de nombres de demos o verticales.
3. Demostrar el contrato con al menos dos identidades de verticales distintas.
4. Migrar Bonasera y los siguientes consumidores al mismo theme-core sin pérdida de
   fidelidad, editabilidad ni comportamiento responsive.
5. Retirar excepciones transitorias después de verificar frontend, Site Editor,
   instalación limpia, accesibilidad y gate visual 1:1.

