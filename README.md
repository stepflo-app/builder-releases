# Stepflo Builder — release artifacts

The installable Stepflo Builder lives here as **GitHub Release assets**, one
release per version, tagged `v<version>`. Nothing is committed to this repo's
tree: release assets are stored by GitHub, so they cost a clone nothing.

This repo is **public on purpose**. Release assets on a private repo need a
token, and the supported install is a one-liner a stranger pipes into `bash`.

## Install

```bash
curl -fsSL https://app.stepflo.ai/builder/install.sh | bash
```

The installer itself still ships from the app host. It downloads from
`https://github.com/stepflo-app/builder-releases/releases/latest/download`,
which is a moving alias for the newest release, so that URL never changes.

## What each release carries

| asset | purpose |
| --- | --- |
| `stepflo-builder-<version>.tar.gz` | the app, one top-level directory |
| `SHA256SUMS` | `<sha256>  stepflo-builder-<version>.tar.gz` — the authority |
| `VERSION` | `<version>` — display only, never used to pick the artifact |
| `install.sh` | the installer, byte-identical to the app host's copy |

`SHA256SUMS` is fetched first and names the artifact, so the filename and the
checksum come from the same file and cannot disagree.

## Publishing a version

From `stepflo-app`:

```bash
cd builder && ./release.sh --out /tmp/rel          # builds VERSION, SHA256SUMS, tarball, install.sh
gh release create "v$(cat /tmp/rel/VERSION)" /tmp/rel/* \
  --repo stepflo-app/builder-releases --title "Stepflo Builder $(cat /tmp/rel/VERSION)" --notes ""
```

Always bump `builder/VERSION` first. A published version's bytes never change.

## Why not a private repo, or the app repo?

The app repo is private, so its release assets would require auth a public
one-liner cannot supply. Committing tarballs to the app repo instead made every
clone carry every release forever — git history is permanent and tarballs do not
delta-compress. This repo exists to pay neither cost.
