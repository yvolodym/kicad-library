# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A personal KiCad library distributed as a KiCad Package Manager (PCM) addon. It bundles symbols and footprints for parts the author needed but did not find in stock KiCad libraries. Targets KiCad 9.0.0+. The `3dmodels/yvolodym.3dshapes/` directory exists but is currently empty — no footprint references a 3D model.

## Documentation languages

`README.md` is in **German** (end-user facing). Internal docs (this file, `release-kicad-addons.md`) and `metadata*.json` strings are in **English**. Keep these conventions when editing.

## Library Layout

There is **one** library, not many — a single triplet of files keyed off the `yvolodym` name:

- `symbols/yvolodym.kicad_sym` — all symbols live in this one file.
- `footprints/yvolodym.pretty/` — all footprints (`.kicad_mod`) live here.
- `3dmodels/yvolodym.3dshapes/` — slot for `.step`/`.wrl` files (currently empty).

The library nickname for symbols' `Footprint` property is `PCM_yvolodym:` — that is the auto-registered nickname when KiCad installs the addon via PCM. When adding a part, keep symbol/footprint/3D-model names aligned and prefix the symbol's footprint reference with `PCM_yvolodym:`.

## Release Pipeline

The PCM addon is built and published in two places: a GitHub release (binary zip) and the KiCad PCM metadata repo on GitLab (so it appears in the PCM UI). Both must be updated for a release to be visible to users.

### `package.py`

`python package.py [version]` (Python 3, no third-party deps) does:

1. Validates `version` against `^\d{1,4}(\.\d{1,4}(\.\d{1,6})?)?$` and refuses duplicates already present in `metadata.json`'s `versions` array.
2. Zips `3dmodels/`, `footprints/`, `symbols/`, `resources/` into `build/yvolodym-kicad-addon.zip`, embedding a stripped-down `metadata.json` (single-version entry, no download fields) **inside** the zip — PCM requires this internal copy.
3. Rewrites the top-level `metadata.json` by **prepending** a full version record (sha256, size, install size, GitHub download URL) to the existing `versions` array. Older entries are preserved.

`metadata.template.json` is the source of truth for the static fields (name, description, identifier, author, license). The top-level `metadata.json` is regenerated from this template on every run — do not hand-edit it; edit the template and re-run `package.py`. Note however that the script overwrites the `versions` array with `[new] + existing`, so historical entries persist across regenerations.

The download URL is hardcoded to `https://github.com/yvolodym/kicad-library/releases/download/v{VERSION}/yvolodym-kicad-addon.zip` — i.e., the GitHub release tag must be `v<version>` (with the `v` prefix), even though `package.py` itself takes the version without it.

### GitHub Actions (`.github/workflows/release.yml`)

Manually triggered via `workflow_dispatch` ("Release Addon" → Run workflow → enter version). The workflow runs `package.py`, commits the updated `metadata.json` back to the dispatched branch, then creates a `v<version>` tag and GitHub release with the zip attached. Needs `permissions: contents: write` (already set). Push goes to `${{ github.ref_name }}`, so it works regardless of whether the default branch is `main` or `master`.

### GitLab metadata submission

`release-kicad-addons.md` describes the manual GitLab step: fork `gitlab.com/kicad/addons/metadata`, create folder `packages/com.github.yvolodym.kicad-libraries/`, and submit `metadata.json` plus `resources/icon.png` via merge request.
