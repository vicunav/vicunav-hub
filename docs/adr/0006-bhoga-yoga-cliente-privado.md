# ADR 0006: Bhoga Yoga como implementación privada de cliente

## Contexto

Bhoga Yoga opera un sitio real en `https://bhoga.yoga/`, construido con Elementor, cuya
conversión principal deriva a WhatsApp sin persistir reservas ni pagos. Se migra
localmente a Gutenberg y, tras un corte autorizado, sustituirá a Elementor en
producción. Su contenido, fotografías y testimonios pertenecen a un cliente real.

## Decisión

`vicunav-bhoga-yoga` es un repositorio privado y autocontenido:

| Ruta | Responsabilidad |
| --- | --- |
| `theme/` | Theme de bloques `bhoga-yoga`, sin theme padre |
| `plugin/` | Plugin `bhoga-yoga-content`: FAQ, testimonios y ajustes |
| `content/` | Contenido y media autorizados del cliente |
| `docs/`, `tests/`, `GATES.md` | Evidencia, gates, QA y operación |

No depende de ningún otro repositorio ni contiene código reusable por otros proyectos.
La conversión a WhatsApp se mantiene como enlace directo, como en producción.

Producción es una referencia de solo lectura hasta una autorización explícita de corte.
La migración adopta el criterio de fidelidad 1:1 del
[ADR 0005](0005-fidelidad-visual-bloqueante.md): baseline inmutable, evidencia por
página y estado, y aprobación humana explícita en su propio gate.

## Consecuencias

- Bhoga Yoga permanece privado; su contenido y media no se incorporan a otros repositorios.
- Antes de reemplazar Elementor deben existir un staging privado funcional, un backup
  inmutable y un rollback ensayado.
- El ensayo en el hosting real y el corte live los ejecuta el propietario manualmente,
  fuera de este repositorio.
- El roadmap y los gates del proyecto viven en `vicunav-bhoga-yoga`.
