# Release process

## 1. GitHub release (automated)

Trigger the **Release Addon** workflow from the GitHub Actions tab:

1. Open the repo on GitHub → **Actions** → **Release Addon** → **Run workflow**.
2. Enter the new version (e.g. `1.0.4`). The current version is the first entry in `metadata.json`'s `versions` array.
3. Click **Run workflow**.

The workflow:
- runs `package.py <version>` to build `build/yvolodym-kicad-addon.zip` and regenerate `metadata.json`,
- commits the updated `metadata.json` back to the dispatched branch,
- creates a `v<version>` tag and a GitHub release with the zip attached,
- prints a summary with current → new version and a reminder for step 2.

If the version already exists in `metadata.json`, `package.py` aborts and the workflow fails — bump and retry.

## 2. KiCad PCM metadata (manual, GitLab)

PCM only ships the zip after the metadata index on GitLab is updated:

- Fork [`gitlab.com/kicad/addons/metadata`](https://gitlab.com/kicad/addons/metadata) and create a branch.
- In `packages/com.github.yvolodym.kicad-libraries/` (create the folder if missing), commit:
  - `metadata.json` (copy from this repo's root after the workflow ran)
  - `resources/icon.png`
- Open a merge request.

## Local fallback

If you need to build a zip without GitHub Actions:

```bash
python package.py 1.0.4
```

The zip lands in `build/yvolodym-kicad-addon.zip` and `metadata.json` is updated in place. You then have to create the GitHub release manually.
