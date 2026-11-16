# WooCommerce Plugin Data Contract

As of November 16, 2026. This document describes the data the plugin needs to exchange
and how it should process and display that data. “Available” means implemented in
the inspected local Mesuro source; deployment and runtime behavior were not tested.
“Required” describes a contract that still needs agreement, not an available API.
The plugin itself has not been implemented yet.

## 1. Display and assign WooCommerce products in Mesuro

**Required, not implemented:** Mesuro should display products from a connected
WooCommerce shop so merchants can assign configurators there. WooCommerce remains
the source of shop product data.

The proposed minimum product record includes:

| Data | Use in Mesuro |
| --- | --- |
| Shop identifier and WooCommerce product ID | Identify a product within the connection |
| Name and optional SKU | Search for and recognize the product |
| Product type and, where applicable, variation ID with parent product ID | Distinguish a parent product from a specific variation |
| Product status | Identify unavailable products |
| Product URL and optional image URL | Inspect and recognize the product in the list |
| Existing configurator assignment, if any | Display the current assignment |

An assignment must contain at least the shop identifier, product/variation ID as
applicable, and Mesuro configurator ID. It must then reach the plugin so the correct
product page loads the correct snapshot.

Open decisions include whether Mesuro fetches products or the plugin sends them,
authentication, pagination/search, updates and deletions, and the authoritative
storage location for assignments. Delivery of assignments changed in Mesuro back
to the plugin is also missing. No synchronization route or concrete field names have
been agreed yet. Customer and order data are not needed for this workflow.

## 2. Retrieve a snapshot

**Available in Mesuro:**
`GET /public/v1/:key/:product/snapshot` returns a delivered snapshot directly as JSON.
The plugin needs the API base URL, public connection key, and assigned Mesuro product
identifier. The shop origin must be allowed in Mesuro, and the request must include
the matching `Origin` header.

- 403 indicates a missing/disallowed origin or missing public connection.
- 404 indicates an invalid identifier or an unavailable snapshot.
- The response uses `Cache-Control: no-store`.
- `X-Mesuro-Version` contains the product version. This header is currently not
  exposed through CORS to browsers on other origins.
- The URL returns the currently delivered version. Public retrieval of a specific
  historical product version is not available yet.

The public key does not provide trusted shop authentication. A snapshot is an input
document for the Mesuro configurator; the plugin does not need to evaluate its rules
or pricing formulas itself. Snapshot format version and product version are distinct.

## 3. Display a snapshot

**Available in Mesuro:** The `mesuro-configurator` Web Component receives the snapshot
URL through its `snapshot` attribute. The plugin also needs the approved embed script
URL. Other attributes are `display` with net/gross, `tax-rate` as a decimal percentage
string, and an optional `accent` hex color.

The embed loads and validates the snapshot and displays measurements, options, price
previews, a summary, and loading/error states. The plugin integrates it on assigned
product pages. It does not need to rebuild the configurator interface.

The display and tax context must follow the agreed WooCommerce behavior. The embed
defaults of gross prices and 20 percent are not sufficient instructions for this
integration. On loading failure, the plugin must display a clear message and prevent
an unvalidated configured purchase.

## 4. Add a configuration to the cart

**Required, not available yet:** A documented embed output interface and a server-side
validation/pricing contract.

| Required data | Plugin processing |
| --- | --- |
| Assigned shop product and configurator ID | Check against the stored assignment |
| Product version and original customer inputs | Submit for server-side validation |
| Completeness/error status | Enable the purchase action or explain problems in the interface |
| Server-confirmed configuration and price reference | Use for the cart item |
| Amount, currency, tax basis, and item quantity | Pass to WooCommerce according to the agreed contract |
| Display data for measurements and selected options | Present clearly in the cart, checkout, and order |

The contract must define measurement units, selection IDs, quantities, and display
labels unambiguously. Original customer inputs must remain distinct from Mesuro's
normalized result. Option quantities and cart item quantity must be handled separately.

Browser data is untrusted, including displayed prices and a “complete” status.
WordPress must obtain server-side confirmation of the configuration and check its
shop/product binding before accepting a price. Whether confirmation uses a signed
offer or another authenticated server response remains to be decided. A public key
or WordPress nonce does not replace this price validation.

The plugin must preserve different configurations of the same product as separate
cart items, restore their data after session restoration, and persist confirmed
details on the order. Rules for price validity, quantity changes, checkout
revalidation, and API failures are still missing. Never use an unvalidated price
when confirmation is missing or invalid.

## Implementation status and open contracts

| Area | Mesuro | WooCommerce plugin |
| --- | --- | --- |
| Load a snapshot | Available | Missing |
| Display the configurator | Embed available | Integration missing |
| Display WooCommerce products in Mesuro | Shop connection missing | Product availability interface missing |
| Edit assignments in Mesuro and deliver them to the shop | Contract and implementation missing | Receipt/storage missing |
| Transfer configuration from the embed | Output contract missing | Receiver missing |
| Confirm configuration and price server-side | Integration contract missing | Validation/acceptance missing |
| Display configuration in cart and order | Display data still needs agreement | Missing |

Before implementing purchasing, agree on authentication, data formats,
currency/tax/rounding, version and price binding, and error handling. Before
implementing product assignment, agree on product availability and assignment
delivery back to the shop. Document concrete routes and payloads as available only
once they are implemented.

## Licensing and scope

This overview describes plugin interfaces in original wording. Proprietary backend
code, pricing algorithms, internal documentation, and bundles are not copied into
the plugin. Copying or distributing those artifacts requires appropriate license
permission. This also applies to bundling the embed; external loading and distribution
within the plugin must be assessed separately.

Internal backend architecture, database structure, and administration routes are
outside the required plugin contract. The backend implementation remains independent.
