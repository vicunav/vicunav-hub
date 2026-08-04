# Estado canónico del ecosistema Vicunav

## Estado del documento

- Responsable operativo: Codex.
- Autoridad de producto y acciones irreversibles: usuario (Mario).
- Auditoría histórica de Claude: **completa**, 2026-08-04, mediante clonado directo
  de los repos públicos de GitHub (no solo lectura de reportes previos).
- Fuente para ejecución: issues y pull requests enlazados desde el
  [backlog](backlog-ecosistema.md).

Este documento describe hechos verificados y enlaza normas o ADRs; no los duplica.
Toda afirmación está marcada como **verificado** (comprobado directamente en el
código/Git/GitHub) o **inferencia** (deducido, no confirmado).

## Propósito y arquitectura vigente — verificado

Vicunav es un ecosistema modular de WordPress para lanzar sistemas de reserva/pedido a
hoteles y restaurantes locales en Venezuela, con doble propósito: negocio real y
portafolio técnico público. Arquitectura en capas:

1. `vicunav-theme-core` — presentación compartida (theme de bloques FSE). No contiene
   lógica de negocio (ADR 0001).
2. `vicunav-plugin-core` — capacidades base (registro de CPT por herencia, seguridad,
   settings, REST) que consumen los demás plugins.
3. `vicunav-pagos` — motor de pagos independiente y opcional, con `referencia_externa`
   polimórfica; los verticales lo declaran vía `Requires Plugins` (ADR 0002).
4. `vicunav-restaurante` / `vicunav-hotel` — lógica vertical, sin leerse datos entre sí
   ni con `pagos` directamente; reaccionan a hooks públicos (ADR 0003). Restaurante
   tiene prioridad de construcción sobre hotel (ADR 0006).
5. `vicunav-demo-hotel` / `vicunav-demo-restaurante` — composición de las capas
   anteriores más contenido de ejemplo, sin lógica propia.

Convención de nombres (ADR 0004): prefijo de repo `vicunav-` (marca), prefijo de código
`vicu_` (límite de 20 caracteres en post types de WordPress).

## Contratos y hooks — verificado

Tres contratos frontera, documentados antes de escribir código (ADR 0003):

- **theme-core:** tokens de `theme.json` con slugs fijos, 2 variantes de header/footer,
  catálogo de patrones (`hero`, `cta`, `testimonials-grid`, `faq-accordion`,
  `contact-info`), mecanismo `register_block_template()` para que verticales futuros
  registren su propia plantilla sin que el theme los conozca.
- **plugin-core:** clase abstracta `Vicu\Core\PostType` (registro de CPT por herencia,
  no composición — decisión deliberada para reducir superficie de improvisación de la
  IA), CPTs compartidos `vicu_faq`/`vicu_testimonial`, menú admin único "Vicunav" con
  `Settings::register_tab()`, helpers de seguridad, REST bajo namespace `vicu/v1`.
  **Claves de `Settings::get()` ya fijadas y en uso real por theme-core:** `phone`,
  `address`, `business_hours` (inglés snake_case, no español — ver contrato completo).
- **pagos:** CPT `vicu_payment_req`, máquina de estados
  (`pendiente → comprobante_subido → confirmado/rechazado`, expiración configurable
  por vertical), proveedor `manual` en v1, Mercantil en v1.1 futuro. Hooks:
  `vicu_pagos_creado`, `vicu_pagos_confirmado`, `vicu_pagos_rechazado`,
  `vicu_pagos_expirado` (prefijo corto `vicu_`, no `vicunav_` — corregido una vez ya en
  esta misma sesión de trabajo por una inconsistencia real).

Contratos completos: `vicunav-theme-core-contract.md`, `vicunav-plugin-core-contract.md`,
`vicunav-pagos-contract.md` (documentos de planeación, no viven aún en ningún repo —
ver "Deuda" abajo).

## ADRs vigentes — verificado

Los 6 en `docs/adr/` de este mismo repo:

- ADR 0001 — separación theme/plugins
- ADR 0002 — pagos como motor independiente
- ADR 0003 — contratos frontera y eventos
- ADR 0004 — estructura de repos, dos prefijos
- ADR 0005 — ACF genuino, nunca SCF, solo para campos editoriales
- ADR 0006 — restaurante antes que hotel

