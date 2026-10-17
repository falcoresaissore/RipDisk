# Third-party source and build provenance

DiskBurn's DMG aggregates unmodified command-line programs and libraries built
from Homebrew bottles. They are separate programs from DiskBurn and keep their
upstream licences. The exact corresponding source archives are shipped in the
matching `DiskBurn-1.0.1-corresponding-source.tar.gz` GitHub release asset.
`third_party/SHA256SUMS` verifies every archive. The exact Homebrew formula
recipes used for provenance are stored in `third_party/homebrew-formulae/`.

| Component | Version | Licence | Upstream source |
|---|---:|---|---|
| libcdio-paranoia | 10.2+2.0.2 | GPL-3.0-only | <https://github.com/libcdio/libcdio-paranoia/releases/download/release-10.2%2B2.0.2/libcdio-paranoia-10.2+2.0.2.tar.gz> |
| libcdio | 2.4.0 | GPL-3.0-or-later | <https://github.com/libcdio/libcdio/releases/download/2.4.0/libcdio-2.4.0.tar.gz> |
| LAME | 4.0 | LGPL-2.0-or-later | <https://downloads.sourceforge.net/project/lame/lame/4.0/lame-4.0.tar.gz> |
| mpg123 | 1.33.7 | LGPL-2.1-only | <https://www.mpg123.de/download/mpg123-1.33.7.tar.bz2> |
| Vorbis Tools | 1.4.3 | LGPL-2.0-or-later AND GPL-2.0-or-later AND GPL-2.0-only | <https://ftp.osuosl.org/pub/xiph/releases/vorbis/vorbis-tools-1.4.3.tar.gz> |
| libvorbis | 1.3.7 | BSD-3-Clause | <https://ftp.osuosl.org/pub/xiph/releases/vorbis/libvorbis-1.3.7.tar.xz> |
| libogg | 1.3.6 | BSD-3-Clause | <https://ftp.osuosl.org/pub/xiph/releases/ogg/libogg-1.3.6.tar.gz> |
| FLAC | 1.5.0 | BSD/GPL/ISC/LGPL | <https://ftp.osuosl.org/pub/xiph/releases/flac/flac-1.5.0.tar.xz> |
| Opus Tools | 0.2 | BSD-2-Clause | <https://ftp.osuosl.org/pub/xiph/releases/opus/opus-tools-0.2.tar.gz> |
| libopusenc | 0.3 | BSD-3-Clause | <https://ftp.osuosl.org/pub/xiph/releases/opus/libopusenc-0.3.tar.gz> |
| Opus | 1.6.1 | BSD-3-Clause | <https://ftp.osuosl.org/pub/xiph/releases/opus/opus-1.6.1.tar.gz> |
| WavPack | 5.9.0 | BSD-3-Clause | <https://www.wavpack.com/wavpack-5.9.0.tar.bz2> |
| Musepack | r475 | BSD/GPL/LGPL/Zlib | <https://files.musepack.net/source/musepack_src_r475.tar.gz> |
| libcuefile | r475 | GPL-2.0-or-later | <https://files.musepack.net/source/libcuefile_r475.tar.gz> |
| libreplaygain | r475 | LGPL-2.1-or-later | <https://files.musepack.net/source/libreplaygain_r475.tar.gz> |
| Monkey's Audio | 13.29 | BSD-3-Clause | <https://monkeysaudio.com/files/MAC_1329_SDK.zip> |
| FFmpeg | 9.0.2 | GPL-3.0-or-later in this Homebrew build | <https://ffmpeg.org/releases/ffmpeg-9.0.2.tar.xz> |
| SVT-AV1 | 4.2.0 | BSD-3-Clause | <https://gitlab.com/AOMediaCodec/SVT-AV1/-/archive/v4.2.0/SVT-AV1-v4.2.0.tar.bz2> |
| OpenSSL | 4.0.3 | Apache-2.0 | <https://github.com/openssl/openssl/releases/download/openssl-4.0.3/openssl-4.0.3.tar.gz> |
| dav1d | 1.5.4 | BSD-2-Clause | <https://code.videolan.org/videolan/dav1d/-/archive/1.5.4/dav1d-1.5.4.tar.bz2> |
| XZ Utils | 5.8.4 | 0BSD AND GPL-2.0-or-later | <https://github.com/tukaani-project/xz/releases/download/v5.8.4/xz-5.8.4.tar.gz> |
| libvmaf | 3.2.1 | BSD-2-Clause-Patent | <https://github.com/Netflix/vmaf/archive/refs/tags/v3.2.1.tar.gz> |
| libvpx | 1.17.0 | BSD-3-Clause | <https://github.com/webmproject/libvpx/archive/refs/tags/v1.17.0.tar.gz> |
| x264 | revision b35605ace3dd | GPL-2.0-or-later | <https://code.videolan.org/videolan/x264/-/archive/b35605ace3ddf7c1a5d67a2eb553f034aef41d55/x264-b35605ace3ddf7c1a5d67a2eb553f034aef41d55.tar.gz> |
| x265 | 4.3 | GPL-2.0-or-later | <https://github.com/Multicorewareinc/x265/releases/download/4.3/x265_4.3.tar.gz> |

## Reproducing the packaged backend

Install the formulae listed in the README, then run `scripts/build-app.sh`.
`scripts/bundle-tools.sh` copies the requested executable files and follows
their Mach-O load commands recursively, copies every non-system dynamic
library, rewrites load paths to `@loader_path`, and ad-hoc signs each binary.
The saved formulae document the upstream URL, checksum, dependency graph, and
configure arguments, including the GPL/version-3 options used by FFmpeg.

The downloaded archives are intentionally ignored by Git so the repository
does not permanently duplicate upstream projects. They are included in the
corresponding-source release asset produced by `scripts/prepare-release.sh`.

