# Plan de la migración Bhoga Yoga

Actualizado: 2026-09-24.

## Propósito

Este plan coordina la migración del sitio real de Bhoga Yoga, una implementación
privada de cliente que se construye con:

- `vicunav-theme-core` como theme base compartido;
- las capacidades base `Vicu\Core` (settings, FAQ y testimonios), incluidas en la
  carpeta `core/` de `vicunav-pagos`;
- `vicunav-bhoga-yoga` como repositorio privado con contenido, media, composición y
  child theme del cliente.

La arquitectura está en el
[ADR 0011](../adr/0011-bhoga-yoga-cliente-privado.md) y el contrato visual aplicable
está en el [ADR 0010](../adr/0010-fidelidad-visual-bloqueante.md).

Producción es solo lectura hasta BHO-09. La composición visual de Bhoga puede empezar
sin esperar el cierre de `HUB-VIS-03` (decisión del 2026-09-22, ver ADR 0011).

## Mapa de propiedad

| Superficie | Propietario |
| --- | --- |
| Tokens, templates, parts, patterns y presentación reusable | `vicunav-theme-core` |
| Settings, FAQ y testimonios transversales | `vicunav-pagos` (carpeta `core/`) |
| Marca, copy, media, rutas, composición y child theme Bhoga | `vicunav-bhoga-yoga` |

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

## Fundaciones

| Orden | ID | Repositorio | Resultado | Estado |
| ---: | --- | --- | --- | --- |
| 1 | BHO-01 | `vicunav-bhoga-yoga` | Repositorio privado, brief, inventario, prompts, QA y rollback | Completado localmente |

## Funnel de la implementación Bhoga

| Orden | ID | Repositorio | Resultado | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | BHO-02 | `vicunav-bhoga-yoga` | Baseline inmutable de tres rutas, estados y viewports | Gate del cliente | Listo para empezar |
| 2 | BHO-03 | `vicunav-bhoga-yoga` | Inventario literal de contenido, media, SEO e integraciones | BHO-02 | Pendiente |
| 3 | BHO-04 | `vicunav-bhoga-yoga` | Hero calibrado con theme-core y las capacidades base | BHO-03 | Pendiente |
| 4 | BHO-05 | `vicunav-bhoga-yoga` | Portada y páginas legales compuestas 1:1 | BHO-04 | Pendiente |
| 5 | BHO-06 | `vicunav-bhoga-yoga` | Elementor desactivado en local y producto Gutenberg verificado | BHO-05 | Pendiente |
| 6 | BHO-07 | `vicunav-bhoga-yoga` | Gate visual, accesible, funcional, SEO y rendimiento | BHO-06 | Pendiente |
| 7 | BHO-08 | Proyecto e infraestructura autorizada | Staging Elementor, backup y rollback ensayado | BHO-07 | Pendiente |
| 8 | BHO-09 | Infraestructura autorizada | Corte live y observación posterior | BHO-08 y aprobación humana explícita | Pendiente |

Cualquier delta reusable que el inventario demuestre en el theme se abre como issue
separado en `vicunav-theme-core`, abstraído de forma neutral según el
[ADR 0013](../adr/0013-theme-core-dinamico-agnostico.md).

## Gate previo del cliente

BHO-02 no comienza hasta confirmar:

- [x] alcance 1:1 frente a mejoras de diseño o conversión;
- [x] autorización de copy, logos, fotografías y testimonios;
- [x] corrección del idioma y verificación de `lang="es"`; el tratamiento de
  desviaciones accesibles se registra durante el baseline;
- [x] números y CTA de WhatsApp aprobados: el usuario confirmó el 2026-09-22
  mantener el mismo mecanismo y número(s) que produce hoy `bhoga.yoga` (enlace
  directo); el número exacto se verifica literal durante BHO-03;
- [x] propiedad y continuidad de SEO, Analytics, Tag Manager y Search Console:
  el usuario confirmó el 2026-09-22 preservar tal cual las integraciones ya
  detectadas en producción (Google Tag Manager/Site Kit, Google Maps,
  Instagram), sin sumar ninguna nueva y sin cambiar cuentas ni configuración;
  la optimización de assets de SiteGround es del hosting y no aplica al código
  migrado; falta verificar IDs y claves exactos durante BHO-03;
- [x] hosting, DNS, CDN, cache, correo y responsables del corte: el usuario
  confirmó el 2026-09-22 mantener el hosting actual de producción, sin
  migración de proveedor;
- [x] destino, privacidad y retención del respaldo Elementor: el usuario
  confirmó el 2026-09-22 un subdominio protegido (autenticación, `noindex`,
  analítica desactivada) en el mismo hosting, según la estrategia ya descrita
  más abajo;
- existencia de páginas, popups, formularios o templates no visibles
  anónimamente: pendiente de verificar durante el baseline de BHO-02.

Gate operativo del cliente cerrado el 2026-09-22, salvo el último punto (se
verifica como parte del propio baseline, no bloquea su inicio). El usuario decidió
además el 2026-09-22 desacoplar BHO-02 del cierre de `HUB-VIS-03` (ver
[ADR 0011](../adr/0011-bhoga-yoga-cliente-privado.md)): BHO-02 puede arrancar ahora,
en paralelo al rework del checkpoint de restaurante. El checkpoint propio de Bhoga
(`BHO-07`) sigue exigiendo el mismo rigor del ADR 0010 sin excepciones: baseline
inmutable, evidencia por página y estado, y aprobación humana explícita antes de
declarar paridad 1:1.

## Estrategia de respaldo Elementor

El staging o subdominio debe quedar protegido con autenticación, `noindex` y analítica
desactivada. Debe conservar base de datos, uploads, plugins, theme y configuración
compatibles, documentar su retención y pasar un smoke test antes del corte.

## Próxima unidad ejecutable

`BHO-02` no depende de `HUB-VIS-03` (decisión del usuario del 2026-09-22) y su gate
operativo está cerrado: es la próxima unidad ejecutable de la pista Bhoga.
