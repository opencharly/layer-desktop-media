# desktop-media

Desktop media codecs, thumbnailers, and the ALSA↔PipeWire bridge for a KDE
workstation.

The `desktop-media` candy installs the GStreamer plugin set (`libav`, `bad`,
`ugly`, `va`, `pipewire`) so Plasma, Phonon and Haruna play every codec, plus
the VLC plugin bundle, DVD CSS, the thumbnailer support libs
(`libgsf`/`libopenraw`/`poppler-glib`), and the ALSA↔PipeWire bridge with
`pavucontrol`. It mirrors the CachyOS netinstall "desktop integration" +
"audio" subgroups; host-firmware entries such as `alsa-firmware`/`sof-firmware`
are deliberately excluded, since the streamed VM has no physical audio hardware
and its sinks come from PipeWire. Every package lands a known binary, an
unsonamed plugin `.so`, or a config drop-in, so each `plan:` scenario is a
deterministic file or package check.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `desktop-media` |
| Requires | `layer-pipewire` (`@github.com/opencharly/pod-pipewire`) |
| Distro | `arch` only |
| Binaries | `/usr/bin/pavucontrol`, `/usr/bin/ffmpegthumbnailer`, `/usr/bin/aplay` |
| Plugins | `/usr/lib/gstreamer-1.0/libgstlibav.so`, `/usr/lib/gstreamer-1.0/libgstva.so` |
| Config | `/etc/alsa/conf.d/99-pipewire-default.conf` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-kde-workstation:
  candy:
    base: cachyos
    candy:
      - '@github.com/opencharly/layer-desktop-media:v2026.243.1041'
```

The `pipewire` dependency supplies the media server; `desktop-media` adds the
codecs, thumbnailers, and the ALSA bridge. After the image is built (no physical
audio hardware needed to verify the artifacts):

```bash
ls /usr/lib/gstreamer-1.0/libgstlibav.so
ls /etc/alsa/conf.d/99-pipewire-default.conf
pavucontrol --version
```

## Layout

- `charly.yml` — the `desktop-media:` candy entity: the `require:` dep, the
  `arch` package arm, the `plan:` file/package `check:` steps.
- `CHANGELOG/` — per-CalVer release notes.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-selkies:pipewire` — the PipeWire audio/media server the
  ALSA bridge routes through
- Codecs: `/charly-selkies:ffmpeg` — the transcoder stack used by downstream media
  consumers
- Fonts: `/charly-selkies:desktop-fonts`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
