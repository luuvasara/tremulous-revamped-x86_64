# Tremulous Revamped: Linux build kit

This repository builds a **Linux x86_64 release zip** of
[Tremulous Revamped](https://github.com/MeepingsCorner/Tremulous-Revamped), laid out like
upstream's Windows zip, and it merges with upstream's Windows zip into one folder that runs on both systems.

**Upstream is the real home.** This kit exists only so Linux players have a download until
upstream publishes Linux builds itself. **It stops when upstream releases Linux builds**: at that
point use theirs. Nothing here changes the game: it builds the plain upstream commit.
Everything is meant to be merged into upstream; fork it, copy it, or send it on.

## What you get

The release is one file, `Tremulous-Revamped-linux-x86_64.zip`: the folder `Tremulous-Revamped/` with
`Tremulous.x86_64`, `Tremulous.pck`, `libtrem_godot.so`, `base/`, optional `sfx_hd/` and
`README-LINUX.txt`. Unzip anywhere, run `./Tremulous.x86_64`. The optional 4K `hd` folder goes next to
the executable, as on Windows.

**It is built to merge with upstream's Windows zip**: same top-level folder, and every file that exists in
both zips (`Tremulous.pck`, `base/`, `sfx_hd/`: 230 files against upstream's v1.0 Windows zip) is
byte-identical. Unzip both into one folder, in either order, and nothing changes; you get one folder that
runs on Windows (`Tremulous.exe`) and Linux (`Tremulous.x86_64`). The Linux zip has no `README.txt` (the
Windows zip's one stays); the Linux notes are in `README-LINUX.txt`. `tools/merge-check.sh WIN_ZIP LINUX_ZIP`
proves this for any pair of zips (both extraction orders, content hashes, overlap and Linux-only lists).
We do not release a Windows or combined build: Windows stays upstream's.

For upstream's own use, `--combined` (off by default) builds one zip that already holds both systems
(`Tremulous-Revamped-win64-linux-x86_64.zip`, the Windows library taken from the committed
`godot/bin/trem_godot.dll`).

Requirements: 64-bit Linux, **glibc 2.28 or newer** (Ubuntu 18.10+, Debian 10+, Fedora 29+,
RHEL/Rocky 8+, Arch, SteamOS), a Vulkan 1.1 or OpenGL 3.3 driver, X11 or Wayland. Sound is Godot's
own audio: FMOD and Steam Audio exist only for Windows.

## Why a kit and not just a binary

The `libtrem_godot.so` committed in upstream's `godot/bin/` needs glibc 2.34 (it was built on a
current distro), so it fails on older systems. Building the game library against an old glibc fixes
that. The Godot Linux engine binary (official export template) already needs only glibc 2.28.

## Build it yourself (one command)

```sh
git clone https://github.com/MeepingsCorner/Tremulous-Revamped
tools/build_release.sh --src Tremulous-Revamped --out out            # Linux zip
tools/build_release.sh --src Tremulous-Revamped --out out --combined # optional: also a Windows+Linux zip
```

What it does, in order:

1. fetches [zig](https://ziglang.org) with pip if you have none (`ZIG=/path/to/zig` to use yours),
   used only as the C compiler and linker, targeting `x86_64-linux-gnu.2.28`;
2. `cargo build --release --target x86_64-unknown-linux-gnu -p trem_godot` with path remapping, so no
   local path ends up in the library; checks `objdump -T` for the highest `GLIBC_` symbol;
3. copies the project to a staging folder (your checkout stays untouched), adds the `Linux` export
   preset from `presets/linux-preset.cfg` if the project has none, imports and exports with Godot;
4. writes the zip(s) reproducibly (sorted names, fixed times from the commit date, fixed modes,
   `+x` on the executables) and `SHA256SUMS.txt`.

You need: Rust (the workspace `rust-version`), Python 3, pip, a **Godot 4.7.2 editor** (`GODOT=...`)
and the **4.7.2 export templates** installed (Linux release template; for `--combined` also the
Windows release template). Upstream's v1.0 Windows exe is the 4.7.2 template, which is why the same
version is used here. For `--combined` the Windows library is upstream's own `trem_godot.dll` as committed in `godot/bin/` (so the Windows half equals upstream's Windows zip, file for file, except the README); `--windows-dll FILE` uses another one and `--windows-mingw` cross-builds it (`x86_64-w64-mingw32-gcc` and the `x86_64-pc-windows-gnu` Rust target). `--sfx-hd DIR` adds upstream's `sfx_hd/` folder (derived sound data that is not in
git: copy it from upstream's Windows zip).

`tools/test-matrix.sh ZIP` runs the zip headless in distro containers against a dedicated server on a
network with no outside access, and prints PASS/FAIL per image.

## Moving it into upstream

The kit is three small pieces, each meant to land in upstream unchanged:

- the `Linux` preset (`presets/linux-preset.cfg`) appended to `godot/export_presets.cfg`;
- `tools/build_release.sh` (+ `tools/zig-cc-wrapper.sh`, `tools/README-LINUX.txt.in`, `tools/README-BOTH.txt.in`)
  next to `tools/build_release.ps1`; with the preset committed, the script skips the append and
  `--src` defaults to the repository root;
- a README section ("Play on Linux") pointing at the Linux zip.

To use this repository as a fork base: clone upstream, copy `tools/` and `presets/` in, commit.

## Licence

GPL-2.0-or-later, like upstream's code (`LICENSE`). The game data in `base/` has its own terms: see
`base/COPYING` and `base/CC` inside the zip.
