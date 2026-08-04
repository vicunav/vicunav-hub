# Estado canónico del ecosistema Vicunav

Actualizado: 2026-08-04.

## Cómo usar este documento

Este archivo conserva la fotografía vigente del ecosistema. Las decisiones de
arquitectura viven en [`docs/adr/`](../adr/), el trabajo pendiente vive en el
[`backlog`](backlog-ecosistema.md) y las reglas compartidas viven en
[`vicunav-standards`](../standards/).

- Responsable operativo: Codex.
- Autoridad final de producto y acciones irreversibles: usuario.
- Fuente del estado de ejecución: issues y pull requests de GitHub.
- Fuente de arquitectura y prioridades: este hub.

## Resumen actual

- Los cuatro repositorios existentes del ecosistema están disponibles y sin issues ni
  pull requests abiertos: `vicunav-hub`, `vicunav-standards`,
  `vicunav-repo-template` y `vicunav-theme-core`.
- La base funcional de `vicunav-theme-core` está terminada. Los issues 1 al 29 están
  cerrados, incluidas las correcciones del color secundario y del CPT usado en la
  documentación de plantillas verticales.
- El siguiente repositorio que debe crearse es `vicunav-plugin-core`.
- `vicunav-pagos`, `vicunav-restaurante`, `vicunav-hotel` y los dos repositorios de
  demo todavía no existen.
- `vicunav-gutenberg` pertenece a la organización Vicunav, pero es una migración
  independiente de `vicunav.com` y no forma parte de este ecosistema modular.

## Arquitectura vigente

1. **Foundation:** `vicunav-theme-core` aporta presentación compartida y
   `vicunav-plugin-core` aportará capacidades base. La lógica de negocio no vive en el
   theme.
2. **Pagos:** `vicunav-pagos` será un motor opcional, independiente de reservas y
   pedidos.
3. **Verticales:** `vicunav-restaurante` y `vicunav-hotel` serán propietarios de sus
   respectivos dominios y se comunicarán mediante contratos y hooks públicos.
4. **Demos:** `vicunav-demo-restaurante` y `vicunav-demo-hotel` compondrán los paquetes
   anteriores con contenido demostrativo, sin introducir lógica propia.

Los paquetes no leen directamente la base de datos interna de otro paquete. Cada repo
mantiene versión e historial propios. Las relaciones se expresan mediante dependencias
y contratos públicos.

## Estado por repositorio

| Repositorio | Estado | Qué contiene o debe contener | Próximo paso |
| --- | --- | --- | --- |
| `vicunav-standards` | Completo | Seguridad, nombres, Git, accesibilidad, compatibilidad, pruebas e idioma | Mantener las normas cuando una decisión transversal cambie |
| `vicunav-repo-template` | Completo | Plantilla base, guía de agentes, contribución, issue atómico, CI y submódulo de estándares | Usarlo para crear `vicunav-plugin-core` |
| `vicunav-hub` | Activo | Arquitectura, ADRs, gobierno, estado y backlog | Mantenerlo sincronizado al cerrar cada etapa |
| `vicunav-theme-core` | Base 0.1.0 completa | Tokens, templates, partes, patrones y contrato de integración | Sustituir la identidad visual placeholder cuando exista la paleta final |
| `vicunav-plugin-core` | No existe | CPT compartidos, ajustes, seguridad y REST | Crear el repositorio desde el template |
| `vicunav-pagos` | No existe | Solicitudes de pago, estados, proveedor manual y eventos | Esperar el contrato base de `plugin-core` |
| `vicunav-restaurante` | No existe | Menú, pedidos y reacción a eventos de pagos | Escribir el spec durable después de `plugin-core` y pagos |
| `vicunav-hotel` | Diferido | Reservas y disponibilidad | Mantener diferido hasta completar restaurante, según ADR 0006 |
| `vicunav-demo-restaurante` | Sitio local, sin repo | Integración real de theme, core, pagos y restaurante | Crear el repo cuando existan los paquetes que debe componer |
| `vicunav-demo-hotel` | No existe | Demostración del vertical hotelero | Esperar la implementación de hotel |

