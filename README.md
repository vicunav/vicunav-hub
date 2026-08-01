# vicunav-hub

`vicunav-hub` documents the architecture and decisions of the Vicunav ecosystem: a
modular set of WordPress themes and plugins for building vertical solutions without
coupling presentation, business logic, or payments.

Each package lives in its own repository, with independent history, version, and README.
Packages communicate through public contracts and hooks; no plugin reads another's
database directly.

## Ecosystem architecture

```mermaid
flowchart TB
    subgraph foundation["1. Foundation"]
        theme["vicunav-theme-core<br/>Shared presentation"]
        plugin["vicunav-plugin-core<br/>Base capabilities"]
    end

    subgraph payments["2. Payment engine"]
        payment["vicunav-pagos<br/>Independent payments"]
    end

    subgraph verticals["3. Verticals"]
        hotel["vicunav-hotel<br/>Bookings"]
        restaurant["vicunav-restaurante<br/>Orders"]
    end

    subgraph demos["4. Demos"]
        demo_hotel["vicunav-demo-hotel"]
        demo_restaurant["vicunav-demo-restaurante"]
    end

    plugin --> payment
    theme --> hotel
    theme --> restaurant
    plugin --> hotel
    plugin --> restaurant
    payment -->|public hooks| hotel
    payment -->|public hooks| restaurant
    theme --> demo_hotel
    theme --> demo_restaurant
    hotel --> demo_hotel
    restaurant --> demo_restaurant
```

The layers separate concrete responsibilities:

1. **Foundation:** `vicunav-theme-core` provides presentation patterns, tokens, and
   templates; `vicunav-plugin-core` concentrates shared base capabilities. Business
   logic does not live in the theme.
2. **Payment engine:** `vicunav-pagos` processes payments without knowing about
   bookings or orders. Verticals declare it through `Requires Plugins` and react to its
   public hooks. A project without transactions can omit it.
3. **Verticals:** `vicunav-hotel` and `vicunav-restaurante` encapsulate bookings and
   orders respectively, without reading another plugin's internal data.
4. **Demos:** `vicunav-demo-hotel` and `vicunav-demo-restaurante` integrate the layers
   into public reference sites.

The standards, template, and documentation repositories support the ecosystem's
development, but are not part of its execution layers.

## Repositories

| Repository | Purpose | Status |
| --- | --- | --- |
| [`vicunav-standards`](https://github.com/vicunav/vicunav-standards) | Shared technical standards for the ecosystem. | Available |
| [`vicunav-repo-template`](https://github.com/vicunav/vicunav-repo-template) | Base template to bootstrap new repositories. | Available |
| [`vicunav-hub`](https://github.com/vicunav/vicunav-hub) | Architecture and decision documentation for the ecosystem. | Available |
| [`vicunav-theme-core`](https://github.com/vicunav/vicunav-theme-core) | Shared presentation patterns, tokens, and templates. | In progress |
| `vicunav-plugin-core` | Base capabilities shared by the plugins. | Pending |
| `vicunav-pagos` | Payment engine independent of the verticals. | Pending |
| `vicunav-hotel` | Hotel vertical logic and its bookings. | Pending |
| `vicunav-restaurante` | Restaurant vertical logic and its orders. | Pending |
| `vicunav-demo-hotel` | Public demo of the hotel vertical. | Pending |
| `vicunav-demo-restaurante` | Public demo of the restaurant vertical. | Pending |

## Architecture decisions

- [ADR 0001: Separation between theme and plugins](docs/adr/0001-separacion-theme-plugins.md)
- [ADR 0002: Payments as an independent engine](docs/adr/0002-pagos-motor-independiente.md)
- [ADR 0003: Boundary contracts and events](docs/adr/0003-contratos-y-eventos.md)
- [ADR 0004: Repository structure](docs/adr/0004-estructura-de-repos.md)
- [ADR 0005: Genuine ACF for fields only](docs/adr/0005-acf-genuino-solo-campos.md)
- [ADR 0006: Restaurant first](docs/adr/0006-restaurante-primero.md)

## License

The documentation in this repository is distributed under
[Creative Commons Attribution 4.0 International](LICENSE).