## Decisiones descartadas — verificado

- WooCommerce para restaurante: descartado, no resuelve el flujo de pago manual real
  del mercado venezolano (sin Stripe/PayPal).
- Stripe/PayPal: no disponibles para comercios en Venezuela.
- Secure Custom Fields (SCF): descartado por ambigüedad legal del fork (ADR 0005).
- Orquestación Claude Code + Codex CLI como motor dual: explorada y **abandonada por
  decisión explícita del usuario** — se volvió al flujo directo Codex Desktop +
  revisión de Claude por chat. No queda infraestructura de esa exploración en ningún
  repo (el skill de Claude Code nunca se llegó a usar en producción).
- Registro de CPT por composición (función configurable) en vez de herencia: descartado
  a favor de la clase abstracta, para forzar un contrato de métodos explícito.

## Entornos de desarrollo — verificado

- LocalWP: sitio `vicunav-demo-restaurante.local`, PHP `8.2.29`, WordPress `7.0.2`,
  nginx. `vicunav-theme-core` enlazado por symlink desde
  `~/Documents/Codex/vicunav/vicunav-theme-core` hacia
  `wp-content/themes/vicunav-theme-core`. Este sitio es el entorno de verificación
  compartido para theme-core, y será el mismo usado para plugin-core/pagos/restaurante
  cuando existan (es también el futuro `vicunav-demo-restaurante` real, Fase 8).
- Estructura local de repos: `~/Documents/Codex/vicunav/{nombre-repo}/`.
- `~/.codex/AGENTS.md` (global, fuera de cualquier repo) contiene: estándar WPCS,
  conventional commits, regla de un-issue-un-commit, regla de completar el ciclo Git
  hasta squash-merge sin quedar en borrador salvo bloqueo real, y modo de ejecución por
  lote (trabajar varios issues seguidos sin pausar, autoverificando contra criterios de
  aceptación, deteniéndose solo ante bloqueo real).

## Estado real de cada repositorio — verificado por clonado directo

| Repositorio | Existe en GitHub | Estado | Notas |
| --- | --- | --- | --- |
| `vicunav-standards` | Sí, público | Completo, 6 docs | security, naming, git, accessibility, compatibility, testing |
| `vicunav-repo-template` | Sí, público | Completo, marcado como Template | Incluye submodule, AGENTS.md, plantilla de issue, CI (WPCS vía PHPCS) |
| `vicunav-hub` | Sí, público | Completo | README con diagrama Mermaid + 6 ADRs |
| `vicunav-theme-core` | Sí, público | **Completo con 2 defectos pendientes sin corregir** | Ver "Riesgos y deuda" |
| `vicunav-plugin-core` | **No existe** | No iniciado | Contrato cerrado, issues redactados, cero código |
| `vicunav-pagos` | No existe | No iniciado | Depende de plugin-core |
| `vicunav-hotel` | No existe | No iniciado, diferido a propósito (ADR 0006) | Sin spec interno todavía |
| `vicunav-restaurante` | No existe | No iniciado | Depende de plugin-core + pagos |
| `vicunav-demo-hotel` | No existe | No iniciado | — |
| `vicunav-demo-restaurante` | No existe como repo | El **sitio LocalWP** ya existe y está en uso | Ver "Entornos" |
| `vicunav-gutenberg` | Sí, público | **Fuera del alcance de esta conversación — ver más abajo** | — |
| `.github` | Sí, público | Perfil de organización, solo carpeta `profile/` | Sin clasificar previamente en ningún README |
| `mariovicunadev` / `vicunav-github-profile` | No confirmado (límite de tasa de API sin autenticar durante la auditoría) | Sin clasificar | Inferencia: probablemente repo de perfil personal de GitHub del usuario, no parte del ecosistema técnico |

## `vicunav-gutenberg` — hallazgo de esta auditoría, no parte de esta conversación

