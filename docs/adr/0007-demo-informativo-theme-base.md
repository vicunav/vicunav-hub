# ADR 0007: Demo informativo sobre el theme base

## Contexto

El ecosistema necesita validar `vicunav-theme-core` en un sitio profesional que no
dependa de reservas, pedidos ni pagos. El proyecto privado de Dra. Yanalith Fortul ya
tiene estrategia y contenido definidos, pero su capa visual y su theme local estaban
en una etapa temprana. Esto permite adoptar la arquitectura compartida sin arrastrar
una migración visual costosa.

Los diseños de restaurante, hotel y Dra. Fortul también funcionan como insumos de
descubrimiento. Pueden revelar tokens, patterns, settings e interacciones reutilizables,
además de requisitos específicos que no deben incorporarse automáticamente al núcleo.

## Decisión

Se incorpora `vicunav-demo-informativo` al mapa del ecosistema. Dra. Fortul será su
implementación de referencia mientras permanezca en desarrollo privado.

El demo:

- consume `vicunav-theme-core` sin copiarlo ni bifurcarlo;
- puede consumir `vicunav-plugin-core` cuando requiera capacidades transversales;
- no depende de `vicunav-pagos`, `vicunav-restaurante` ni `vicunav-hotel`;
- conserva en su propio proyecto el contenido, la estrategia, la composición y las
  restricciones médicas;
- clasifica cada hallazgo del diseño antes de crear Issues de implementación.

La clasificación obligatoria es:

| Hallazgo | Propietario |
| --- | --- |
| Token, template, pattern o estilo reutilizable | `vicunav-theme-core` |
| Setting, contenido estructurado o comportamiento transversal | `vicunav-plugin-core` |
| Lógica de reservas, pedidos o pagos | Vertical o motor correspondiente |
| Composición y contenido del demo | `vicunav-demo-informativo` |
| Requisito médico exclusivo | Proyecto Dra. Fortul o plugin específico aprobado |

## Consecuencias

- El theme local precario de Dra. Fortul se retira y LocalWP enlaza el repositorio
  compartido.
- El diseño aprobado podrá mejorar el theme base, pero no convierte cada decisión
  visual o interacción de un demo en una capacidad común.
- Los contratos de backend se definen después de identificar estado, datos, permisos y
  propietario; no se deducen solo de una pantalla o un botón del prototipo.
- El repositorio actual de Dra. Fortul permanece privado y conserva su nombre hasta una
  decisión separada sobre saneamiento, publicación y transferencia a la organización.
- La pista del demo no bloquea la fundación ni pagos. CORE-01 a CORE-09 se completaron
  mientras continúan pendientes los handoffs visuales.
