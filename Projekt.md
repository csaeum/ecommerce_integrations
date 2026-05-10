<!-- Projekt.md | START -->
# Project: ecommerce_integrations — DACH Fork

## Project Overview

This is a fork of `frappe/ecommerce_integrations` focused on DACH (Germany, Austria, Switzerland) support for Shopware 6.7.

The goal is to build a robust ERPNext ↔ Shopware 6 integration that handles:
- 4 Shopware 6.7 storefronts on a single instance: `.de` (EUR/19%), `.at` (EUR/20%), `.ch` (CHF/7.7%), `.international`
- ERPNext as the leading system (Single Source of Truth)
- DACH-specific tax mapping, price lists, and currency handling
- Multi-sales-channel support within a single ERPNext instance

---

## Repository Structure

```
frappe/ecommerce_integrations (upstream, official)
        ↓ fork
csaeum/ecommerce_integrations (GitHub, public mirror)
        ↑ mirror (automatic push from GitLab)
GitLab: frappe-projekte/ecommerce_integrations (primary workspace)
├── main              ← clean, always in sync with frappe upstream
├── julian-upstream   ← TubaApollo/ecommerce_integrations reference code
└── shopware6-dach    ← active development branch (work here only)
```

**Rules:**
- All development happens on `shopware6-dach`
- Never commit directly to `main`
- `julian-upstream` is read-only — use it for reference and cherry-picking only
- GitHub is a mirror only — never push directly to GitHub

---

## Git Remotes

| Name | URL | Purpose |
|---|---|---|
| `origin` | `git@gitlab.localdomain:frappe-projekte/ecommerce_integrations.git` | Primary — push here daily |
| `github` | `https://github.com/csaeum/ecommerce_integrations.git` | Public mirror (auto-synced) |
| `upstream` | `https://github.com/frappe/ecommerce_integrations.git` | Fetch Frappe updates |
| `julian` | `https://github.com/TubaApollo/ecommerce_integrations.git` | Reference code from TubaApollo |

---

## Shop Configuration

| Shop | Domain | Currency | VAT |
|---|---|---|---|
| DE | projektleder.de | EUR | 19% |
| AT | projektleder.at | EUR | 20% |
| CH | projektleder.ch | CHF | 7.7% |
| International | projektleder.com | EUR | varies |

All storefronts run on a **single Shopware 6.7 instance** with separate Sales Channels per country. ERPNext connects to this one instance and handles each Sales Channel independently.

---

## Architecture Decisions

- **ERPNext is the leading system** — product data, pricing, and stock are managed in ERPNext and pushed to Shopware. Never the other way around.
- **One-directional sync (for now)** — orders flow from Shopware into ERPNext as Sales Orders. Everything else is one-directional (ERPNext → Shopware). Two-way sync is a future goal once the core integration is stable.
- **ZUGFeRD/XRechnung** — handled by `alyf-de/eu_einvoice`. Do not build this yourself.
- **Multi-currency** — CHF price lists are managed separately in ERPNext. Do not mix EUR and CHF logic.

---

## Commit Messages

Follow Conventional Commits — required for clean Pull Requests to Frappe:

```
feat(shopware): add DE/AT/CH tax mapping
fix(shopware): correct CHF price list selection
refactor(shopware): extract auth logic into separate module
docs(shopware): update setup instructions
test(shopware): add unit tests for tax mapping
```

- Scope is always `shopware` for Shopware-related changes
- Keep subject line under 72 characters
- Write in English
- Reference issue numbers where applicable: `feat(shopware): add tax mapping (#42)`

---

## Reference Code

### Julian's Fork (TubaApollo)

Branch: `julian/feat/multi-channel-integrations`

Julian has implemented multi-channel support for Shopware. Before building anything new, always check if Julian has already solved it:

```bash
git log julian/feat/multi-channel-integrations --oneline
```

Cherry-pick approach — do not merge wholesale, only take what is relevant and clean:

```bash
git checkout shopware6-dach
git cherry-pick <commit-hash>
```

### Frappe Upstream

Pull updates regularly to keep `main` in sync:

```bash
git checkout main
git pull upstream main
git checkout shopware6-dach
git rebase main
```

---

## What NOT to Build

| Feature | Solution |
|---|---|
| ZUGFeRD / XRechnung | Install `alyf-de/eu_einvoice` — do not build |
| DATEV export | ERPNext Community Apps or a simple Server Script |
| Shopware webhook receiver | TubaApollo webhook plugin |

---

## Key Links

| Description | URL |
|---|---|
| GitLab (primary) | http://gitlab.localdomain/frappe-projekte/ecommerce_integrations |
| GitHub (mirror) | https://github.com/csaeum/ecommerce_integrations |
| Frappe upstream | https://github.com/frappe/ecommerce_integrations |
| Julian's fork | https://github.com/TubaApollo/ecommerce_integrations |
| Julian's branch | https://github.com/TubaApollo/ecommerce_integrations/tree/feat/multi-channel-integrations |
| Shopware Webhook Docs | https://developer.shopware.com/docs/guides/plugins/apps/webhook.html |
| ERPNext Multi-Currency | https://docs.frappe.io/erpnext/multi-currency-accounting |
| ERPNext Price Lists | https://docs.frappe.io/erpnext/price-lists |
<!-- Projekt.md | ENDE -->
