# Backlog multirrepositorio de Vicunav

Actualizado: 2026-08-04.

## Propósito

Este archivo ordena únicamente el trabajo pendiente que cruza repositorios. Cuando
exista un issue en GitHub, el issue será la fuente de su estado, alcance y aceptación.
Los identificadores de esta tabla son referencias de planificación, no números de
issue.

## Trabajo completado

| Repositorio | Resultado vigente |
| --- | --- |
| `vicunav-standards` | Siete estándares compartidos publicados; sin issues abiertos |
| `vicunav-repo-template` | Template utilizable con submódulo, AGENTS, contribución, issue atómico y CI |
| `vicunav-hub` | Seis ADRs, gobierno, estado y backlog consolidados |
| `vicunav-theme-core` | Base 0.1.0 completa; issues 1 al 29 cerrados y sin PRs abiertos |

Las antiguas tareas para diferenciar `vicunav-secondary` y corregir el CPT de
`plantillas-verticales.md` ya están resueltas en los issues 27 y 29 de
`vicunav-theme-core`.

## Orden de ejecución

| Orden | ID | Repositorio | Trabajo | Depende de | Estado |
| ---: | --- | --- | --- | --- | --- |
| 1 | CORE-01 | `vicunav-plugin-core` | Crear el repositorio desde `vicunav-repo-template` | Ninguna | Siguiente |
| 2 | CORE-02 | `vicunav-plugin-core` | Versionar contrato público y bootstrap técnico con pruebas | CORE-01 | Por crear |
| 3 | CORE-03 | `vicunav-plugin-core` | Implementar `Vicu\Core\PostType` | CORE-02 | Por crear |
| 4 | CORE-04 | `vicunav-plugin-core` | Registrar CPT `vicu_faq` | CORE-03 | Por crear |
| 5 | CORE-05 | `vicunav-plugin-core` | Registrar CPT `vicu_testimonial` | CORE-03 | Por crear |
| 6 | CORE-06 | `vicunav-plugin-core` | Implementar `Vicu\Core\Settings` | CORE-02 | Por crear |
| 7 | CORE-07 | `vicunav-plugin-core` | Crear menú Vicunav y `Settings::register_tab()` | CORE-06 | Por crear |
| 8 | CORE-08 | `vicunav-plugin-core` | Implementar helpers de seguridad | CORE-02 | Por crear |
| 9 | CORE-09 | `vicunav-plugin-core` | Crear base REST bajo `vicu/v1` | CORE-02 | Por crear |
| 10 | PAGOS-01 | `vicunav-pagos` | Crear repo, contrato y CPT `vicu_payment_req` | CORE-03, CORE-08, CORE-09 | Por crear |
| 11 | PAGOS-02 | `vicunav-pagos` | Implementar estados, expiración y eventos públicos | PAGOS-01 | Por crear |
| 12 | PAGOS-03 | `vicunav-pagos` | Implementar proveedor manual v1 | PAGOS-02 | Por crear |
| 13 | REST-01 | `vicunav-restaurante` | Escribir spec durable y descomponerlo en issues | CORE-02, PAGOS-02 | Por crear |
| 14 | REST-02 | `vicunav-restaurante` | Crear repo e implementar menú y pedidos | REST-01 | Por descomponer |
| 15 | DEMO-REST-01 | `vicunav-demo-restaurante` | Versionar la composición del demo LocalWP | REST-02, PAGOS-03 | Por descomponer |
| 16 | HOTEL-01 | `vicunav-hotel` | Escribir spec del vertical hotelero | DEMO-REST-01 | Diferido por ADR 0006 |
| 17 | DEMO-HOTEL-01 | `vicunav-demo-hotel` | Crear la demo del vertical hotelero | HOTEL-01 | Diferido |

## Siguiente issue: CORE-01

### Objetivo

Crear el repositorio público `vicunav-plugin-core` desde
`vicunav-repo-template`, sin introducir todavía lógica de negocio ni contratos
incompletos.

### Alcance

- Crear el repositorio bajo la organización Vicunav usando el template actual.
- Clonarlo en `~/Documents/Codex/vicunav/vicunav-plugin-core`.
- Sustituir los placeholders de `README.md` y `AGENTS.md` por el propósito real.
- Conservar `CONTRIBUTING.md`, las plantillas de GitHub y el workflow de lint.
- Inicializar `docs/standards/` en el commit vigente de `vicunav-standards`.
- Confirmar la configuración de squash-merge y protección de `main` antes del primer
  cambio funcional.

### Fuera de alcance

- Registrar CPTs.
- Implementar ajustes, endpoints REST o helpers.
- Crear el repositorio de pagos.
- Definir interfaces no confirmadas por el contrato actual.

### Criterios de aceptación

- [ ] El repo existe en `github.com/vicunav/vicunav-plugin-core` y es público.
- [ ] No quedan placeholders del template.
- [ ] El README público está en inglés y explica el propósito del plugin.
- [ ] La documentación interna está en español.
- [ ] El submódulo de estándares apunta a su versión vigente.
- [ ] La rama `main` solo admite squash-merge mediante pull request.
- [ ] El working tree queda limpio después del merge.

### Validación

- Inspeccionar la estructura local y la página pública del repositorio.
- Ejecutar `git submodule status` y comprobar el commit esperado.
- Buscar `{{` y `}}` en los archivos versionados; el resultado debe ser cero.
- Confirmar en GitHub que no esté permitido hacer merge commits hacia `main`.

## Pendientes que todavía requieren especificación

- El contrato de `vicunav-plugin-core` debe formalizar las firmas públicas antes de
  implementar CORE-03 a CORE-09.
- El contrato de pagos debe cerrar persistencia, transiciones, payloads de hooks,
  expiración e idempotencia antes de PAGOS-02.
- El dominio de restaurante todavía no tiene un spec durable versionado. REST-01 debe
  definir estados del pedido, totales, disponibilidad, permisos, endpoints y pruebas
  antes de crear issues de implementación.
- La paleta final de marca sigue pendiente, pero no bloquea `plugin-core` ni pagos.

## Reglas de mantenimiento

- Una entrada pertenece a un repositorio y se convierte en un issue, una rama, un PR y
  un squash-merge.
- No mezclar cambios de varios repositorios en el mismo commit.
- Eliminar del backlog el detalle de una tarea cuando su issue se cierre; conservar
  solo el resultado relevante en el estado canónico.
- Añadir trabajo nuevo únicamente cuando tenga propietario, dependencia y aceptación
  observables.
- Actualizar este archivo y el estado al finalizar cada fase, no después de cada commit
  interno.
