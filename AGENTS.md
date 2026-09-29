# AGENTS.md — layer-desktop-media

Standalone candy repo for the `desktop-media` layer — the GStreamer/VLC codec
set, thumbnailer libs, and the ALSA↔PipeWire bridge for a KDE workstation. The
candy lives in `charly.yml` at the repo root: the `require:` dep on
`layer-pipewire`, the `arch` package arm, and the `plan:` file/package `check:`
steps. It carries an owning `desktop-media-skill:` entity, projected as
`/charly-selkies:desktop-media`.

Canonical files:

- `charly.yml` — the `desktop-media:` candy entity and its `desktop-media-skill:`
  skill entity.
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:desktop-media` — the owning skill: the KDE workstation media
  stack (GStreamer/VLC codecs, thumbnailers, the ALSA↔PipeWire bridge), its
  packages, and the `plan:` checks that prove them. Load before editing,
  building, or troubleshooting the layer.
- `/charly-selkies:pipewire` — the PipeWire audio/media server the candy's ALSA
  bridge routes through.
- `/charly-selkies:ffmpeg` — the transcoder stack (negativo17 nonfree build) used
  by downstream media consumers. Load when changing codec claims.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package sections).
  Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the GStreamer
  `libav`/`va` plugin `.so` files, the `pipewire-alsa` package and its
  `/etc/alsa/conf.d/99-pipewire-default.conf` drop-in, and the
  `pavucontrol`/`ffmpegthumbnailer` binaries. They must stay valid on the `arch`
  arm they run on.

## Modify this repo

- Edit the `desktop-media:` candy entity in `charly.yml`. The `require:` dep pins
  the PipeWire provider; a package or behaviour change that a downstream box
  relies on belongs in the `plan:` as an observable `check:` step.
- Host-firmware entries (`alsa-firmware`, `sof-firmware`) are deliberately
  excluded — the streamed VM has no physical audio hardware.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
