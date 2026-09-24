# Estado actual

Actualizado: 2026-09-24.

Cada proyecto es un repositorio autocontenido con su propio theme de bloques y su
propio plugin ([ADR 0001](adr/0001-separacion-theme-plugins.md)). Las decisiones
vigentes están en [`docs/adr/`](adr/) y los pendientes en el [backlog](backlog.md).

## Por repositorio

| Repositorio | Estado |
| --- | --- |
| `vicunav-restaurante` | Proyecto de referencia autocontenido: `plugin/` (plugin único con módulos de dominio restaurante, pagos manuales y capacidades compartidas de FAQ, testimonios, ajustes y REST), `theme/` (`vicunav-bonasera`), `content/`, `assets/`, instalador local y QA. La lógica funcional está implementada y verificada; el checkpoint de fidelidad visual 1:1 de sus nueve rutas está abierto. |
| `vicunav-bhoga-yoga` | Implementación privada del cliente: `theme/` (`bhoga-yoga`), `plugin/` (`bhoga-yoga-content`: FAQ, testimonios y ajustes), contenido, documentación y gates propios. Según su roadmap, BHO-02 a BHO-07 están completados localmente en `devbhogayoga.local`; BHO-08 y BHO-09 los ejecuta el propietario fuera del repositorio. Producción no se ha modificado. |
| `vicunav-standards` | Estándares técnicos compartidos, incluida la norma de fidelidad visual. Es la fuente del submódulo `docs/standards`. |
| `vicunav-repo-template` | Plantilla con submódulo de estándares, AGENTS, guía de contribución, plantillas de issue y PR, y CI. |
| `vicunav-hub` | Decisiones (seis ADR), estado, backlog y gobernanza. |
| `vicunav-gutenberg` | Proyecto independiente: migración de `vicunav.com` de Elementor a Gutenberg. Se gestiona en su propio repositorio. |
| `.github` | Perfil público de la organización. |

## Estándares

Los repositorios `vicunav-hub`, `vicunav-restaurante`, `vicunav-bhoga-yoga` y
`vicunav-repo-template` apuntan a `docs/standards` en el commit vigente de
`vicunav-standards`. Para verificarlo: `git submodule status` en cada repositorio.

## Verificación

El estado de ejecución de cada proyecto se consulta en sus issues y pull requests. Este
documento se actualiza al cerrar una fase, no tras cada commit.