**Esto es lo más importante que encontró la auditoría.** `vicunav-gutenberg` es un
repositorio real, público, maduro (historial hasta al menos PR #97), que migra el sitio
**real y actual** de vicunav.com de Elementor a Gutenberg FSE — el mismo proyecto que el
usuario mencionó al inicio de esta conversación, pero que se desarrolló **completamente
en paralelo, fuera de esta conversación, sin que Claude (aquí) tuviera ninguna
visibilidad de su avance.**

Verificado directamente en el repo:
- Tiene su propio `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `SECURITY.md` — gobierno
  independiente del de este ecosistema.
- Historial de commits menciona una integración con "qwen" (`tooling: sandbox qwen
  direct writes`) — **posible indicio de un tercer agente de IA operando sobre este
  repo**, no confirmado con certeza desde esta auditoría.
- Contiene un commit `docs: translate README to English, align with current state` con
  el **mismo mensaje exacto** que el commit más reciente de `vicunav-theme-core` — señal
  fuerte de que algún proceso tocó ambos repos de forma coordinada, sin pasar por esta
  conversación.
- Release candidate 0.2.0, con QA de WCAG 2.2 AA, multilenguaje (Polylang), formularios
  con Contact Form 7 y Turnstile ya implementados.

**Claude no tiene contexto histórico de este repo y no debe tratarlo como parte del
ecosistema de 10 repos diseñado en esta conversación** hasta que el usuario aclare su
relación real con Vicunav (ver "Preguntas abiertas").

## Riesgos, deuda y preguntas abiertas

### Riesgo — regresión de idioma sin autorizar (verificado, sin resolver)

El commit más reciente de `vicunav-theme-core` (`fb8d3ff`) tradujo el `README.md` al
inglés. Esto contradice la convención establecida explícitamente en esta conversación
("toda documentación en español") y **nunca fue solicitado por Claude ni reportado por
el usuario**. El mismo patrón apareció en `vicunav-gutenberg`. No se revirtió. Requiere
decisión del usuario, no una corrección automática — ver "Preguntas abiertas".

### Deuda — 2 correcciones pedidas y nunca aplicadas (verificado)

Sobre `vicunav-theme-core`, solicitadas por Claude, confirmadas como **no aplicadas**
al clonar el repo real:
1. `vicunav-secondary` sigue siendo idéntico a `vicunav-neutral-700` (`#475569` ambos)
   en `theme.json`.
2. `docs/plantillas-verticales.md` sigue usando el CPT inventado `vicu_restaurant` en
   vez de `vicu_menu_item` (el real, según el spec de restaurante).

Ambas están en el backlog como la siguiente acción recomendada.

### Deuda — contratos de planeación no versionados en ningún repo

Los tres documentos de contrato (`theme-core`, `plugin-core`, `pagos`) y los specs de
verticales existen solo como archivos de planeación fuera de Git. Deberían vivir
formalmente en sus repos correspondientes (`plugin-core`/`pagos` cuando se creen;
`theme-core` ya existe y no los tiene todavía).

### Deuda — paleta de color sigue siendo placeholder

Documentado como tal en el propio README de `theme-core`. Hay una sesión de diseño en
curso en Claude Design (fuera de este repo) para definir la paleta real.

### Preguntas abiertas — requieren al usuario, no se pueden verificar

1. **¿Qué relación tiene `vicunav-gutenberg` con este ecosistema?** ¿Es el sitio
   principal de vicunav.com, independiente por diseño, o debería integrarse/consumir
   `vicunav-theme-core` y los demás paquetes en algún momento? Sin esta respuesta, el
   hub no puede clasificarlo en su README.
2. **¿La traducción al inglés de README de `theme-core` (y `vicunav-gutenberg`) es una
   decisión nueva del usuario que no llegó a esta conversación, o algo que Codex hizo
   por su cuenta y debe revertirse?** Si es una decisión real, hay que actualizar la
   convención en `vicunav-standards/docs/naming.md` y `AGENTS.md`, no dejarla implícita.
3. **¿Existe efectivamente un agente adicional ("qwen") operando sobre
   `vicunav-gutenberg` o algún otro repo?** Si sí, vale la pena que quede documentado
   quién gobierna qué repo, para evitar que dos agentes de IA hagan cambios
   contradictorios sin coordinación.

## Siguiente acción recomendada

Ver backlog — la entrada de mayor prioridad y sin ningún bloqueo es corregir las 2
deudas pendientes de `vicunav-theme-core` (color duplicado, CPT inventado en docs), no
iniciar `vicunav-plugin-core` todavía, porque cerrar un repo que se dio por completo con
defectos conocidos sin corregir no es una buena base para construir encima.
