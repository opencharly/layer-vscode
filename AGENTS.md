# AGENTS.md — layer-vscode

Standalone candy repo for the `vscode` layer — Visual Studio Code, installed
per-distro onto a shared `/usr/bin/code` launcher. The candy lives in `charly.yml`
at the repo root: the `VSCODE_VERSION` / `VSCODE_SHA256` vars, the per-distro
packages/repo, the Arch tarball `run:` step, the `check:` assertions, and the
embedded `skill:` entity projected into the marketplace corpus as
`/charly-tools:vscode`.

Canonical files:

- `charly.yml` — the `vscode:` candy entity and the `vscode-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-tools:vscode` — the owning skill. The two install paths, the shared
  launcher, and the version pin. Load before editing or troubleshooting the
  layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package/repo
  sections, service declarations). Load before editing any entity field or plan
  step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence. The version
  check runs at both build and deploy scope; keep both valid.
- The Arch path downloads from a versioned Microsoft URL with a sha256 the repo
  controls; a version bump moves `VSCODE_VERSION`, `VSCODE_SHA256`, and the URL
  template together.

## Modify this repo

- Edit the `vscode:` candy entity AND the `vscode-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a behaviour
  change not mirrored in the skill leaves the corpus stale.
- The Arch runtime-library package list mirrors the upstream `code` package's
  `depends`; keep it complete when the app's requirements move.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
