# ADR 0015: Un theme y un plugin propios por proyecto

Estado: aceptado el 2026-09-24. Sustituye al [ADR 0013](0013-theme-core-dinamico-agnostico.md)
y a la parte del [ADR 0002](0002-pagos-motor-independiente.md) que definía pagos como
motor compartido entre verticales.

## Contexto

El ecosistema se organizó alrededor de paquetes compartidos: un theme base
(`vicunav-theme-core`), un motor de pagos y unas capacidades base (`Vicu\Core`)
consumidos por varios proyectos. En la práctica esa separación multiplicó repositorios,
contratos, revisiones fijadas y contexto que cada cambio debía coordinar, sin que
existiera más de un consumidor real por paquete: el theme base solo lo usan dos
proyectos mediante child themes, y pagos y core solo los usa el vertical de restaurante.

## Decisión

Cada proyecto tiene su propio theme y su propio plugin, dentro de su propio repositorio:

- No existe un theme compartido entre proyectos. Un proyecto que hoy usa
  `vicunav-theme-core` recibe una copia de su versión vigente, renombrada, fusionada con
  su child theme y convertida en el theme específico del proyecto.
- No existen un plugin de pagos ni un plugin core compartidos. Cada proyecto contiene un
  único plugin con la lógica que necesita (dominio, pagos, ajustes, contenido).
- Los estándares transversales (`vicunav-standards`) y la plantilla
  (`vicunav-repo-template`) siguen siendo compartidos porque no son código de ejecución.

## Consecuencias

- Desaparecen `vicunav-theme-core`, `vicunav-pagos` y, en su momento, el concepto de
  capas base compartidas del hub; el hub deja de describir contratos entre paquetes de
  ejecución y documenta proyectos y decisiones.
- Las mejoras reusables ya no se propagan por dependencia: se copian conscientemente de
  un proyecto a otro.
- Los contratos públicos entre paquetes (ADR 0003) se reducen a las fronteras internas
  de cada plugin.
- La ejecución está planificada en
  [`plan-un-theme-y-plugin-por-proyecto.md`](../handoff/plan-un-theme-y-plugin-por-proyecto.md).
