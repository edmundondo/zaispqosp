# Changelog

All notable changes to the Zambia privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

## [0.7.0] — 2026-09-23

### Added
- Raw CSV export can now download **`customers`** (follow-up phone numbers, E.164) and
  **`customer_emails`** (follow-up emails) for the selected site.

### Fixed
- CSV export neutralises spreadsheet formula injection in visitor-typed text (cells starting with
  `=`, `+`, `-`, `@`), while leaving `+263…`-style phone numbers readable.

## [0.6.0] — 2026-09-23

### Added
- **Pipeline health card on Overview.** Per table, for the selected site: when the last real
  public submission arrived, counts for the last 24h / 7 days, and distinct devices (7d), with a
  🟢/🟡/🔴/⚪ freshness flag. Added because a silent backend rejection (below) looked exactly like
  "no testers yet" on this dashboard.
- `supabase-antispam-migration.sql` lives in `zwispqosp` (shared project, one migration for all six sites).

### Fixed
- A panel whose backend fetch failed (e.g. an expired login → `JWT expired`) stayed on
  "Loading…" forever because the error was only logged to the console. It now shows the real
  error in the panel, with a sign-in-again hint for auth errors.
- Root cause of missing tester results, fixed in the shared database (migration
  `add_device_id_antispam_and_open_lang_codes`): the public sites send a `device_id` column that
  didn't exist, so every public insert was rejected. Also opened `translations.lang` to any
  2–4 letter code and added the per-device rate-limit trigger the demo sites' comments promised.

## [0.5.1] — 2026-09-18

### Added
- Malawi (`mw`) added to `SITE_LABELS`/`MISSING_LANGS`/`PROVIDER_DIRECTORY` (this app is kept as a
  byte-identical copy of `zwispqosp` aside from title/version/`currentSite`). See `zwispqosp`'s
  CHANGELOG v0.6.1 entry for the full detail; the new `maispqosp` repo (v0.1.0) is Malawi's own
  copy of this same admin app.

## [0.5.0] — 2026-09-17

### Added
- **Role-based access control (RBAC), ported from zwispqosp v0.6.0.** `admins.role` is now one of
  `viewer` (read-only, the default for any newly-added admin), `country_admin` (read/write, scoped
  to the site codes in `admins.scope`), or `global_admin` (full read/write across every country,
  but only while break-glass is switched on). A "Global Admin · break-glass ON/OFF" badge now sits
  next to "Signed in as…" in the header, with a toggle for eligible global admins, logged to a new
  `admin_audit_log` table on every activation/deactivation via the `toggle_break_glass()` RPC. The
  moderation-delete, ISP-license-save, and translation approve/reject actions now check
  `canWrite(site)` client-side first; the real enforcement is server-side RLS shared across all
  five country apps (`has_write_access(site)` / `is_global_admin()`).

### Fixed
- **The site-selector dropdown/header only showed a country once it had a live row in
  `qos_reports`** — since Zambia had none yet, the dropdown rendered blank instead of showing
  "Zambia (zm)". `populateSiteSelect()` now always seeds every known country from `SITE_LABELS`
  up front, so this app (and the other four) show their own country correctly from first load.

Changelog file introduced with this release. Earlier history (v0.1.0 initial launch through the
v0.3.1 this app carried before this update) was not previously recorded here — see `git log` for
the raw commit history; this app has always been kept as a copy of the shared `zwispqosp` template
at whatever version that template was at build time, so its own dated feature history lives in
`zwispqosp`'s CHANGELOG.

## [0.4.0] — 2026-09-17

### Added
- **Ported the full Drill-down Explorer from `zwispqosp` v0.5.0/v0.5.1** (this app is kept as a
  byte-identical copy of zwispqosp aside from title/version/`currentSite`): a single, condensed,
  breadcrumb-navigable Explorer replacing the previous stacked-card layouts across Provider
  analytics (City → Area → ISP → reports, now including Zambia's real, sourced areas from
  `zaispqosd` v1.1.0), ISP licensing, Benchmark reports, Moderation and Translations.
- **Benchmark reports now render inline** as the leaf of the same breadcrumb Explorer, instead of
  opening in a new tab — full stat grid, 12-week QoS trend, complaint-cluster keyword tally and
  scrollable raw-comments table, with Print/Save-as-PDF via a scoped in-page print.
- Every Overview stat tile is now a clickable entry point into the relevant drill-down.

### Fixed
- Same rating-form truncation fix as `zaispqosd` v1.1.0 (CSS grid `0fr → 1fr` expand, no fixed
  `max-height` cap) wherever this admin app renders the same expandable-row pattern.

### Notes
- See `zwispqosp`'s CHANGELOG v0.5.0/v0.5.1 entries for the full detail this app inherits.
