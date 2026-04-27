# simc-cli-mirror

This repository **redistributes the unmodified Windows CLI binary** of
[SimulationCraft](https://github.com/simulationcraft/simc) from the upstream
[nightly download server](http://downloads.simulationcraft.org/nightly/).

It exists so that downstream tools can consume `simc.exe` over HTTPS from
GitHub Releases — with a published SHA256 — instead of scraping a
third-party HTTP directory listing per user.

We do **not** modify simc. We do **not** rebuild simc. A scheduled CI job
downloads the upstream `.7z`, extracts `simc.exe`, computes its SHA256, and
re-publishes the pair as a GitHub Release. Releases are driven entirely by
GitHub Actions on the smallest available runner; there is nothing to build
locally.

simc is licensed under **GPLv3**. This repository inherits that license. See
`LICENSE` for the full text.

Upstream source: https://github.com/simulationcraft/simc

## How it works

`.github/workflows/mirror-simc.yml` runs on `ubuntu-latest`:

1. Daily cron at 06:30 UTC, plus on-demand `workflow_dispatch`.
2. Scrapes `http://downloads.simulationcraft.org/nightly/?C=M;O=D` for the
   most recent `simc-*-win64.7z`.
3. If a GitHub Release already exists for that filename's tag, exits early.
4. Otherwise: `curl` the archive, `7z e` to pull out `simc.exe`, rename to
   the Tauri sidecar triple form (`simc-x86_64-pc-windows-msvc.exe`),
   `sha256sum` the result.
5. `gh release create` with the binary, the `.sha256`, and an
   `UPSTREAM_URL.txt` pinning the exact archive we extracted from.

Release tag pattern: `simc-<version>-<sha>` (the upstream filename minus the
`-win64.7z` suffix).

## Consuming a release

Latest release is always at:

```
https://github.com/Ndmjohansen/simc-cli-mirror/releases/latest
```

Each release has three assets:

| Asset                                       | Purpose                              |
|---------------------------------------------|--------------------------------------|
| `simc-x86_64-pc-windows-msvc.exe`           | the simc CLI binary                  |
| `simc-x86_64-pc-windows-msvc.exe.sha256`    | sha256 sidecar                       |
| `UPSTREAM_URL.txt`                          | exact upstream URL we mirrored from  |

GitHub Releases support fetching the latest asset via API:

```
GET https://api.github.com/repos/Ndmjohansen/simc-cli-mirror/releases/latest
```

returns JSON with the asset URLs.

## Verifying the mirror is honest

If you want to verify that the binary in a release is byte-identical to the
upstream archive's `simc.exe`:

1. Read `UPSTREAM_URL.txt` from the release.
2. Download the upstream archive from that URL.
3. `7z e <archive> simc-*-win64/simc.exe`.
4. `sha256sum simc.exe`.
5. Compare to `simc-x86_64-pc-windows-msvc.exe.sha256` from the release.

The hashes must match.

## Why a separate repo

A narrow, single-purpose repo:

- Clean GPL3 boundary — the binaries' license terms apply to this repo, not
  to whatever consumes them.
- Anyone can consume the releases — no project-specific assumptions.
- Easy to read, easy to audit. The whole pipeline is one workflow file.

## Attribution

SimulationCraft is the work of the [SimulationCraft project](https://github.com/simulationcraft/simc)
and its contributors. Please direct simc bugs and feature requests upstream;
this repository only redistributes their work.
