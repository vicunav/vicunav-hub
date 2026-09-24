# ADR 0003: ACF genuino solo para campos editoriales

## Contexto

En octubre de 2024 WordPress.org bifurcó Advanced Custom Fields como Secure Custom
Fields (SCF) tras el conflicto entre WP Engine y Automattic. SCF ofrece sin costo
funciones que ACF reserva a su versión de pago, lo que crea una zona gris legal y ética
para repositorios públicos con nombre propio.

## Decisión

Cuando un proyecto usa ACF, es únicamente la versión gratuita y genuina distribuida desde
`advancedcustomfields.com`, nunca SCF. ACF se limita a campos que el dueño del negocio
edita directamente, como precio, fotos u horario.

El registro de cada tipo de contenido es código propio del plugin. Los grupos de campos
se versionan como código en `acf-json/`; nunca quedan configurados solo en la interfaz.

## Consecuencias

Sin Repeater ni Flexible Content, los datos repetibles se modelan con tipos de contenido
relacionados o taxonomías nativas. ACF no es dependencia obligatoria: cada proyecto puede
prescindir de él sin afectar a otros.
