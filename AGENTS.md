# Instructions for Mesuro for WooCommerce

## Scope

Read `README.md` and `docs/integration.md` first. The plugin has four responsibilities:

1. Make WooCommerce products available in Mesuro for display and configurator assignment.
2. Receive assignments from Mesuro and retrieve the matching snapshot.
3. Display the Mesuro configurator on the assigned product page.
4. Add a server-validated configuration and price to the cart and order, displaying
   the configuration details clearly.

Document required inputs, outputs, processing, display, and error handling. Explicitly
separate implemented interfaces from proposals and missing contracts. Cover both
product availability in Mesuro and assignment delivery back to the shop. Do not invent
transfer directions, authoritative storage locations, or API routes.

## Integration boundaries

WooCommerce manages shop products, carts, checkout, and orders. Mesuro manages
configurators, rules, and price calculation. The plugin integrates the embed; it does
not implement its own pricing engine, rules engine, or Mesuro administration interface.

Browser prices, completeness indicators, and other browser data are untrusted.
Do not enable purchasing without server-side confirmation of the configuration, price,
and product/shop binding. Public keys and WordPress nonces do not replace server
authentication. Document missing contracts as blockers and continue independent work.

Keep product versions separate from snapshot format versions, and original customer
inputs separate from normalized results. Explicitly agree on currency, measurement
units, quantities, tax basis, price validity, and rounding. Preserve configuration
details in the cart, checkout, and order.

## Language and coding rules

- Use English for source code, identifiers, comments, docblocks, documentation,
  developer-facing messages, test descriptions, and commit/PR descriptions.
- Write user-facing source strings in English and make them translatable through
  WordPress internationalization APIs. Translations may use their target language.
- Follow WordPress coding standards for PHP and WordPress/WooCommerce conventions.
  Follow the project's configured standards for JavaScript, CSS, and tooling once
  those are established. Do not claim tooling exists before it is configured.
- Use descriptive names, small focused functions, and explicit dependencies. Keep
  integration/network access separate from data transformation and display logic.
- Prefix or namespace plugin-owned symbols to avoid collisions. Use supported
  WordPress/WooCommerce APIs rather than depending on their internal storage layout.
- Comment non-obvious decisions and constraints. Avoid comments that merely repeat
  the code. Never include secrets or real customer data in examples or comments.
- Keep changes focused on the current task and update affected documentation in the
  same change. Avoid unrelated cleanup and speculative infrastructure.

## Test-driven development (TDD)

Use TDD for new executable behavior and bug fixes:

1. Write a focused test describing the required observable behavior. For a bug,
   first reproduce it with a regression test.
2. Run the test and confirm it fails for the expected behavioral reason, rather
   than a broken test setup.
3. Implement the smallest change that makes the test pass.
4. Refactor while keeping the tests green, then run the relevant checks.

Test public behavior and meaningful integration boundaries rather than mirroring
implementation details. Use synthetic fixtures and controlled substitutes for
external services in automated tests. Do not require private backend source code
or production credentials to run the plugin test suite.

For integration behavior that needs WordPress/WooCommerce, use an appropriate test
environment. If a required environment is unavailable, report which checks could
not run; do not claim the behavior is verified. Documentation-only changes need
source, link, and diff checks rather than executable tests.

## YAGNI

Implement only what a current requirement needs. Choose the simplest solution that
satisfies it. Do not add hypothetical integrations, generic synchronization frameworks,
configuration switches, extension points, or compatibility layers without a concrete
use case. Do not create abstractions solely for possible future reuse.

Before adding complexity, identify the current requirement, explain why a simpler
implementation is insufficient, and state the concrete problem it solves today.
YAGNI does not justify skipping validation, security, data integrity, tests, or
established architectural boundaries.

## Licensing and public documentation

The backend is proprietary. Without documented license permission, do not copy or
port source code, algorithms, tests, fixtures, complete schemas, bundles,
documentation text, or assets. Do not introduce dependencies on private workspace
packages. Implement plugin code independently against agreed interfaces and use
synthetic test data. Assess external embed loading and bundle redistribution
separately; public availability does not grant redistribution rights.

Limit public documentation to the integration contract the plugin needs. Do not
include internal backend file paths, commit hashes, database details, or internal
administration workflows. When authorized to inspect the backend, read its repository
instructions and license first; do not modify it as part of a plugin task. Do not
claim an interface is verified without inspecting it.

Never commit secrets, session cookies, or real customer/order data. Check the licenses
and provenance of third-party dependencies. The README declares GPL-2.0-or-later;
the corresponding LICENSE file is currently missing.

## Implementation and verification

There is currently no plugin code, build system, or test system. Document supported
PHP/WordPress/WooCommerce versions and verification commands after establishing them.
Apply the TDD workflow when introducing the first executable behavior and its test setup.

Follow WordPress/WooCommerce conventions: check capabilities and nonces for
administrative changes, validate inputs, escape outputs, protect server requests
against SSRF, and keep secrets server-side. Expose product data only to authenticated
connections authorized for the shop.

Relevant integration checks include product lists/pagination, variants, assignment
changes, removed products, manipulated prices, incorrect product/shop binding,
incomplete inputs, invalid price confirmations, version changes, API failures,
quantity changes, separate configurations, session restoration, and order persistence.
Check tax and rounding behavior against the agreed contract. Claim compatibility
with Blocks, classic checkout, or HPOS only after implementation and verification.

Report what changed, which checks actually ran, and which blockers remain.
