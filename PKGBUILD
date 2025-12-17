# Maintainer: KokaKiwi <kokakiwi+aur@kokakiwi.net>
# Contributor: George Rawlinson <george@rawlinson.net.nz>

_name=ImHex
pkgname=${_name,,}
pkgver=1.38.0
pkgrel=1
pkgdesc='A Hex Editor for Reverse Engineers, Programmers and people that value their eye sight when working at 3 AM'
url='https://imhex.werwolv.net'
license=('GPL-2.0-or-later')
arch=('x86_64')
depends=('glfw' 'mbedtls' 'curl' 'dbus'
         'freetype2' 'file' 'hicolor-icon-theme' 'xdg-desktop-portal' 'fontconfig'
         'fmt' 'yara' 'capstone')
makedepends=('git' 'cmake'
             'llvm' 'librsvg' 'nlohmann-json' 'libxrandr'
             'python' 'cli11' 'dotnet-runtime' 'gcc')
optdepends=('dotnet-runtime: support for .NET scripts')
provides=('imhex-patterns')
conflicts=('imhex-patterns-git')
source=("$pkgname-$pkgver.tar.gz::https://github.com/WerWolv/ImHex/releases/download/v$pkgver/Full.Sources.tar.gz"
        "imhex-patterns-$pkgver.tar.gz::https://github.com/WerWolv/ImHex-Patterns/archive/refs/tags/ImHex-v$pkgver.tar.gz")
sha256sums=('dc4bb8e6a3800d59da284b92f56ca9816a32790aa11121a9a143b1a078c5d8d6'
            'eb211d696f409f3747942d6f7139e56dae76c3c073e1fee0471a3e7ed82b5f28')
options=(!lto)

build() {
  cd "${srcdir}/${_name}"

  cmake -B build -S "." \
    -Wno-dev \
    -Wno-deprecated \
    --compile-no-warning-as-error \
    -D CMAKE_BUILD_TYPE=Release \
    -D CMAKE_INSTALL_PREFIX=/usr \
    -D CMAKE_SKIP_RPATH=ON \
    -D IMHEX_OFFLINE_BUILD=ON \
    -D IMHEX_IGNORE_BAD_CLONE=ON \
    -D IMHEX_STRIP_RELEASE=OFF \
    -D IMHEX_STRICT_WARNINGS=OFF \
    -D IMHEX_BUNDLE_DOTNET=OFF \
    -D USE_SYSTEM_LLVM=ON \
    -D USE_SYSTEM_YARA=ON \
    -D USE_SYSTEM_FMT=ON \
    -D USE_SYSTEM_NLOHMANN_JSON=ON \
    -D USE_SYSTEM_CAPSTONE=ON \
    -D USE_SYSTEM_CLI11=ON \
    -D IMHEX_VERSION="$pkgver" \
    -D CMAKE_COMPILE_WARNING_AS_ERROR=OFF

  cmake --build build
}

package() {
  test -e "$srcdir/ImHex-Patterns" && rm "$srcdir/ImHex-Patterns"
  ln -s "$srcdir/ImHex-Patterns-ImHex-v$pkgver" "$srcdir/ImHex-Patterns"
  
  cd "${srcdir}/${_name}"

  DESTDIR="$pkgdir" cmake --install build

  # Remove updater
  rm "$pkgdir/usr/bin/imhex-updater"

  # Patterns
  install -dm0755 "$pkgdir/usr/share/imhex"
  cp -r -t "$pkgdir/usr/share/imhex" \
    "$srcdir/ImHex-Patterns-ImHex-v$pkgver"/{constants,encodings,includes,magic,nodes,patterns,plugins,scripts,tests,themes,tips,yara}

  # Desktop file(s)
  install -Dm0644 "resources/icon.svg" "$pkgdir/usr/share/pixmaps/imhex.svg"

  # Documentation
  install -Dm0644 -t "$pkgdir/usr/share/doc/$pkgname" \
    "README.md"
}
