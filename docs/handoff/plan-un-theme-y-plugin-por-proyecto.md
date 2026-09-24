# Plan: un theme y un plugin propios por proyecto

Actualizado: 2026-09-24. Decisión: [ADR 0015](../adr/0015-un-theme-y-un-plugin-por-proyecto.md).

Este plan solo ordena el trabajo; no se ha ejecutado nada de lo que describe. Los IDs
son referencias de planificación hasta que exista el issue de GitHub correspondiente.

## Punto de partida

| Pieza compartida | Consumidores reales hoy |
| --- | --- |
| `vicunav-theme-core` (theme base) | `vicunav-demo-restaurante` (child `vicunav-bonasera`) y `vicunav-bhoga-yoga` (child `bhoga-yoga`) |
| `vicunav-pagos` (incluye `Vicu\Core`) | `vicunav-restaurante`, y por ello `vicunav-demo-restaurante`; `vicunav-bhoga-yoga` usa solo `Vicu\Core` (ajustes, FAQ, testimonios) |
| `vicunav-restaurante` (plugin de dominio) | `vicunav-demo-restaurante` |

## Estado final buscado

- Cada proyecto contiene su theme (un único theme, sin `Template:` padre) y su plugin (un
  único plugin) en su repositorio.
- No existen `vicunav-theme-core` ni `vicunav-pagos` como repositorios ni como
  dependencias; el hub no los menciona salvo en el historial de los ADR.
- `vicunav-standards` y `vicunav-repo-template` se conservan.

## Riesgos que fijan el orden

1. **Global Styles y ajustes ligados al slug del theme.** WordPress guarda Global Styles,
   `theme_mods` y personalizaciones del Site Editor por el slug del theme activo. Renombrar
   un theme en un sitio con contenido pierde esos datos si no se migran. Los sitios locales
   se reconstruyen desde cero con el instalador del demo; cualquier sitio con datos reales
   requiere migración explícita antes de renombrar.
2. **Slugs de patterns y bloques.** Los patterns y template parts usan el prefijo
   `vicunav-theme-core/`. El contenido guardado que los referencia debe reescribirse al
   nuevo prefijo en la misma versión.
3. **Fidelidad visual (ADR 0010).** Fusionar theme base y child no puede cambiar el render:
   cada proyecto debe demostrar equivalencia con su baseline antes de dar por terminado el
   paso. Restaurante sigue con su checkpoint reabierto: no se altera hasta cerrarlo o hasta
   que el usuario decida hacerlo en paralelo.
4. **Datos persistidos del core.** Para no migrar datos, se conservan los identificadores
   `vicu_faq`, `vicu_testimonial`, la opción `vicu_core_settings` y el namespace interno
   mientras existan sitios con datos.

## Fases

### Fase 0: decisiones pendientes del usuario

- Nombre final del proyecto/repositorio de restaurante y su theme. Propuesta: el proyecto
  restaurante es `vicunav-demo-restaurante` (theme `vicunav-bonasera`), y absorbe el
  plugin `vicunav-restaurante`; alternativa: mantener los dos repositorios, el plugin
  con pagos y core internos y el demo con su propio theme.
- Nombre del theme y plugin de Bhoga (propuesta: theme `bhoga-yoga` y plugin `bhoga-yoga`
  dentro de `vicunav-bhoga-yoga`).
- Si hay sitios con datos reales de Global Styles que migrar antes de renombrar.

### Fase 1: plugin único por proyecto

| ID | Repositorio | Resultado | Depende de |
| --- | --- | --- | --- |
| PROJ-01 | proyecto restaurante | Plugin único = dominio restaurante + pagos manual + `Vicu\Core`; una sola cabecera, un solo bootstrap, sin `Requires Plugins`; tests fusionados y verdes | Fase 0; PR de absorción de core en pagos integrado |
| PROJ-02 | `vicunav-bhoga-yoga` | Plugin propio con ajustes, FAQ y testimonios (`Vicu\Core` reducido a lo que usa Bhoga) | Fase 0 |
| PROJ-03 | proyectos | Instalador y `config/dependencies.json` sin `vicunav-pagos`; revisiones fijadas actualizadas | PROJ-01, PROJ-02 |

### Fase 2: theme único por proyecto

| ID | Repositorio | Resultado | Depende de |
| --- | --- | --- | --- |
| PROJ-04 | proyecto restaurante | Copia de la versión vigente de `vicunav-theme-core` fusionada con `vicunav-bonasera`; sin `Template:`; slug, text domain, prefijos de pattern y handles renombrados; el resto del render idéntico | Fase 0 |
| PROJ-05 | `vicunav-bhoga-yoga` | Igual con el child `bhoga-yoga` | Fase 0 |
| PROJ-06 | ambos | Verificación de equivalencia visual contra el baseline de cada proyecto y aprobación humana (ADR 0010) | PROJ-04, PROJ-05 |

### Fase 3: retirada

| ID | Repositorio | Resultado | Depende de |
| --- | --- | --- | --- |
| PROJ-07 | `vicunav-hub` | Reescribir README, estado, backlog y gobernanza para describir proyectos, no paquetes compartidos; retirar de la tabla `vicunav-theme-core` y `vicunav-pagos` | PROJ-03, PROJ-06 |
| PROJ-08 | `vicunav-standards` y `vicunav-repo-template` | Quitar referencias a theme-core, pagos y core; bump del submódulo en los consumidores | PROJ-07 |
| PROJ-09 | GitHub | Archivar y, con confirmación explícita del usuario, borrar `vicunav-theme-core` y `vicunav-pagos` | PROJ-08 |

## Criterios de cierre

- Ningún repositorio ni manifiesto nombra `vicunav-theme-core`, `vicunav-pagos` ni
  `vicunav-plugin-core`; `grep` sobre todos los repos devuelve cero.
- Cada proyecto se instala desde cero con su instalador y pasa sus suites (PHPUnit,
  phpcs, QA de runtime) sin dependencias de otro repositorio.
- La equivalencia visual de cada theme está registrada y aprobada.
