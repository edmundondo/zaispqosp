# Changelog

All notable changes to the Zambia privileged admin backend are recorded here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning
follows [Semantic Versioning](https://semver.org/) (MAJOR.MINOR.PATCH).

The version number shown here matches the `<meta name="app-version">` tag in
`index.html` and the `v{version}` badge in the page's footer.

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
