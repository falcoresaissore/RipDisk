# DiskBurn 1.0.1 for macOS ARM64

DiskBurn is a native SwiftUI audio CD ripper derived from Asunder 3.0.1. This
clean project contains the macOS port source rather than the unused legacy
C/GTK build tree. The GPL licence and original contributor credits are retained
in `COPYING`, `NOTICE`, and `AUTHORS`.

## Build

```sh
cd ~/Downloads/DiskBurn
./scripts/build-app.sh
```

The application is produced at `dist/DiskBurn.app`. The script compiles an
optimized, arm64-only executable, packages the inherited Asunder artwork as its
macOS application icon, embeds the complete backend and its notices, and
applies an ad-hoc code signature.

Downloaded third-party source archives are required when regenerating licence
notices or preparing a public binary release. The verified release workspace
contains them under `third_party/sources`; they are omitted from Git and
included in the corresponding-source release archive instead.

## Create a DMG

```sh
./scripts/build-dmg.sh
```

The distributable image is produced at `dist/DiskBurn-1.0.1-arm64.dmg`. It
contains `DiskBurn.app`, an Applications shortcut, and a complete ARM64 ripping
and encoding backend. No Homebrew installation or backend download is required
on the destination Mac.

## Moving the application to another Mac

The current bundle can be copied to another Apple Silicon Mac running macOS 27
or later. The application prefers the commands and dynamic libraries embedded
inside its own Resources directory, so a clean destination Mac does not need
Homebrew. The minimum system version reflects the Homebrew bottles available on
the build Mac; producing a package for older macOS releases requires rebuilding
those third-party tools against an older deployment target.

This local build is ad-hoc signed rather than signed with an Apple Developer ID
and notarized. Gatekeeper may therefore require the destination user to
Control-click the app and choose Open. A Developer ID signature and Apple
notarization are required for normal warning-free distribution. Intel Macs are
not supported by this arm64-only build.

## Bundled backend

Ripping and encoding use the same command-line tool model as the original
Asunder. The DMG embeds these commands and their non-system dynamic libraries:

| Feature | Command |
|---|---|
| Secure CD audio extraction | `cd-paranoia` from `libcdio-paranoia` |
| MP3 | `lame` |
| Ogg Vorbis | `oggenc` from `vorbis-tools` |
| FLAC | `flac` |
| Opus | `opusenc` from `opus-tools` |
| WavPack | `wavpack` |
| Musepack | `mpcenc` |
| AAC | `ffmpeg` (AAC encoder) |
| Monkey's Audio | `mac` |

WAV output needs no encoder. The Encode preference panel verifies the embedded
commands. Homebrew is required only on a machine rebuilding DiskBurn from
source; it is not required to run the distributed application. Build-machine
packages are:

```sh
brew install libcdio-paranoia lame vorbis-tools flac opus-tools wavpack musepack mac ffmpeg
```

AAC uses FFmpeg because Homebrew's legacy `fdk-aac-encoder` formula is disabled.
The public macOS audio encoder registry on the build system does not expose an
AAC encoder, so FFmpeg cannot currently be replaced by an Apple system command
without dropping AAC support.

## Licence and GitHub release

DiskBurn application source is GPL-2.0-only, matching Asunder 3.0.1. Bundled
backend programs keep their own licences; this does not relicense them as
DiskBurn. Complete notices are embedded in the application and exact upstream
sources/checksums are documented in `THIRD_PARTY_SOURCES.md`.

Prepare the three GitHub release assets with:

```sh
./scripts/prepare-release.sh
```

Upload the DMG, the matching `corresponding-source.tar.gz`, and
`SHA256SUMS.txt` to the same GitHub release. Do not publish the DMG alone.

## Port design

- SwiftUI main window preserves the original album fields, single-artist
  option, selectable track list, toolbar actions, and bottom-right Rip action.
- A dedicated Fetch Information toolbar action reruns GNUDB lookup without
  rereading the disc and reports connection, server, and no-match errors.
- The About information opens as a non-blocking popover and dismisses when the
  user clicks back on the ripping interface.
- The progress sheet retains separate total, ripping, and encoding indicators.
  It shows Cancel while active and an explicit Close action when the job ends.
- General, Filenames, Encode, and Advanced settings mirror the original tabs.
- System, Light, and Dark appearance modes are available in General settings.
- Native IOKit supplies USB/internal optical-drive discovery and audio TOC data,
  with `drutil` retained as a fallback.
- GnuDB CDDB lookup preserves automatic album and track metadata.
- Advanced settings expose the GNUDB identity used for protocol-compliant
  metadata requests; a unique local identity is generated on first launch.
- GNUDB uses HTTPS on `gnudb.gnudb.org` port `443`; legacy CDDBP port `8880`
  is migrated automatically and is no longer used by the native client.
- `cd-paranoia` performs extraction; individual encoder processes preserve the
  original set of output formats.

## Current validation boundary

The application builds, signs, and launches as an arm64-only bundle. A USB
TSSTcorp DVD+-RW drive and a 14-track audio CD were detected through IOKit.
Track 1 was extracted with `cd-paranoia` and encoded to a valid 44.1 kHz stereo
Ogg Vorbis file, confirming the complete drive-to-encoder path. GNUDB HTTPS
connectivity and the disc query were also validated; the disc returned two
matches for The Killers / Hot Fuss. Every bundled encoder has also completed a
clean-environment synthetic WAV conversion test, and every Mach-O dependency
is checked to ensure that no `/opt/homebrew` load path remains.
