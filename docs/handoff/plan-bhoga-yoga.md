# Plan del vertical Yoga y migración Bhoga Yoga

Actualizado: 2026-08-26.

## Propósito

Este plan coordina cuatro superficies separadas:

- `vicunav-theme-core` como theme base compartido;
- `vicunav-yoga` como plugin vertical neutral y reusable;
- `vicunav-bhoga-yoga` como implementación privada del cliente real;
- `vicunav-demo-yoga` como demostración pública saneada.

La arquitectura está en el
[ADR 0011](../adr/0011-bhoga-yoga-cliente-privado.md) y el contrato visual aplicable
está en el [ADR 0010](../adr/0010-fidelidad-visual-bloqueante.md).

Producción es solo lectura hasta BHO-09. Las fundaciones no visuales pueden prepararse,
pero ninguna composición comienza antes de cerrar `HUB-VIS-03`.

## Mapa de propiedad

| Superficie | Propietario |
| --- | --- |
| Tokens, templates, parts, patterns y presentación reusable | `vicunav-theme-core` |
| Entidades, permisos, servicios, estado y bloques del dominio Yoga | `vicunav-yoga` |
| Settings, FAQ y testimonios transversales | `vicunav-plugin-core` |
| Marca, copy, media, rutas y composición Bhoga | `vicunav-bhoga-yoga` |
| Identidad ficticia, media licenciada y composición demostrativa | `vicunav-demo-yoga` |

## Estado observado

- Referencia: `https://bhoga.yoga/`.
- Destino Bhoga: `https://devbhogayoga.local/`.
- Producción usa WordPress, Hello Elementor y una portada Elementor.
- Rutas públicas detectadas: portada, política de privacidad y términos y
  condiciones.
- La conversión principal deriva a WhatsApp; no se observó reserva persistida.
- El usuario corrigió el idioma declarado y se verificó `lang="es"` en las tres rutas.
- El usuario confirmó derechos de copy, logo, fotografía y testimonios para la finalidad
  prevista del proyecto.
- Las URLs públicas de WhatsApp presentan variantes que requieren confirmación.
- El LocalWP para la demo reusable todavía no existe. Se recomienda
  `vicunav-demo-yoga.local`.

## Fundaciones

| Orden | ID | Repositorio | Resultado | Estado |
| ---: | --- | --- | --- | --- |
| 1 | YOGA-00 | `vicunav-hub` | Decidir separación entre theme, vertical, cliente y demo | Completado localmente |
| 2 | YOGA-01 | `vicunav-yoga` | Repositorio, estándares, bootstrap neutral 0.1.0 y contrato v1 en borrador | Completado localmente |
| 3 | BHO-01 | `vicunav-bhoga-yoga` | Repositorio privado, brief, inventario, prompts, QA y rollback | Completado localmente |
| 4 | DEMO-YOGA-01 | `vicunav-demo-yoga` | Repositorio, estándares, arquitectura y prompt de fundación | Completado localmente |

## Funnel del vertical reusable

| Orden | ID | Repositorio | Resultado | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | YOGA-02 | `vicunav-yoga` | Contrato público 1.0.0 aprobado y mapa de entidades, permisos, estados y bloques | Decisión de producto | Completado y publicado |
| 2 | YOGA-03 | `vicunav-yoga` | Entidades, administración, servicios y settings del contrato | YOGA-02 | Pendiente |
| 3 | YOGA-04 | `vicunav-yoga` | Bloques API 3, estados editor/frontend y CSS neutral | YOGA-03 y `HUB-VIS-03` | Bloqueado |
| 4 | YOGA-05 | Varios | Integración con theme core, demo y cliente real | YOGA-04 | Pendiente |

## Funnel de la implementación Bhoga

