# ArtCraft Launcher assets

Canonical metadata and assets for [ArtCraft Launcher](https://github.com/storytold/craft-launcher):
the base app catalog, content-addressed app icons and screenshots. ArtCraft Services consumes
this repository at website build time, enriches the catalog with release metadata, and signs
the published TOML. The signing key remains in the website's build environment, not here.

## Updating the catalog and icons

- Edit `catalog.toml` for catalog entries, metadata and descriptions; leave the release tables
  and generated timestamps to the website build.
- Store PNGs in `app-icons/<app-id>-<sha256>.png`, with the full lowercase SHA-256 digest of
  the file bytes. The catalog's `icon_url` must point to the matching file served at
  `https://getartcraft.com/launcher-app-icons/` and `icon_sha256` must contain that digest.
- Add attribution and any required licenses in `app-icons/`. Leave previously published
  content-addressed icons available for cached catalogs.

A push to `main` or a new `v*` screenshot tag notifies ArtCraft Services to rebuild.
The workflow requires the organization-provided `ARTCRAFT_SERVICES_DISPATCH_TOKEN`
with repository dispatch access to ArtCraft Services. Each build uses a particular catalog
commit and immutable screenshot tag revisions.

Screenshots are served from `https://getartcraft.com/launcher-screenshots/<tag>/<app>/<key>.jpg`.
The launcher also reads the fallback `https://raw.githubusercontent.com/storytold/craft-launcher-assets/<tag>/<app>/<key>.jpg`
at a pinned tag (`SCREENSHOTS_REV` in `crates/core/src/details.rs`), and keeps what it downloads
under that tag, so a published tag must never change.

## Changing a picture

1. Add or replace JPEGs (1200 × 750 px, 8:5, no metadata, ideally under 150 KB; the launcher
   refuses anything over 2 MB or not a JPEG).
2. Update `ATTRIBUTION.md` in the same change.
3. Tag the result (`v2`, `v3`…) and bump `SCREENSHOTS_REV` in the launcher. Older launchers keep
   reading the tag they know.

Every picture's license is in `ATTRIBUTION.md`: the apps' UI is under each app's own license, and
the artwork, photos and footage shown are public domain or were made with the apps themselves.
