# ADR 0001: Separación entre theme y plugin dentro de cada proyecto

## Contexto

Un sitio debe poder cambiar de diseño sin perder lógica de negocio, y su lógica no debe
depender de la presentación. Cada proyecto Vicunav es un repositorio autocontenido con
su propio theme de bloques y su propio plugin.

## Decisión

Dentro de cada proyecto:

- `theme/` contiene únicamente presentación: `theme.json`, tokens, templates, template
  parts, patterns y estilos.
- `plugin/` contiene la lógica de negocio: tipos de contenido, reglas, datos
  transaccionales, REST y bloques dinámicos.

No se comparte código entre proyectos por dependencia. Un proyecto nuevo crea su theme y
su plugin desde cero.

## Consecuencias

- Un CPT, una regla de negocio o un dato transaccional en el theme viola esta decisión.
- El theme puede reemplazarse sin afectar la lógica; el plugin no consulta el theme.
- Un bloque de dominio consume presets y propiedades públicas del theme con fallbacks
  neutrales, sin literales de marca.
