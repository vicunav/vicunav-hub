# ADR 0012: Sitio propio de Vicunav en repositorio nuevo

## Contexto

El rework de posicionamiento de Vicunav cerró su Fase A el 2 de agosto de 2026
con headline, subheadline y cuatro pilares confirmados, y adoptó la Dirección B
de identidad: grafito cálido, crema, ámbar y taupe, con Plus Jakarta Sans como
familia única.

El diseño completo del sitio se produjo en Claude Design: quince plantillas en
tres breakpoints, con copy final alineado al SSOT. Incluye cinco páginas
existentes, tres landings de vertical, cuatro páginas de servicio, listado y
detalle de artículos, y una landing de reclutadores.

`vicunav-gutenberg` migra hoy vicunav.com de Elementor a Gutenberg y está en
release candidate 0.2.0, con Home, Servicios, Portafolio y Contacto
implementados y con QA. Ese repositorio nació como banco de pruebas de la
migración, no como el sitio definitivo, y su identidad visual precede a la
Dirección B.

## Alternativas consideradas

1. Continuar sobre `vicunav-gutenberg`, reemplazando su capa visual.
2. Partir de `vicunav-theme-core` y construir un child theme.
3. Crear `vicunav-web`, un block theme FSE propio desde cero, en repositorio nuevo.

La primera arrastra decisiones de una identidad anterior y mezcla el historial
del banco de pruebas con el del producto. La segunda acopla el sitio de la marca
a la evolución del theme compartido, que todavía está en construcción y cuyos
colores son placeholder hasta Fase 2. La tercera permite que el sitio de la
marca avance sin bloquear ni ser bloqueado por el ecosistema modular.

## Decisión

Crear `vicunav-web` como block theme FSE propio, en repositorio nuevo, con
`theme.json` como fuente de verdad de sus tokens.

Alcance del repositorio:

- theme, templates, template parts, patterns y tokens visuales del sitio;
- composición de las quince plantillas del diseño aprobado;
- assets propios servidos localmente;
- plugin propio de `vicunav-web` para registrar `vicu_vertical` y
  `vicu_project`, sin depender de `vicunav-plugin-core` ni de ningún otro
  paquete compartido del ecosistema modular.

Fuera de alcance:

- pagos, reservas y pedidos: `vicunav-web` no es un vertical transaccional y
  no los necesita.

**Corrección del 2026-08-29:** la versión original de este ADR asignaba el
registro de `vicu_vertical` y `vicu_project` a la clase abstracta de
`vicunav-plugin-core`. Mario corrigió esto el mismo día: `vicunav-web` es una
instalación fresca e independiente y no depende de nada del ecosistema
modular, ni siquiera de sus plugins compartidos. Ambos CPT se registran en un
plugin propio de `vicunav-web`, igual que su theme.

La transformación del diseño se ejecuta con el skill
`transform-claude-to-gutenberg` bajo contrato `paridad-1-1`, con los gates
bloqueantes del ADR 0010.

### Arquitectura de contenido

Confirmada por Mario tal como la propuso `arquitectura-wordpress.md` del
handoff de diseño:

| Contenido | Tipo | Clave |
| --- | --- | --- |
| Home, Contacto, Nosotros, Servicios índice | Páginas nativas | — |
| Los 4 servicios | Páginas hijas de `/servicios/` | — |
| Verticales o nichos | CPT | `vicu_vertical` |
| Artículos | Posts nativos | `post` |
| Casos de portafolio | CPT | `vicu_project` |
| Landing de Mario | Página única, fuera del menú público | — |

### Destino de `vicunav-gutenberg`

`vicunav-gutenberg` se mantiene vivo en paralelo, sin relación de dependencia
con `vicunav-web`. No se archiva ni se congela. Es la versión anterior del
sitio y Mario planea reconvertirlo en una implementación de vertical de nicho
más adelante, algo fuera del alcance de este ADR y que se decidirá en su propia
unidad de trabajo cuando corresponda.

### Logotipo

El SSOT confirma que hoy no existe un logotipo gráfico: los prototipos usan
"Vicunav" compuesto en Plus Jakarta Sans como wordmark tipográfico. Mario
confirmó que habrá un logotipo real más adelante. Hasta entonces, el slot de
logotipo (inventario de assets, A11) se trata como **sustitución aprobada**,
con la geometría y composición exactas del wordmark actual del header y
footer. Cuando llegue el archivo real, se abre una unidad de trabajo propia
para sustituirlo, con su propia evidencia visual.

## Consecuencias

- `vicunav.com` pasa a servirse desde `vicunav-web` cuando el sitio alcance
  paridad aprobada.
- `vicunav-gutenberg` sigue activo en paralelo, sin bloquear ni depender de
  `vicunav-web`; su eventual reconversión a vertical de nicho es una decisión
  futura e independiente.
- Los tokens de marca viven en `vicunav-web` y no en `vicunav-theme-core`. Si
  más adelante se comparten, se promueven con reutilización demostrada, nunca
  copiando estilos entre repositorios.
- `vicu_vertical` y `vicu_project` se añaden al registro vivo de CPTs de
  `vicunav-standards/docs/naming.md` en el mismo cambio que los registra.
- Los placeholders de imagen se aceptan como sustitución aprobada y quedan
  registrados en el inventario de assets. Cada asset real posterior abre su
  propia unidad de trabajo.
- El wordmark tipográfico se trata como sustitución aprobada del logotipo
  hasta que exista un archivo real.

## Fuente de verdad

`vicunav-web` para presentación y composición del sitio de la marca.
`vicunav-hub` para esta decisión y sus dependencias.

## Repositorios afectados

`vicunav-web` nuevo (theme y plugin propios), `vicunav-standards` por el
registro de CPTs, `vicunav-hub` por el ADR y el estado del ecosistema.

## Estado

Aprobado por Mario el 2026-08-28.
