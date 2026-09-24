# ADR 0002: Estructura de repositorios y prefijos

## Contexto

Cada proyecto necesita versionarse y presentarse por separado, con historial propio.

## Decisión

Un repositorio por proyecto y uno por función de soporte, todos con el prefijo
`vicunav-`:

- `vicunav-restaurante`: proyecto de referencia de restaurante (público).
- `vicunav-bhoga-yoga`: implementación privada de un cliente real.
- `vicunav-standards`: estándares técnicos compartidos, incluidos como submódulo
  `docs/standards` en cada repositorio.
- `vicunav-repo-template`: plantilla para repositorios nuevos.
- `vicunav-hub`: decisiones y estado.
- `vicunav-gutenberg`: migración independiente de `vicunav.com`.
- `.github`: perfil público de la organización.

Los identificadores internos del código (hooks, opciones, post types) usan el prefijo
`vicu_`; los namespaces PHP usan la raíz `Vicu`. El prefijo corto respeta el límite de 20
caracteres de los post types de WordPress.

## Consecuencias

Cada repositorio tiene su historial, versión y README. Los proyectos nuevos parten de
`vicunav-repo-template`, con su propio theme y plugin.
