# ADR 0004: Estructura de repositorios

## Contexto

Se observó que un monorepo dificultaba versionar cada pieza de forma independiente y
presentarla por separado en un portafolio público.

## Decisión

Se definieron diez repositorios, uno por paquete, con los propósitos siguientes:

- `vicunav-standards`: Estándares técnicos compartidos del ecosistema.
- `vicunav-repo-template`: Plantilla base para inicializar repositorios.
- `vicunav-hub`: Documentación de arquitectura y decisiones del ecosistema.
- `vicunav-theme-core`: Patrones, tokens y templates de presentación compartidos.
- `vicunav-plugin-core`: Capacidades base compartidas por los plugins.
- `vicunav-pagos`: Motor de pagos independiente de los verticales.
- `vicunav-hotel`: Lógica del vertical hotelero y sus reservas.
- `vicunav-restaurante`: Lógica del vertical de restaurante y sus pedidos.
- `vicunav-demo-hotel`: Demostración pública del vertical hotelero.
- `vicunav-demo-restaurante`: Demostración pública del vertical de restaurante.

Se adoptó el prefijo `vicunav-` para los repositorios, orientado a la marca y a su
presentación en GitHub. Se reservó `vicu_` para el espacio de nombres de los
identificadores internos del código, incluidos hooks y post types; los namespaces PHP
usaron la raíz equivalente `Vicu`. El prefijo corto evitaba agotar el límite de 20
caracteres de los post types de WordPress.

## Consecuencias

Cada repositorio obtuvo su propio historial, versión y README. Esta distribución exigía
más disciplina de mantenimiento a cambio de una separación y una presentación más
claras.