| Orden | ID | Repositorio | Resultado | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | BHO-02 | `vicunav-bhoga-yoga` | Baseline inmutable de tres rutas, estados y viewports | `HUB-VIS-03` y gate del cliente | Bloqueado |
| 2 | BHO-03 | `vicunav-bhoga-yoga` | Inventario literal de contenido, media, SEO e integraciones | BHO-02 | Pendiente |
| 3 | THEME-YOGA-01 | `vicunav-theme-core` | Deltas reusables de theme demostrados por el inventario | BHO-03 | Pendiente |
| 4 | BHO-04 | Varios | Hero calibrado con theme core y plugin Yoga | THEME-YOGA-01, YOGA-04 y BHO-03 | Pendiente |
| 5 | BHO-05 | `vicunav-bhoga-yoga` | Portada y páginas legales compuestas 1:1 | BHO-04 | Pendiente |
| 6 | BHO-06 | `vicunav-bhoga-yoga` | Elementor desactivado en local y producto Gutenberg verificado | BHO-05 | Pendiente |
| 7 | BHO-07 | `vicunav-bhoga-yoga` | Gate visual, accesible, funcional, SEO y rendimiento | BHO-06 | Pendiente |
| 8 | BHO-08 | Proyecto e infraestructura autorizada | Staging Elementor, backup y rollback ensayado | BHO-07 | Pendiente |
| 9 | BHO-09 | Infraestructura autorizada | Corte live y observación posterior | BHO-08 y aprobación humana explícita | Pendiente |

## Funnel de la demo Yoga

| Orden | ID | Repositorio | Resultado | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | DEMO-YOGA-02 | `vicunav-demo-yoga` | Identidad ficticia, copy, media licenciada y LocalWP dedicado | YOGA-02 y decisión de demo | Pendiente |
| 2 | DEMO-YOGA-03 | `vicunav-demo-yoga` | Pins exactos e instalación idempotente | THEME-YOGA-01 y YOGA-04 | Pendiente |
| 3 | DEMO-YOGA-04 | `vicunav-demo-yoga` | Rutas, composición FSE y flujos reales | DEMO-YOGA-02 y DEMO-YOGA-03 | Bloqueado por `HUB-VIS-03` |
| 4 | DEMO-YOGA-05 | `vicunav-demo-yoga` | Gate visual, accesible, funcional y de rendimiento | DEMO-YOGA-04 | Pendiente |

## Decisiones de producto requeridas

El contrato YOGA-02 quedó aprobado explícitamente el 2026-08-26:

- instructores como contenido estructurado;
- prácticas o tipos de clase como contenido estructurado;
- horarios o convocatorias editables sin cupos;
- bloques dinámicos de instructores, prácticas y horarios;
- CTA externo configurable hacia WhatsApp;
- FAQ y testimonios mediante `vicunav-plugin-core`;
- reservas, pagos y membresías fuera de v1.

## Aprendizaje continuo de la skill

Cada unidad de migración debe registrar aprendizajes verificables que puedan mejorar
`vicunav-transform-claude-to-gutenberg` sin convertir el proyecto cliente en una
dependencia del tooling. Los cambios a prompts, validadores o captura deben incluir
una regresión, medir su impacto en contexto y conservar los gates de fidelidad visual.

| Unidad | Propietario | Resultado | Estado |
| --- | --- | --- | --- |
| TOOL-YOGA-01 | `vicunav-transform-claude-to-gutenberg` | Consolidar hallazgos de BHO-03 a BHO-07 y optimizar flujo, evidencia y consumo | Pendiente |

## Gate previo del cliente

BHO-02 no comienza hasta confirmar:

- [x] alcance 1:1 frente a mejoras de diseño o conversión;
- [x] autorización de copy, logos, fotografías y testimonios;
- [x] corrección del idioma y verificación de `lang="es"`; el tratamiento de
  desviaciones accesibles se registra durante el baseline;
- números y CTA de WhatsApp aprobados;
- propiedad y continuidad de SEO, Analytics, Tag Manager y Search Console;
- hosting, DNS, CDN, cache, correo y responsables del corte;
- destino, privacidad y retención del respaldo Elementor;
- existencia de páginas, popups, formularios o templates no visibles anónimamente.

## Estrategia de respaldo Elementor

El staging o subdominio debe quedar protegido con autenticación, `noindex` y analítica
desactivada. Debe conservar base de datos, uploads, plugins, theme y configuración
compatibles, documentar su retención y pasar un smoke test antes del corte.

## Próxima unidad ejecutable

La siguiente unidad de producto es YOGA-03. BHO-02, YOGA-04 y DEMO-YOGA-04 permanecen
bloqueados por `HUB-VIS-03`; BHO-02 requiere además completar su gate operativo.
