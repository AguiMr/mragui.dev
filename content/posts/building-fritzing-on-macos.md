---
title: "Building Fritzing 1.0.7 from source on macOS with one script"
date: 2026-10-01
draft: false
tags: ["fritzing", "macos", "apple-silicon", "electronics", "build"]
description: "A single script that builds Fritzing 1.0.7 natively on macOS, using only official dependency sources and nothing from SourceForge."
---

[Fritzing](https://fritzing.org) is a great tool for breadboard, schematic and PCB diagrams. It's open source (GPLv3), but the prebuilt binaries are a paid download. You can build it yourself, but on macOS the build is fiddly: Fritzing expects a very specific set of dependencies in specific places.

So I wrapped the whole build in one script: **[Fritzing-macos](https://github.com/AguiMr/Fritzing-macos)**.

## Usage

```bash
git clone https://github.com/AguiMr/Fritzing-macos.git
cd Fritzing-macos
chmod +x build_fritzing_macos.sh
./build_fritzing_macos.sh
```

You need [Homebrew](https://brew.sh) and the Xcode Command Line Tools. If the tools are missing, the script starts their install and asks you to run it again. The first build takes about 20–40 minutes. After that, re-running skips everything that's already built.

When it finishes:

```bash
open "$(find build-workspace -name Fritzing.app | head -1)"
```

I tested it on **macOS 26 (Tahoe), Xcode 26, Apple Silicon**. It should also work on Intel Macs and on macOS 13+ with Xcode 15+, but I haven't verified that.

## Where every dependency comes from

I didn't want a build script that downloads code from random mirrors. Every dependency comes from an official source, or is vendored in the repo with checksums:

| Dependency | Version | Source |
|---|---|---|
| Qt | 6.5.3 | Official Qt servers, via `aqtinstall` in its own Python environment |
| fritzing-app | 1.0.7 | github.com/fritzing/fritzing-app |
| fritzing-parts | `develop` | github.com/fritzing/fritzing-parts |
| Boost (headers) | 1.84 | archives.boost.io |
| libgit2 | 1.7.1 | github.com/libgit2 |
| svgpp | 1.3.1 | github.com/svgpp |
| QuaZip | 1.4 | github.com/stachenov/quazip |
| Clipper1 | 6.4.2 | **Vendored** in the repo, with SHA-256 checksums |
| ngspice | 46 | github.com/imr/ngspice |

## Problems I ran into

**Clipper is only on SourceForge.** Fritzing 1.0.7 needs Clipper1 version 6.4.2, which is only published on SourceForge. That site is unreachable on many networks, and I didn't want the build to rely on it. So the original, unmodified source is committed to the repo, with checksums and a provenance note, so you can see exactly what gets compiled.

**ngspice 42 doesn't compile with current Xcode.** Fritzing 1.0.7 expects a folder named `ngspice-42`. Version 42 includes a library (`cppduals`) that fails to build against the C++ standard library in Xcode 16.3 and later. ngspice 46 builds cleanly and is compatible, so the script builds 46 and puts it in the folder Fritzing expects.

**Fritzing expects its dependencies in hardcoded locations.** The build files assume every dependency sits right next to the `fritzing-app/` folder. So the script creates a self-contained `build-workspace/` folder inside the repo and puts everything there, instead of spreading files into the folder above it.

**The app crashed on launch with a `QtCore5Compat` error.** Qt's deploy tool (`macdeployqt`) copies the Qt libraries the app needs into the app bundle, but it misses ones that only bundled third-party libraries need. QuaZip needs `QtCore5Compat`, so the app crashed with `Library not loaded: @rpath/QtCore5Compat`. The script now finds and fixes these after the deploy step.

**"Cannot read file /bins/core.fzb".** The parts library has to be placed inside the app bundle (`Contents/parts`). If you see this error, a step was skipped, and running the script again fixes it.

## License

Fritzing is GPLv3, and Clipper is under the Boost Software License. The repo only contains the build script and the vendored Clipper source. It doesn't redistribute Fritzing itself. If Fritzing is useful to you, consider supporting the project by buying it.