## Lo que ya existe en `vicunav-theme-core`

- Compatibilidad mínima: WordPress 6.6 y PHP 8.1.
- Tokens públicos de color, espaciado y tipografía en `theme.json`.
- Templates generales: `index`, `front-page`, `page`, `single` y `archive`.
- Dos headers: predeterminado y orientado a contacto.
- Dos footers: completo y mínimo.
- Patrones `hero-centered`, `hero-split-image`, `cta-simple`,
  `testimonials-grid`, `faq-accordion` y `contact-info`.
- Integración opcional con `Vicu\Core\Settings::get()` mediante las claves `phone`,
  `address` y `business_hours`.
- Consultas preparadas para los CPT compartidos `vicu_faq` y `vicu_testimonial`.
- Documentación para que un vertical registre plantillas con
  `register_block_template()` sin acoplarse al theme.

El detalle versionado del contrato está en
[`docs/contrato-publico.md`](https://github.com/vicunav/vicunav-theme-core/blob/main/docs/contrato-publico.md).

## Contratos previstos

### `vicunav-plugin-core`

- Namespace raíz `Vicu\Core`.
- Clase abstracta `Vicu\Core\PostType` para registro de CPT por herencia.
- CPT compartidos `vicu_faq` y `vicu_testimonial`.
- Servicio `Vicu\Core\Settings` con `get()` y capacidad de registrar pestañas.
- Menú administrativo único "Vicunav".
- Helpers reutilizables de seguridad.
- Endpoints bajo el namespace REST `vicu/v1`.

El contrato debe formalizarse en el nuevo repositorio antes de implementar código que
dependa de él.

### `vicunav-pagos`

- CPT `vicu_payment_req`.
- Estados `pendiente`, `comprobante_subido`, `confirmado`, `rechazado` y `expirado`.
- Proveedor manual en v1. La integración Mercantil queda para una versión posterior.
- Referencia externa polimórfica para no conocer reservas ni pedidos.
- Hooks `vicu_pagos_creado`, `vicu_pagos_confirmado`,
  `vicu_pagos_rechazado` y `vicu_pagos_expirado`.

El contrato de pagos debe versionarse en su repositorio antes de que un vertical lo
consuma.

## Decisiones vigentes

- La presentación y la lógica de negocio están separadas: ADR 0001.
- Pagos es un motor independiente y opcional: ADR 0002.
- Las fronteras se implementan mediante contratos y eventos: ADR 0003.
- Cada paquete vive en un repositorio independiente: ADR 0004.
- Solo se usa ACF genuino y gratuito para campos editoriales: ADR 0005.
- Restaurante se construye antes que hotel: ADR 0006.
- Los README y superficies públicas se escriben en inglés; la documentación interna y
  los comentarios de código se escriben en español.
- Qwen es una herramienta global y opcional para trabajo mecánico verificable. No es
  una dependencia ni un agente propietario de ningún repositorio.

## Entorno local

- Raíz de repositorios: `~/Documents/Codex/vicunav/`.
- LocalWP: `vicunav-demo-restaurante.local`, usado para verificar el theme y los
  paquetes futuros.
- `vicunav-theme-core` está enlazado al sitio LocalWP mediante symlink.
- Los estándares compartidos se consumen como submódulo en `docs/standards/`.

## Qué falta

1. Crear `vicunav-plugin-core` y versionar su contrato público.
2. Implementar y probar las capacidades base de `plugin-core`.
3. Crear `vicunav-pagos` y cerrar su contrato antes de escribir los verticales.
4. Redactar un spec durable de restaurante y convertirlo en issues atómicos.
5. Implementar restaurante y publicar su demo.
6. Sustituir la paleta placeholder del theme cuando termine la decisión de marca.
7. Diseñar e implementar hotel y su demo después de validar restaurante.

La única siguiente acción ejecutable está detallada en el
[`backlog`](backlog-ecosistema.md).
