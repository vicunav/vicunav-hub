# Estado canónico del ecosistema Vicunav

## Estado del documento

- Responsable operativo: Codex.
- Autoridad de producto y acciones irreversibles: usuario.
- Última auditoría local parcial: 2026-08-03.
- Auditoría histórica de Claude: pendiente.
- Fuente para ejecución: issues y pull requests enlazados desde el
  [backlog](backlog-ecosistema.md).

Este documento es la memoria durable multirrepositorio. Debe describir hechos
verificados y enlazar normas o ADRs; no reemplaza esas fuentes ni los issues.

## Propósito y arquitectura vigente

Vicunav es un ecosistema modular de WordPress que separa presentación, capacidades
base, pagos y lógica vertical. El mapa general y los repositorios previstos están en
el [README del hub](../../README.md). Las decisiones vinculantes están en el
[índice de ADRs](../adr/README.md).

Orden conceptual actual:

1. `vicunav-theme-core` aporta presentación compartida mediante un theme de bloques.
2. `vicunav-plugin-core` concentra capacidades base sin asumir una vertical.
3. `vicunav-pagos` funciona como motor independiente mediante contratos públicos.
4. `vicunav-restaurante` y `vicunav-hotel` encapsulan sus dominios; restaurante tiene
   prioridad según el ADR 0006.
5. Los repositorios demo integran capas sin convertirse en fuentes de lógica común.

## Inventario inicial por verificar con Claude

| Repositorio | Presencia local verificada | Estado publicado en el hub | Auditoría histórica |
| --- | --- | --- | --- |
| `vicunav-standards` | Sí | Disponible | Pendiente |
| `vicunav-repo-template` | Sí | Disponible | Pendiente |
| `vicunav-hub` | Sí | Disponible | Pendiente |
| `vicunav-theme-core` | Sí | En progreso | Pendiente |
| `vicunav-plugin-core` | No observada | Pendiente | Pendiente |
| `vicunav-pagos` | No observada | Pendiente | Pendiente |
| `vicunav-hotel` | No observada | Pendiente | Pendiente |
| `vicunav-restaurante` | No observada | Pendiente | Pendiente |
| `vicunav-demo-hotel` | No observada como repo | Pendiente | Pendiente |
| `vicunav-demo-restaurante` | No observada como repo | Pendiente | Pendiente |
| `vicunav-gutenberg` | Sí | No clasificado en el README | Pendiente |
| `mariovicunadev` | Sí | No clasificado en el README | Pendiente |
| `vicunav-github-profile` | Sí | No clasificado en el README | Pendiente |

La ausencia en el workspace observado no demuestra por sí sola que el repo no exista
en GitHub ni que su estado funcional coincida con el README.

## Contratos y decisiones

Claude debe completar esta sección con enlaces concretos y explicar, sin copiar el
texto normativo:

- límites entre theme, capacidades base, pagos y verticales;
- contratos públicos, hooks, namespaces y propiedad de datos;
- compatibilidad y estrategia de pruebas;
- orden de implementación y dependencias entre repositorios;
- decisiones descartadas que podrían reaparecer por falta de contexto.

## Entornos de desarrollo

Estado conocido: existe un sitio LocalWP denominado
`vicunav-demo-restaurante.local` que se ha usado para probar
`vicunav-theme-core` mediante symlink. Claude debe confirmar rutas, componentes
activos, contenido de prueba, versión de PHP/WordPress, estado de logs y cualquier
otro entorno que haya utilizado. No se registran contraseñas ni salts.

## Riesgos, deuda y preguntas abiertas

Pendiente de la auditoría de Claude. Cada punto debe indicar su fuente, impacto,
repositorio propietario y si bloquea el siguiente issue.

## Siguiente acción recomendada

Pendiente del cierre de Claude. Debe quedar exactamente un issue verificable y sin
bloqueos, enlazado desde el backlog.
