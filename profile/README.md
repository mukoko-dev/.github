# Mukoko

**Africa's super app. Your Identity. Your Sovereignty. Your Honey.**

_Ndiri nekuti tiri — I am because we are._

[mukoko.com](https://www.mukoko.com) ·
[The Mukoko Manifesto](./canonical/MUKOKO_MANIFESTO.md) ·
[The Nyuchi Architecture](https://github.com/nyuchi/.github/blob/main/profile/canonical/NYUCHI_ARCHITECTURE.md) ·
[The Bundu Order](https://github.com/bundu-labs/.github/blob/main/profile/canonical/BUNDU_ORDER.md)

---

## What Mukoko is

Mukoko — Shona for _beehive_ — is the consumer surface of the Bundu
ecosystem: seventeen mini-apps, four substrate components and one
Mukoko Account. It is the first tenant of the infrastructure Nyuchi
Africa operates, and it is governed by the Bundu Foundation.

Every mini-app works in three modes at once: **Musha** (home, the
experience you use), **Basa** (work, the service other apps use) and
**Nhaka** (heritage, what it gives the open commons).

The [Mukoko Manifesto v5.0.0](./canonical/MUKOKO_MANIFESTO.md) is the
platform's philosophy and its Seven Covenants. The technical detail,
and the status of every component, is in
[the Nyuchi Architecture v5.0.0](https://github.com/nyuchi/.github/blob/main/profile/canonical/NYUCHI_ARCHITECTURE.md),
which wins wherever two documents disagree.

## What you can use today

| Product                    | Where                                              | Status |
| -------------------------- | -------------------------------------------------- | ------ |
| Mukoko Account (Mukoko ID) | [accounts.mukoko.com](https://accounts.mukoko.com) | Live   |
| Mukoko Events              | [events.mukoko.com](https://events.mukoko.com)     | Live   |
| Mukoko News                | [news.mukoko.com](https://news.mukoko.com)         | Live   |
| Mukoko Weather             | [weather.mukoko.com](https://weather.mukoko.com)   | Live   |
| Mukoko Kweli (Places)      | [kweli.mukoko.com](https://kweli.mukoko.com)       | Live   |

The status of all
seventeen is in
[the Architecture, §10](https://github.com/nyuchi/.github/blob/main/profile/canonical/NYUCHI_ARCHITECTURE.md#10-the-seventeen-mini-apps).
Barstool is folded into Mukoko Kweli.

## How Mukoko apps reach data

- **The Nyuchi API** (`api.nyuchi.com/v1`,
  [`nyuchi/api-gateway`](https://github.com/nyuchi/api-gateway)) is
  the internal API for every app in the ecosystem and the only thing
  that connects to a database. Each first-party Mukoko app calls it
  directly with its own client ID and secret.
- **The Mukoko API** (`api.mukoko.com`,
  [`mukoko-api`](https://github.com/mukoko-dev/mukoko-api), Cloudflare
  Workers, no database access) is the public consumer API. Each app's
  public API lives there by namespace — `api.mukoko.com/v1/weather`
  rather than `weather.mukoko.com/api`. **Building.**
- **Sign-in** is WorkOS AuthKit at `accounts.mukoko.com`: one login for
  the whole ecosystem.

## Repositories

This organisation holds the Mukoko repositories: the Mukoko docs
([`mukoko`](https://github.com/mukoko-dev/mukoko)), `mukoko-api`,
`mukoko-auth`, `kweli` and `kweli-mcp`, Mukoko Weather
(`mukoko-weather`, `mukoko-weather-mobile`), Mukoko Events
(`mukoko-events`, formerly `nhimbe`, `mukoko-events-admin`),
`mukoko-lingo`, `mukoko-circles`, `mukoko-home`, Mukoko News
(`mukoko-news`, `mukoko-news-gateway`, `mukoko-ingestion-pipeline`),
`mukoko-events-mcp`, `bushtrade`, `nyuchi-identity`, and the super apps
`super-app-web` and `super-app-mobile`.

Mukoko's shared UI comes from Mzizi, the Bundu Foundation's design
system, in [`mzizi-dev`](https://github.com/mzizi-dev).

## Contributing

Engineering rules, the lint gate and the reusable CI workflows are
shared across the estate from
[`nyuchi/.github`](https://github.com/nyuchi/.github): read its
[`CONTRIBUTING.md`](https://github.com/nyuchi/.github/blob/main/CONTRIBUTING.md)
and [`AGENTS.md`](https://github.com/nyuchi/.github/blob/main/AGENTS.md)
before opening a PR.

_Built by Nyuchi Africa · Governed by the Bundu Foundation · Ubuntu in
code_
