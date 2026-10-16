# Tremulous Revamped: test builds

Test builds of [Tremulous Revamped](https://github.com/MeepingsCorner/Tremulous-Revamped) with our
open pull requests applied, for Windows and Linux (x86_64), in one zip.

**For normal play use the official release:** https://github.com/MeepingsCorner/Tremulous-Revamped/releases.
These builds exist so the changes can be tried together before upstream merges them. When upstream has
merged or released them, this repository stops; nothing here is meant to replace upstream.

## Download and start

Get `Tremulous-Revamped-win64-linux-x86_64.zip` from the newest pre-release under
[Releases](../../releases), check it against `SHA256SUMS.txt`, unzip it anywhere, then:

| System | Start |
|---|---|
| Windows 10/11 (64-bit) | double-click **`Tremulous-Windows.exe`** |
| Linux x86_64 (glibc 2.28 or newer) | run **`./Tremulous-Linux.x86_64`** |

Both share `base/` and `sfx_hd/`. Godot loads the game file named like the program, so the small
`Tremulous-Windows.pck` / `Tremulous-Linux.pck` (the same file) is there twice. The optional 4K `hd`
folder goes next to the programs, as in the official release.

## What is in it

Each release lists what it contains: upstream's code plus our changes, most of them sent upstream as pull requests. Our pull requests to upstream:
https://github.com/MeepingsCorner/Tremulous-Revamped/pulls?q=is%3Apr+author%3Aluuvasara

## Where the files come from

| File | Source |
|---|---|
| `Tremulous-Windows.exe` | Godot 4.7.2's official signed Windows executable, renamed (unchanged bytes; sha256 in the release) |
| `Tremulous-Linux.x86_64` | Godot 4.7.2's official Linux export template, renamed |
| `trem_godot.dll`, `libtrem_godot.so`, `*.pck` | built from the source branch named in the release |
| audio DLLs, `base/`, `sfx_hd/` | unchanged from the official Tremulous Revamped release |

## Source

Each release names the exact source commit it was built from; see its release notes for where that source is published.
The build kit used for the Linux part is described in [README-kit.md](README-kit.md).

## Licence

GPL-2.0-or-later, like upstream's code (`LICENSE`). The game data in `base/` has its own terms: see
`base/COPYING` and `base/CC` inside the zip.

Disclosure: these builds were prepared with AI assistance; the builds and tests were run and checked by the contributors.
