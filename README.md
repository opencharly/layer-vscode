# layer-vscode

Visual Studio Code for OpenCharly images, launchable as `/usr/bin/code` on every
distro.

The `vscode` candy installs Microsoft's VS Code by two paths that converge on the
same launcher: on Arch a sha256-verified direct tarball extracted to
`/opt/vscode` and symlinked to `/usr/bin/code`, and on Fedora the `code` RPM from
Microsoft's yum repo. Either path lands an executable at `/usr/bin/code` that
prints its semantic version, so both are verifiable the same cross-distro way.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `vscode` |
| Version | pinned `1.123.0` (`VSCODE_VERSION` var) + `VSCODE_SHA256` |
| Binary | `/usr/bin/code` |
| Packages | Arch: runtime libs (`gtk3`, `nss`, `libsecret`, …); Fedora: `code` RPM |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-editor-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-vscode:v2026.239.1631'
```

Then, inside the built image:

```bash
code --version          # prints the semantic version
code .                  # open a folder
```

The candy's `plan:` asserts the launcher at `/usr/bin/code` and `code --version`
exiting 0 with a semantic version, on both build and deploy scope.

## Layout

- `charly.yml` — the `vscode:` candy entity (the `VSCODE_VERSION` /
  `VSCODE_SHA256` vars, the per-distro packages/repo, the Arch tarball `run:`
  step, the `check:` assertions) and the embedded `vscode-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-tools:vscode`
- `/charly-coder:dev-tools` — CLI dev tools companion
- `/charly-coder:typst` — document tooling sibling
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
