# ADR 0011: Bhoga Yoga como implementación privada de cliente

## Contexto

Bhoga Yoga opera un sitio real en `https://bhoga.yoga/`, construido con Elementor. El
usuario solicitó migrarlo localmente a Gutenberg, sustituir después Elementor en
producción y conservar la versión anterior en un staging o subdominio de respaldo.
El LocalWP `devbhogayoga.local` ya existe y está operativo.

La inspección inicial mostró un sitio de presentación cuya conversión principal deriva a
WhatsApp y no persiste reservas ni pagos. Ese alcance no exige un plugin vertical: el
sitio se construye con los paquetes compartidos existentes y su propio child theme.

El contenido, las fotografías y los testimonios de Bhoga pertenecen a un cliente real.
No deben incorporarse a ningún paquete reusable del ecosistema.

## Alternativas consideradas

1. Crear un plugin vertical Yoga reusable y una demo pública, además de la
   implementación del cliente.
2. Crear un único theme Bhoga Yoga que contenga presentación, contenido y dominio.
3. Construir el sitio como implementación privada que consume `vicunav-theme-core` y
   las capacidades base incluidas en `vicunav-pagos`, con un child theme propio.

La primera alternativa añade un vertical sin necesidad demostrada por el alcance real
del cliente. La segunda contradice la separación entre presentación y lógica de
negocio. La tercera mantiene propiedad y privacidad observables con el mínimo de
piezas.

## Decisión

Se adopta la tercera alternativa:

| Repositorio | Responsabilidad |
| --- | --- |
| `vicunav-theme-core` | Theme base, tokens, templates, parts, patterns y presentación reusable |
| `vicunav-pagos` | Motor de pagos y, en su carpeta `core/`, capacidades base compartidas (`Vicu\Core`: settings, FAQ y testimonios) |
| `vicunav-bhoga-yoga` | Contenido real, media autorizada, identidad, rutas, composición, child theme y operación del cliente |

La implementación Bhoga consume `vicunav-theme-core` y las capacidades base de
`vicunav-pagos` mediante revisiones exactas. No usa ningún plugin vertical. La conversión a WhatsApp se mantiene como enlace directo, como
en producción.

Producción se trata como referencia de solo lectura hasta una autorización explícita
de corte. La migración visual adopta íntegramente el contrato de fidelidad 1:1 del
[ADR 0010](0010-fidelidad-visual-bloqueante.md): baseline inmutable, mapa de
propiedad, corte representativo, migración incremental con evidencia y aprobación
humana página por página del checkpoint final.

El usuario decidió el 2026-09-22 desacoplar el inicio de `BHO-02` del cierre de
`HUB-VIS-03`: la pista Bhoga puede empezar su propio baseline visual en paralelo al
rework del checkpoint de restaurante, en vez de esperar a que ese checkpoint cierre
primero. Esta decisión se tomó con el riesgo explícito de que el mismo tipo de falla
que originó `HUB-VIS-03` — declarar fidelidad 1:1 mediante métricas estructurales sin
revisión humana real, ver [plan-fidelidad-visual.md](../handoff/plan-fidelidad-visual.md)
— pueda repetirse en Bhoga si su propio checkpoint no aplica el contrato del ADR 0010
con el mismo rigor. La mitigación no es esperar a restaurante: es que el checkpoint de
Bhoga (dentro de `BHO-07`) exija la misma evidencia y aprobación humana explícita que
exige el ADR 0010 para cualquier migración, sin atajos.

## Consecuencias

- Bhoga Yoga continúa privado y separado de todo runtime reusable.
- Los valores de marca se resuelven en la implementación del cliente (composición,
  configuración y child theme), nunca en `vicunav-theme-core` ni en `vicunav-pagos`.
- El child theme de Bhoga solo es admisible como excepción explícita según el
  [ADR 0013](0013-theme-core-dinamico-agnostico.md), que desaconseja child themes como
  mecanismo normal; su aprobación como excepción queda por confirmar por el usuario.
- Antes de reemplazar Elementor debe existir un staging privado funcional, un backup
  inmutable y un rollback ensayado.

## Propagación

- El estado y backlog canónicos registran la pista Bhoga.
- El plan operativo vive en
  [`docs/handoff/plan-bhoga-yoga.md`](../handoff/plan-bhoga-yoga.md).
- `vicunav-bhoga-yoga` conserva su contrato de rollback, código, pruebas y
  documentación específica.

## Estado

Decisión actualizada el 2026-09-24 tras la limpieza del ecosistema: Bhoga ya no
depende de ningún plugin vertical. El 2026-09-22 el usuario cerró el gate operativo del
cliente (WhatsApp, integraciones, hosting y destino del backup Elementor; ver
[plan-bhoga-yoga.md](../handoff/plan-bhoga-yoga.md)) y decidió desacoplar `BHO-02` de
`HUB-VIS-03`, permitiendo que la composición visual de Bhoga empiece en paralelo al
rework del checkpoint de restaurante. El corte live continúa pendiente de `BHO-08` y
`BHO-09`.
