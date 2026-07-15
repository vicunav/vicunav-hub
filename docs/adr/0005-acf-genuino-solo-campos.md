# ADR 0005: ACF genuino solo para campos

## Contexto

Se valoró ACF como una habilidad muy solicitada en el mercado. En octubre de 2024,
WordPress.org bifurcó Advanced Custom Fields (ACF) como Secure Custom Fields (SCF) tras
el conflicto entre WP Engine y Automattic. SCF pasó a ofrecer sin costo funciones que
ACF reservaba a su versión de pago, lo que introdujo para el proyecto una zona gris
legal y ética sobre su procedencia y uso.

## Decisión

Se decidió usar únicamente la versión gratuita y genuina de ACF, distribuida desde
`advancedcustomfields.com`, y nunca SCF. Esta elección evitaba la ambigüedad en
repositorios públicos publicados con un nombre propio.

ACF se usó solo para campos que el dueño del negocio editaba directamente, como precio,
fotos u horario. El registro de cada CPT permaneció como código propio mediante la clase
abstracta de `vicunav-plugin-core`.

Los grupos de campos se versionaron como código en `acf-json/`; nunca quedaron
configurados únicamente mediante la interfaz visual.

## Consecuencias

Al limitarse a ACF gratuito, no se dispuso de Repeater ni Flexible Content. Los datos
repetibles, como la galería de una habitación o los ingredientes de un plato, se
modelaron mediante CPTs relacionados o taxonomías nativas, no mediante ACF Pro.

La decisión quedó como reversible en cada repositorio: si uno dejaba de necesitar ACF,
podía retirarlo sin afectar al resto. ACF no se convirtió en una dependencia obligatoria
del ecosistema.
