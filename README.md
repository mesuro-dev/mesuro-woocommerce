# Mesuro for WooCommerce

Open-source integration connecting WooCommerce products with [Mesuro](https://mesuro.at) product configurators.

## Status

Early development. Not ready for production use.

## Planned features

- Show WooCommerce products in Mesuro and link them to configurators there.
- Receive those assignments and display the matching configurator on each product page.
- Let customers enter measurements and select product options.
- Validate configurations and calculate prices through the Mesuro API.
- Add configured products to the cart with a server-validated price.
- Preserve configuration details in the cart and order.

WooCommerce manages products, checkout, and orders. Mesuro manages configurations, rules, and pricing.

## Requirements

- WordPress with WooCommerce
- Access to a Mesuro instance

Supported versions and installation instructions will be documented as development progresses.

## Development

See the [technical integration overview](docs/integration.md) for verified backend interfaces, missing capabilities, and licensing boundaries. Contributors and coding agents should follow [AGENTS.md](AGENTS.md).

The plugin is developed independently from the Mesuro backend. Local development and testing will use a WordPress/WooCommerce environment connected to a local Mesuro instance.

## Contributing

Issues and pull requests are welcome. Please discuss larger changes in an issue first. Do not include credentials or customer data in reports.

## License

GPL-2.0-or-later. See [LICENSE](LICENSE).

The license applies to this repository. The Mesuro SaaS is a separate service.
