# simc-cli-mirror

Mirrors the unmodified Windows `simc.exe` from
[downloads.simulationcraft.org/nightly/](http://downloads.simulationcraft.org/nightly/)
to GitHub Releases, so downstream tools can pull it over HTTPS with a known
SHA256 (computed and exposed by GitHub for every release asset).

A daily Actions job grabs the latest `simc-*-win64.7z`, extracts `simc.exe`,
and publishes a release tagged `simc-<version>-<sha>`. The binary is
byte-identical to the upstream archive — we don't modify, rebuild, sign, or
otherwise touch it.

simc is GPLv3. See `LICENSE`. Upstream: https://github.com/simulationcraft/simc.

## Each release

One asset: `simc.zip` containing a single file, `simc.exe` (~113 MB raw,
~15 MB zipped). The release notes link to the exact upstream archive URL
we mirrored from. The asset SHA256 is shown next to it on the release page
and in the release-assets API (`digest` field).

## Verifying

After unzipping, `sha256sum simc.exe` and compare to the SHA256 of the
upstream binary: download the upstream `.7z` from the URL in the release
notes, `7z e <archive> simc-*-win64/simc.exe`, hash the result. The two
must match.

## Antivirus false positives

Some AV products flag `simc.exe` (e.g. `Trojan:Win32/Sabsik.FL.A!ml`). The
binary is byte-identical to upstream — verify with the SHA256 check above.
Submit to Microsoft (https://www.microsoft.com/en-us/wdsi/filesubmission)
or whitelist the path locally.
