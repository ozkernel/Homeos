# HomeOS third-party sources

This page covers the HomeOS v2.0.0 Linux amd64 production runtime with Debian 13.7. Obtain the corresponding third-party source from the same distribution as your Docker image.

| Runtime source archive | Value |
| --- | --- |
| Filename | `HomeOS-third-party-source-linux-amd64.tar` |
| v2.0.0 release asset URL (upload before Docker image publication) | `https://github.com/ozkernel/homeos/releases/download/v2.0.0/HomeOS-third-party-source-linux-amd64.tar` |
| Size | 1,088,655,360 bytes |
| SHA-256 | `9b1d8356c56de55409584584e50e26f22c468faa42768e626e3f341baf1709c9` |
| Payload | 205 exact Debian source packages, Node.js v24.22.3, 668 upstream source files and 675 total archive files |

`SHA256SUMS` records the SHA-256 digest of every archive file except itself.

The archive includes the original Debian source tarballs, patches and build/install rules, the official Node.js source, the Docker Node entrypoint and its MIT licence, a binary-to-source version map, and the exact Debian FFmpeg build flags. It contains third-party runtime sources and support records only.

## Runtime terms

- Debian FFmpeg `7:7.1.5-0+deb13u1` is GPL-2.0-or-later. The build enables GPL components, libx264, libx265, libmp3lame and shared libraries. The archive includes the matching FFmpeg source together with the covered libraries and the other Debian GPL, LGPL, MPL and applicable documentation sources. Debian supplied the installed binaries and patches. The HomeOS build applies no changes to those works. [FFmpeg licensing](https://ffmpeg.org/legal.html) · [Debian FFmpeg source](https://packages.debian.org/source/trixie/ffmpeg).
- HomeOS runs FFmpeg as a distinct executable using command-line arguments and standard streams. HomeOS’s proprietary application code remains separately licensed. Preserve recipients’ applicable rights to modify, replace, debug and redistribute third-party code, including the LGPL permissions needed to debug a modified library. [GNU GPL FAQ on program interfaces](https://www.gnu.org/licenses/gpl-faq.en.html).
- Node.js `v24.22.3` carries its MIT grant and bundled notices in `/usr/local/LICENSE`. The source archive includes the exact official Node.js source and its build instructions. [Node.js release](https://nodejs.org/en/blog/release/v24.22.3).

## Publish a matching image and source download

1. Build the `production` target. It copies the merged runtime into one fresh filesystem payload layer and preserves the working directory with an empty `WORKDIR` layer. The image includes HomeOS’s licence, third-party notices and this source information under `/usr/share/doc/homeos/`. Browser notices are available at `/assets/homeos-third-party-notices.md`. Retain the original notices in `/usr/local/LICENSE`, `/usr/share/doc` and `/usr/share/common-licenses`.
2. For each public image, record the platform and registry digest in its release listing. Compare `dpkg-query -W`, the corresponding source version fields, `node -v` and `ffmpeg -buildconf` against the archive records. Review the real layers with `docker image save --platform linux/amd64`. Verify the deleted toolchains and their earlier layers are absent. Refresh the notices and archive whenever the runtime versions, platform or build change.
3. When a new matching archive is required, use `node scripts/collect-third-party-sources.mjs THIRD_PARTY_SOURCES.md LOCAL_SOURCE_DIRECTORY`. The collector fetches the exact Debian descriptors, checks every declared file against SHA-256 and verifies the official Node archive hash. Use it with a reviewed manifest for the target image. Add the binary-to-source map, FFmpeg configuration, upstream licence texts and build notes. On Debian, `dpkg-source -x package_version.dsc` extracts a package together with its build and patch instructions. Independently authenticate origin with Debian’s signed source indexes or maintainer signatures and the [Node.js release keys](https://github.com/nodejs/node#release-keys) when needed. [Debian Snapshot](https://snapshot.debian.org/).
4. Publish the archive next to the corresponding image download at no extra charge, with a clear link in the registry listing or release page. Test both downloads as a recipient and keep the matching source available while the corresponding binaries are offered. Keep proprietary application files and user data outside the source archive. Use the checksum above to verify the archive for the documented runtime. [GPL version 2 §3](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) · [GPL version 3 §6](https://www.gnu.org/licenses/gpl-3.0.html) · [LGPL version 2.1](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html) · [MPL version 2.0 §3.2](https://www.mozilla.org/en-US/MPL/2.0/).

## Exact source manifest

The Debian rows are source package name and source version, including Debian epochs and revisions. All corresponding binary names and versions are in [Third-party notices](THIRD_PARTY_NOTICES.md).

Node-source: v24.22.3 48ca3aa8a9b02133356922ef4fc80200e00598c80a4ef446f7c39a459222593d

```homeos-debian-source
acl 2.3.2-2
adduser 3.152
alsa-lib 1.2.14-1+deb13u1
apt 3.0.4
attr 1:2.5.2-3
audit 1:4.0.2-2+deb13u1
base-files 13.8+deb13u7
base-passwd 3.6.7
bash 5.2.37-2
bzip2 1.0.8-6
cairo 1.18.4-1
cdebconf 0.280
chromaprint 1.5.1-7
codec2 1.2.0-3
coreutils 9.7-3
dash 0.5.12-12
db5.3 5.3.28+dfsg2-9
dbus 1.16.2-2
debian-archive-keyring 2025.1
debianutils 5.23.2
diffutils 1:3.10-4
dpkg 1.22.22
e2fsprogs 1.47.2-3
elfutils 0.192-4
ffmpeg 7:7.1.5-0+deb13u1
fftw3 3.3.10-2
findutils 4.10.0-3
flac 1.5.0+ds-2
flite 2.2-7
fonts-dejavu 2.37-8
freetype 2.13.3+dfsg-1+deb13u1
fribidi 1.0.16-1
game-music-emu 0.6.3-7
gcc-14 14.2.0-19
gdk-pixbuf 2.42.12+dfsg-4+deb13u1
glib2.0 2.84.4-3~deb13u5
glibc 2.41-12+deb13u4
gmp 2:6.3.0+dfsg-3
gnutls28 3.8.9-3+deb13u4
graphite2 1.3.14-2+deb13u1
grep 3.11-4
gzip 1.13-1+deb13u1
harfbuzz 10.2.0-1+deb13u1
hostname 3.25
init-system-helpers 1.69~deb13u1
jackd2 1.9.22~dfsg-4
jbigkit 2.1-6.1
keyutils 1.6.3-6
krb5 1.21.3-5+deb13u1
lame 3.100-6
lcms2 2.16-2+deb13u2
libass 1:0.17.3-1+deb13u1
libasyncns 0.8-6
libavc1394 0.5.4-5
libbluray 1:1.3.4-1
libbs2b 3.1.0+dfsg-8
libcaca 0.99.beta20-5+deb13u1
libcap-ng 0.8.5-4
libcap2 1:2.75-10+deb13u1
libcdio 2.2.0-4.1~deb13u1
libcdio-paranoia 10.2+2.0.2-1
libdatrie 0.2.13-3
libdc1394 2.2.6-5
libdecor-0 0.2.2-2
libdvdnav 6.1.1-3
libdvdread 6.1.3-2
libffi 3.4.8-2
libglvnd 1.7.0-1
libidn2 2.3.8-2
libiec61883 1.2.0-7
libmysofa 1.3.3+dfsg-1
libopenmpt 0.7.13-1
libpciaccess 0.17-3
libpgm 5.3.128~dfsg-2.1
libplacebo 7.349.0-3
libpng1.6 1.6.48-1+deb13u6
libraw1394 2.1.2-2
librsvg 2.60.0+dfsg-1+deb13u1
libsamplerate 0.2.2-4
libsdl2 2.32.4+dfsg-1
libseccomp 2.6.0-2
libselinux 3.8.1-1
libsemanage 3.8.1-1
libsepol 3.8.1-1
libsndfile 1.2.2-2+deb13u1
libsodium 1.0.18-1+deb13u1
libsoxr 0.1.3-4
libssh 0.11.5-0+deb13u1
libtasn1-6 4.20.0-2+deb13u1
libthai 0.1.29-2
libtheora 1.2.0~alpha1+dfsg-6
libudfread 1.1.2-1
libunistring 1.3-2
libusb-1.0 2:1.0.28-1
libva 2.22.0-3
libvidstab 1.1.0-2
libxcrypt 1:4.4.38-1
libzstd 1.5.7+dfsg-1
lm-sensors 1:3.6.2-2
lz4 1.10.0-4
mawk 1.3.4.20250131-1
mbedtls 3.6.6-0.1~deb13u1
mesa 25.0.7-2+deb13u1
mpg123 1.32.10-1+deb13u1
nettle 3.10.1-1
numactl 2.0.19-1
openal-soft 1:1.24.2-1
openssl 3.5.7-1~deb13u3
p11-kit 0.25.5-3
pam 1.7.0-5
pango1.0 1.56.3-1
perl 5.40.1-6+deb13u1
pocketsphinx 0.8+5prealpha+1-15
pulseaudio 17.0+dfsg1-2
rubberband 3.3.0+dfsg-2
rust-sequoia-sqv 1.3.0-3
sed 4.9-2+deb13u1
shadow 1:4.17.4-2
shared-mime-info 2.4-5
shine 3.1.1-2
slang2 2.3.3-5
speex 1.2.1-3
sphinxbase 0.8+5prealpha+1-21
sqlite3 3.46.1-7+deb13u2
srt 1.5.4-1+deb13u1
svt-av1 2.3.0+dfsg-1
systemd 257.13-1~deb13u1
sysvinit 3.14-4
tar 1.35+dfsg-3.1
twolame 0.4.0-2
util-linux 2.41.5-0+deb13u1
x264 2:0.164.3108+git31e19f9-2
x265 4.1-2
xorg 1:7.7+24+deb13u1
xvidcore 2:1.3.7-1
xxhash 0.8.3-2
xz-utils 5.8.1-1+deb13u2
z3 4.13.3-1
zeromq3 4.3.5-1
zimg 3.0.5+ds1-1
zvbi 0.2.44-1
aom 3.12.1-1+deb13u1
brotli 1.1.0-2+deb13u1
cjson 1.7.18-3.1+deb13u1
dav1d 1.5.1-1
debconf 1.5.91
expat 2.9.0-1~deb13u1
fontconfig 2.15.0-2.3
highway 1.2.0-2
jpeg-xl 0.11.2-0.1~deb13u3
lapack 3.12.1-6
lerc 4.0.0+ds-5
libbsd 0.12.2-2
libdeflate 1.23-2
libdrm 2.4.124-2
libedit 3.1-20250104-1
libgsm 1.0.22-1
libjpeg-turbo 1:2.1.5-4
libmd 1.1.0-2
libogg 1.3.5-3
librabbitmq 0.15.0-1+deb13u2
librist 0.2.11+dfsg-1
libunibreak 6.1-3
libvdpau 1.5-3
libvorbis 1.3.7-3
libvpl 1:2.14.0-1
libvpx 1.15.0-2.1+deb13u1
libwebp 1.5.0-0.1
libx11 2:1.8.12-1
libxau 1:1.0.11-1
libxcb 1.17.0-2
libxcursor 1:1.2.3-1
libxdmcp 1:1.1.5-1
libxext 2:1.3.4-1
libxfixes 1:6.0.0-2
libxi 2:1.8.2-1
libxkbcommon 1.7.0-2
libxml2 2.12.7+dfsg+really2.9.14-2.1+deb13u3
libxrandr 2:1.5.4-1
libxrender 1:0.9.12-1
libxshmfence 1.3.3-1
libxss 1:1.2.3-1
libxv 2:1.0.11-1.1
libxxf86vm 1:1.1.4-1
lilv 0.24.26-1
llvm-toolchain-19 1:19.1.7-3
ncurses 6.5+20250216-2
norm 1.5.9+dfsg-3.1
ocl-icd 2.3.3-1
openjpeg2 2.5.3-2.1~deb13u2
opus 1.5.2-2
pcre2 10.46-1~deb13u3
pixman 0.44.0-3
rust-rav1e 0.7.1-9
serd 0.32.4-1
snappy 1.2.2-1
sord 0.16.18-1
sratom 0.6.18-1
tiff 4.7.0-3+deb13u3
tzdata 2026f-0+deb13u1
vulkan-loader 1.4.309.0-1
wayland 1.23.1-3
xkeyboard-config 2.42-1
zix 0.6.2-1
zlib 1:1.3.dfsg+really1.3.1-1+deb13u1
```
