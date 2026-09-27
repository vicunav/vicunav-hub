# vicunav-hub

`vicunav-hub` holds the architecture decisions, current state and backlog of the Vicunav
projects. It contains documentation only.

Every project is a self-contained repository with its own block theme and its own
plugin. Presentation lives in the theme and business logic in the plugin; no code is
shared between projects as a dependency. New projects get a new theme and plugin,
written from scratch.

## Repositories

| Repository | Role | Visibility |
| --- | --- | --- |
| [`vicunav-restaurante`](https://github.com/vicunav/vicunav-restaurante) | Reference restaurant project (the Bonasera trattoria): single plugin, block theme, content, assets, local installer and QA. | Public |
| `vicunav-bhoga-yoga` | Private client project: migrate the live `bhoga.yoga` site to a local Gutenberg block theme. Tailwind CSS stack, owner-executed (see ADR 0006 and 0007). | Private |
| [`vicunav-standards`](https://github.com/vicunav/vicunav-standards) | Shared technical standards, included as the `docs/standards` submodule in every repository. | Public |
| [`vicunav-repo-template`](https://github.com/vicunav/vicunav-repo-template) | Template to bootstrap new repositories. | Public |
| [`vicunav-hub`](https://github.com/vicunav/vicunav-hub) | Decisions, state and backlog (this repository). | Public |
| [`vicunav-gutenberg`](https://github.com/vicunav/vicunav-gutenberg) | Independent project that migrates the `vicunav.com` site from Elementor to Gutenberg. | Public |
| [`.github`](https://github.com/vicunav/.github) | Public profile of the GitHub organization (the `.github` repository). | Public |

## Architecture decisions

- [ADR 0001: Theme and plugin separation inside each project](docs/adr/0001-separacion-theme-plugins.md)
- [ADR 0002: Repository structure and prefixes](docs/adr/0002-estructura-de-repos.md)
- [ADR 0003: Genuine ACF for editorial fields only](docs/adr/0003-acf-genuino-solo-campos.md)
- [ADR 0004: Restaurant commerce without WooCommerce](docs/adr/0004-restaurante-sin-woocommerce.md)
- [ADR 0005: Blocking 1:1 visual fidelity for Gutenberg migrations](docs/adr/0005-fidelidad-visual-bloqueante.md)
- [ADR 0006: Styling stack per project (native CSS or Tailwind CSS)](docs/adr/0006-stack-de-estilos-por-proyecto.md)
- [ADR 0007: Execution model per project (agent-led or owner-led)](docs/adr/0007-modelo-de-ejecucion-por-proyecto.md)

## State, backlog and governance

- [Current state](docs/estado.md)
- [Backlog](docs/backlog.md)
- [How a decision is made and propagated](docs/gobernanza.md)

## License

The documentation in this repository is distributed under
[Creative Commons Attribution 4.0 International](LICENSE).
