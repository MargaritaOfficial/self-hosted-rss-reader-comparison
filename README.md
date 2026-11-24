# Self-Hosted RSS Reader Comparison

> **Decision:** Use this comparison when update scheduling, filtering, user separation, client APIs, retention, and operating weight drive the RSS choice; skip it if importing OPML is your only requirement.
>
> **For:** news-heavy readers, teams, and minimalists replacing a hosted feed reader  ·  **Not for:** users who want a zero-operations hosted service or a popularity ranking

This is a non-commercial, source-traceable decision guide. It normalizes upstream declarations; it does not claim that repository reachability proves functionality, security, maintenance, or fitness.

**Start here:** [Download the feed operations ledger](assets/rss-reader-compatibility.csv) — twelve projects, located upstream evidence, and explicit unknowns.

## Choose by workload

- Small, opinionated, multi-user server: Miniflux.
- Broad web-reader compatibility and extensions: FreshRSS.
- Rule-heavy power-user workflow: Tiny Tiny RSS, after confirming the active fork and plugin set.
- Single-binary personal reading: Yarr or Fusion.
- Hosted-service codebase: Feedbin is feature-rich but upstream does not recommend casual production self-hosting.

## Match reader, updater and client

OPML transfers feed subscriptions, not necessarily stars, filters, read state or saved bodies. Stringer is a single-user queue in the compact-reader layer.

## Reader compatibility map

- **Compact readers** — Miniflux, Stringer, Yarr, Fusion, and RSS trade ecosystem breadth for a smaller operating surface.
- **General web readers** — FreshRSS, CommaFeed, selfoss, and Leed cover familiar multi-feed reading with different databases and extension models.
- **Power-user filtering** — Tiny Tiny RSS emphasizes filters and plugins; verify fork provenance and current mobile/API compatibility.
- **Service-scale applications** — NewsBlur and Feedbin provide rich product features but bring multi-service operations; Feedbin explicitly warns about production self-hosting burden.

## Feed operations ledger

| Project | Role | Reader and user model | Updater and runtime | Filters and client integration |
|---|---|---|---|---|
| Miniflux | minimal multi-user reader | Miniflux is a minimalist and opinionated feed reader. It's simple, fast, lightweight and super easy to install. | - Updates feeds in the background using an internal scheduler or a traditional cron job. | - Compatibility with existing mobile applications using the Fever or Google Reader API. |
| FreshRSS | general-purpose reader | It is a multi-user application with an anonymous reading mode. It supports custom tags. There is an API for (mobile) clients, and a Command-Line Interface. | PostgreSQL 10+ or SQLite or MariaDB 10.6+ or MySQL 8.0+ | Google Reader API (best), and Fever API (limited features, less efficient, less safe). |
| Tiny Tiny RSS | power-user reader | Tiny Tiny RSS (tt-rss) ====================== | What's on the main branch (or latest and the most recent sha- tag for the Docker images) is intended to be stable | Unknown in the frozen upstream README |
| CommaFeed | general-purpose reader | Google Reader inspired self-hosted RSS reader, based on Quarkus and React/TypeScript. | Unknown in the frozen upstream README | - Can automatically mark articles as read based on user-defined rules |
| NewsBlur | service-scale reader | A personal news reader bringing people together to talk about the world. | - PostgreSQL for relational data (feeds, subscriptions, accounts) | Unknown in the frozen upstream README |
| Stringer | single-user reader | Unknown in the frozen upstream README | Stringer is a Ruby app based on Rails, PostgreSQL, Hotwire (Turbo and Stimulus) and GoodJob. | Unknown in the frozen upstream README |
| selfoss | extensible personal reader | selfoss is a multipurpose RSS reader and feed aggregation web application. It allows you to easily follow updates from different web sites, social networks and … | 5. Create cronjob or systemd timer for updating feeds and point it to https://yourselfossurl.com/update via wget or curl. You can also execute the cliupdate.php… | Selfoss supports importing OPML files. Find the OPML export in the old application, it is usually located somewhere in settings. Then visit the page https://you… |
| Yarr | minimal reader | yarr (yet another rss reader) is a web-based feed aggregator. | Single binary, no dependencies | Unknown in the frozen upstream README |
| Fusion | compact reader | Unknown in the frozen upstream README | - Self-hosting friendly: single binary or Docker deployment | - Fever API compatibility for third-party clients (Reeder, Unread, FeedMe, etc.) |
| RSS | opinionated personal reader | - No user system or user management system. | You'd also need a process to run the laravel queue system in addition to a cron job to run the schedule. | Unknown in the frozen upstream README |
| Feedbin | hosted-service codebase | Feedbin is a simple, fast and nice looking RSS reader. | The main Feedbin project is a Ruby on Rails application. In addition to the main project there are several other services that provide additional functionality.… | Feedbin is a web based RSS reader. It provides a user interface for reading and managing feeds as well as a REST-like API for clients to connect to. |
| Leed | lightweight web reader | Unknown in the frozen upstream README | All tasks are performed in the background thanks to a scheduled task (cron), so the user does not experience delays due to the recovery and processing of each o… | Unknown in the frozen upstream README |

