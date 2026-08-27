# ADR 0011: Bhoga Yoga, vertical reusable y demo Yoga

## Contexto

Bhoga Yoga opera un sitio real en `https://bhoga.yoga/`, construido con Elementor. El
usuario solicitó migrarlo localmente a Gutenberg, sustituir después Elementor en
producción y conservar la versión anterior en un staging o subdominio de respaldo.
El LocalWP `devbhogayoga.local` ya existe y está operativo.

La inspección inicial mostró un sitio informativo cuya conversión principal deriva a
WhatsApp. Por sí sola, esa observación no exigía un plugin vertical. El usuario aclaró
después que el objetivo de producto sí incluye una base reusable para futuros clientes
del nicho Yoga y una demo pública independiente.

El contenido, las fotografías y los testimonios de Bhoga pertenecen a un cliente real.
No deben confundirse con datos ficticios de una demo pública ni incorporarse al
runtime reusable.

## Alternativas consideradas

1. Mantener toda la migración, incluida la lógica reusable, dentro de
   `vicunav-bhoga-yoga`.
2. Crear un único theme Bhoga Yoga que contenga presentación, contenido y dominio.
3. Separar theme base, plugin vertical, implementación cliente y demo saneada.

La primera alternativa acopla futuros clientes a una marca real. La segunda contradice
la separación entre presentación y lógica de negocio. La tercera mantiene propiedad,
privacidad y reutilización observables.

## Decisión

Se adopta la tercera alternativa con cuatro propietarios:

| Repositorio | Responsabilidad |
| --- | --- |
| `vicunav-theme-core` | Theme base, tokens, templates, parts, patterns y presentación reusable |
| `vicunav-yoga` | Plugin neutral con entidades, servicios, permisos, estado y bloques del dominio Yoga |
| `vicunav-bhoga-yoga` | Contenido real, media autorizada, identidad, rutas, composición y operación del cliente |
| `vicunav-demo-yoga` | Contenido ficticio o saneado, media licenciada y composición pública demostrativa |

`vicunav-plugin-core` continúa como propietario de settings, FAQ y testimonios
transversales cuando su contrato cubra el requisito. `vicunav-yoga` no duplica esas
capacidades ni contiene literales Bhoga.

La implementación Bhoga consume obligatoriamente `vicunav-theme-core` y
`vicunav-yoga` mediante revisiones exactas. La demo Yoga consume esos paquetes y
`vicunav-plugin-core` desde un LocalWP separado.

El contrato funcional 1.0.0 de `vicunav-yoga` quedó aprobado el 2026-08-26 antes de
registrar CPT, tablas, endpoints o bloques. El bootstrap inicial existe sin dominio
persistido. El alcance aprobado cubre instructores, prácticas, horarios o convocatorias
y conversión externa a WhatsApp; reservas, pagos y membresías permanecen fuera.

Producción se trata como referencia de solo lectura hasta una autorización explícita
de corte. La migración visual adopta el
[ADR 0010](0010-fidelidad-visual-bloqueante.md) y permanece bloqueada hasta cerrar
`HUB-VIS-03`. Las fundaciones no visuales de los repositorios no eluden ese gate.

## Consecuencias

- El ecosistema incorpora `vicunav-yoga` como vertical público y
  `vicunav-demo-yoga` como website demo público.
- Bhoga Yoga continúa privado y separado del runtime reusable.
- Los valores de marca se resuelven como composición o configuración del consumidor,
  nunca como literales en el plugin.
- Los bloques del vertical usan API version 3, registro por metadata y CSS funcional
  neutral que consume presets públicos del theme.
- La demo requiere identidad independiente y un LocalWP distinto de
  `devbhogayoga.local`.
- Antes de reemplazar Elementor debe existir un staging privado funcional, un backup
  inmutable y un rollback ensayado.

## Propagación

- El estado y backlog canónicos registran los tres repositorios Yoga.
- El plan operativo vive en
  [`docs/handoff/plan-bhoga-yoga.md`](../handoff/plan-bhoga-yoga.md).
- Cada repositorio conserva su contrato, código, pruebas y documentación específica.

## Estado

Decisión actualizada el 2026-08-26 tras la aclaración del usuario. Las fundaciones de
los tres repositorios están publicadas según su visibilidad acordada y el contrato
1.0.0 está aprobado. La composición visual y el corte live continúan pendientes de
sus gates.
