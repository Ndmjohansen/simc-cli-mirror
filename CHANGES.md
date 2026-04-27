# Modifications statement (GPL3 §2.a)

The `simc.exe` binary published in this repository's GitHub Releases is
**unmodified**. It is extracted as-is from the upstream nightly archive at
http://downloads.simulationcraft.org/nightly/.

This repository:

- Does not patch simc source code.
- Does not rebuild simc.
- Does not strip, resign, or otherwise alter the binary.
- Does not change file metadata beyond the rename (`simc.exe` →
  `simc-x86_64-pc-windows-msvc.exe`) required by Tauri's sidecar naming
  convention.

The SHA256 in each release matches the binary inside the corresponding
upstream `.7z`. To verify independently, download the upstream archive from
the URL noted in each release's `UPSTREAM_URL.txt` asset, extract `simc.exe`,
and compute its SHA256. The hash should be identical.

The full source for simc is at https://github.com/simulationcraft/simc.