Values are short excerpts from the frozen upstream README, not feature-test scores. The CSV retains the located evidence for each cell. “Unknown” means the retrieved document did not establish that axis; it is not a claim that the feature is absent.

## Assemble the minimal stack

Export OPML, starred items, tags, and filter rules separately because OPML carries only subscription structure. Run old and new update engines in parallel for a week, compare unread counts and duplicate handling, then test mobile API clients before making the old database read-only.

## Adoption notes by project

### Miniflux

- **Choose it for:** Small teams or power users wanting a narrow surface.
- **License at the frozen source:** [Apache-2.0](https://raw.githubusercontent.com/miniflux/v2/eea1ec706fcd40687032804d197bb67cb0d96ef4/LICENSE).
- **Adoption test:** Test filter syntax, client API gaps, full-text behavior, and database backup before migration.
- **Source:** [miniflux/v2](https://github.com/miniflux/v2).

### FreshRSS

- **Choose it for:** Broad self-hosted feed-reader compatibility.
- **License at the frozen source:** [AGPL-3.0](https://raw.githubusercontent.com/FreshRSS/FreshRSS/71cda6bfe2301e0da789dc4141415b1090cfc0bd/LICENSE.txt).
- **Adoption test:** Pin a release rather than an edge branch; test extensions, update jobs, database restore, and client API.
- **Source:** [FreshRSS/FreshRSS](https://github.com/FreshRSS/FreshRSS).

### Tiny Tiny RSS

- **Choose it for:** Users who value rules and plugins.
- **License at the frozen source:** [GPL-3.0-or-later](https://raw.githubusercontent.com/tt-rss/tt-rss/bdd57ea86ff6beab69be2fc883d202181a6bd18a/COPYING).
- **Adoption test:** The original service retired in 2025; this repository is a continuation fork. Audit plugin mirrors and migration from old images.
- **Source:** [tt-rss/tt-rss](https://github.com/tt-rss/tt-rss).

### CommaFeed

- **Choose it for:** Users wanting a familiar web reader.
- **License at the frozen source:** [Apache-2.0](https://raw.githubusercontent.com/Athou/commafeed/4a8ffe250147fbcdaded433bdf3e54181ab71c25/LICENSE).
- **Adoption test:** Measure memory, update scheduling, API/client compatibility, and database export with your feed count.
- **Source:** [Athou/commafeed](https://github.com/Athou/commafeed).

### NewsBlur

- **Choose it for:** Teams deliberately operating a feature-rich feed service.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/samuelclay/NewsBlur/92b56e896c4ea6046377073204ad2894db9d0912/LICENSE.md).
- **Adoption test:** Operational complexity is part of the product; scope workers, search, storage, mail, and upgrade procedure before adoption.
- **Source:** [samuelclay/NewsBlur](https://github.com/samuelclay/NewsBlur).

### Stringer

- **Choose it for:** One reader wanting a focused queue.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/stringer-rss/stringer/8a4c90e2f1967028c3b5f3dca7fa3c6d356d25c4/LICENSE).
- **Adoption test:** Confirm maintenance, feed-fetch scheduling, import/export, and compatibility with current dependencies.
- **Source:** [stringer-rss/stringer](https://github.com/stringer-rss/stringer).

### selfoss

- **Choose it for:** Personal readers needing a flexible source model.
- **License at the frozen source:** [GPL-3.0](https://raw.githubusercontent.com/fossar/selfoss/4f97e67e1dcab2c2e7d63a5bd12ab6fbaca6a21d/COPYING).
- **Adoption test:** Verify update cron, database choice, plugins, and OPML/star export instead of assuming extension parity.
- **Source:** [fossar/selfoss](https://github.com/fossar/selfoss).

### Yarr

- **Choose it for:** Low-dependency personal reader.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/nkanaev/yarr/b75d4636119e71b9817fdd4e3e0b9ec267fe2f9b/license).
- **Adoption test:** Validate multi-user requirements, retention, full-text needs, and SQLite backup; its small scope is intentional.
- **Source:** [nkanaev/yarr](https://github.com/nkanaev/yarr).

### Fusion

- **Choose it for:** Personal users wanting a modern small deployment.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/0x2E/fusion/c31ade09b481fa877633899e8cf5d4c86a72c4f9/LICENSE).
- **Adoption test:** Confirm user model, import/export, retention, and database backup before replacing an established reader.
- **Source:** [0x2E/fusion](https://github.com/0x2E/fusion).

### RSS

- **Choose it for:** A maintainer-aligned personal workflow rather than a broad product.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/ssddanbrown/rss/b6ef210cb62913a58d6fc6b5aefbe02ec4f6ddfc/LICENSE).
- **Adoption test:** Upstream labels it low-maintenance with limited error handling; do not expect feature expansion or strong alerting.
- **Source:** [ssddanbrown/rss](https://github.com/ssddanbrown/rss).

### Feedbin

- **Choose it for:** Operators intentionally reproducing a hosted-service stack.
- **License at the frozen source:** [MIT](https://raw.githubusercontent.com/feedbin/feedbin/e976b6043219f7ebcdf3f35d148c4cb6f14d9bde/LICENSE.md).
- **Adoption test:** Upstream says it does not recommend production self-hosting without ample configuration time; treat it as an expert option.
- **Source:** [feedbin/feedbin](https://github.com/feedbin/feedbin).

### Leed

- **Choose it for:** Users wanting a traditional small reader.
- **License at the frozen source:** [AGPL-3.0](https://raw.githubusercontent.com/LeedRSS/Leed/a50fd997e40189989f70b40ff91fd6b788409188/License-AGPL-3.0.txt).
- **Adoption test:** Documentation is multilingual and older conventions may remain; verify current PHP compatibility, security defaults, update job, and maintenance.
- **Source:** [LeedRSS/Leed](https://github.com/LeedRSS/Leed).

## Run a reversible subscription migration

1. Import a small OPML and a deliberately failing feed.
2. Observe the real update process while the web reader is closed.
3. Test the chosen mobile/API client and representative filter rules.
4. Export feed subscriptions and saved/read state separately, then rebuild.

## What this guide covers

Feed consumption and update operations, not audience lists, newsletter authoring or mail delivery.

## Adjacent guides

- [self-hosted-read-it-later-landscape](https://github.com/rayrooto/self-hosted-read-it-later-landscape) — Use it when feed discovery is solved but durable capture and reading state are the next decision.
- [open-source-document-workflow-index](https://github.com/mckee19/open-source-document-workflow-index) — Use it when saved feed items become governed documents that need OCR, retrieval, or retention.
- [self-hosted-notification-routing-index](https://github.com/brownrasr/self-hosted-notification-routing-index) — Use it when selected feed events need controlled fan-out to phones, chat, email, or webhooks.

## Located sources and update policy

[`sources.json`](sources.json) binds each cited declaration to a commit, document, exact line range and local snapshot hash. [`assets/projects.json`](assets/projects.json) holds the normalized roles and decision axes. Current HEAD reachability is recorded separately; it is not a maintenance or security assessment. Licensing is version- and directory-specific. Re-read the selected release before adoption.

Run the local evidence and package checks, then opt into public-source reachability:

```bash
python3 scripts/verify_sources.py
python3 scripts/verify_sources.py --online
```

These checks validate source binding, excerpts, hashes, language, the decision asset and nonempty public content. They do not certify feature behavior, migration success or restore success. No popularity score, paid placement or affiliate ranking is used. Restricted-license and historical examples are explicit contrasts, not silently recommended open-source defaults.

If this saved you a decision, star the repo — it is how the next reader finds it.
