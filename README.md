# mpv player

Command line media player packaged for Linyaps.

## Description

This is the [mpv media player](https://mpv.io/) packaged as a Linyaps application. mpv is a free, open source, and cross-platform media player with support for a wide variety of media file formats, audio and video codecs, and subtitle types.

## Package Information

- **Package ID**: `io.mpv`
- **Version**: 0.41.0.0
- **Source**: [mpv-player/mpv](https://github.com/mpv-player/mpv) (v0.41.0)
- **Base**: `org.deepin.base/25.2.2`

## Features

- Supports almost all video and audio formats
- Hardware video decoding (VAAPI, VDPAU, Vulkan)
- High quality video rendering with libplacebo
- Lua scripting support
- Wayland and X11 display server support
- Network streaming support
- Advanced subtitle rendering

## Building

### Prerequisites

Ensure you have the LingLong build tools installed. Then:

```bash
# Clone this repository
git clone <repository-url>
cd io.mpv

# Build the package
ll-builder build
```

### Build Dependencies

The package requires the following build dependencies (automatically resolved by the build system):

- build-essential, meson, ninja-build, pkgconf
- FFmpeg libraries (libavcodec-dev, libavformat-dev, etc.)
- Video output (libva-dev, libvulkan-dev, libegl-dev, etc.)
- Audio output (libasound2-dev, libpulse-dev, libpipewire-0.3-dev)
- And many more (see `linglong.yaml` for the complete list)

## Installation

```bash
ll-cli install io.mpv
```

## Usage

```bash
mpv [options] [file]
```

### Common Options

| Option | Description |
| -------- | ------------- |
| `--fs` | Fullscreen mode |
| `--no-audio` | Disable audio |
| `--volume=<value>` | Set volume (0-100) |
| `--loop=<count>` | Loop playback |
| `--sub=<file>` | Load subtitle file |

### Examples

```bash
# Play a video file
mpv video.mp4

# Play with subtitles
mpv --sub=subtitle.srt movie.mkv

# Play URL stream
mpv https://example.com/stream.m3u8

# Play audio only
mpv --no-video music.flac
```

## Known Issues

### Wayland + VAAPI Hardware Acceleration Crash

The base image (`org.deepin.base/25.2.2`) includes libva version 1.20, which has compatibility issues on some Wayland compositors:

1. libva 1.20 uses the `wl_drm` protocol, which not all Wayland compositors implement
2. libva itself has some implementation issues with this protocol

**Symptom**: Enabling hardware video decoding via VAAPI on Wayland will cause a coredump (segfault).

**Workarounds**:

1. **Disable hardware acceleration** (use software decoding):

   ```bash
   mpv --hwdec=no video.mp4
   ```

2. **Unset WAYLAND_DISPLAY** (run under XWayland or raw X11):

   ```bash
   unset WAYLAND_DISPLAY
   mpv --hwdec=vaapi video.mp4
   ```

## License

mpv is licensed under GPLv2+. See the [upstream repository](https://github.com/mpv-player/mpv) for details.
