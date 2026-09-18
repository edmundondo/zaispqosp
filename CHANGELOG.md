# Changelog

All notable changes to the Zambia privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

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
