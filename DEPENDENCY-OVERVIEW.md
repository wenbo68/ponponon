# Dependency Overview

- Date: 2026/06/07
- Sources: none at repo root. Checked for `package.json`, `pyproject.toml`, `requirements*.txt`, `go.mod`, `Cargo.toml`, `Gemfile`, `pom.xml`, `build.gradle` — none found.

> Notes: This repository is a **source-recovery archive** of `@anthropic-ai/claude-code@2.1.88`, reconstructed from the published `cli.js.map`. It has **no dependency manifest of its own** — the recovered source under `src/` is not a buildable package. The only manifest in the repo is the original one **bundled inside `claude-code-2.1.88.tgz`** at `package/package.json`. In the published package, all runtime libraries are pre-bundled into `cli.js`, so that manifest declares **empty `dependencies`** and only lists platform-specific `optionalDependencies`. A populated `node_modules/` (~190 packages) is also present, representing the libraries the original build was compiled against (e.g. `@anthropic-ai/sdk`, `react`, `ink`-fork, `@modelcontextprotocol/*`, `@aws-sdk/*`, `axios`, `chalk`, `zod`), but these are not declared in any repo manifest. The list below reflects the tarball's declared manifest verbatim.

## List

### claude-code-2.1.88.tgz → package/package.json — dependencies

- (empty)
    - The published package declares no runtime dependencies; all third-party libraries are bundled directly into the compiled `cli.js`.

### claude-code-2.1.88.tgz → package/package.json — optionalDependencies

- `@img/sharp-darwin-arm64`: `^0.34.2`
    - Prebuilt `sharp` (libvips) image-processing binary for macOS Apple Silicon; powers image resize/format handling for pasted/attached images.
- `@img/sharp-darwin-x64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for macOS Intel.
- `@img/sharp-linux-arm`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Linux ARM (32-bit).
- `@img/sharp-linux-arm64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Linux ARM64.
- `@img/sharp-linux-x64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Linux x64 (glibc).
- `@img/sharp-linuxmusl-arm64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Linux ARM64 on musl libc (Alpine).
- `@img/sharp-linuxmusl-x64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Linux x64 on musl libc (Alpine).
- `@img/sharp-win32-arm64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Windows ARM64.
- `@img/sharp-win32-x64`: `^0.34.2`
    - Prebuilt `sharp` image-processing binary for Windows x64.

## Taxonomy

- Runtime dependencies: external libraries required at run time.
    - (none declared — all bundled into `cli.js`)
- Image processing (optional, platform-specific binaries): prebuilt `sharp`/libvips native addons selected per OS+architecture, used to resize and re-encode images supplied to the CLI.
    - `@img/sharp-darwin-arm64`
    - `@img/sharp-darwin-x64`
    - `@img/sharp-linux-arm`
    - `@img/sharp-linux-arm64`
    - `@img/sharp-linux-x64`
    - `@img/sharp-linuxmusl-arm64`
    - `@img/sharp-linuxmusl-x64`
    - `@img/sharp-win32-arm64`
    - `@img/sharp-win32-x64`
