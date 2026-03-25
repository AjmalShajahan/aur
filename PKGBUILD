# Maintainer: nopw <aur@n0.pw>

pkgname=stremio-linux-shell-git
pkgver=v1.0.0.beta.13.r39.g7431320
pkgrel=1
pkgdesc="A native Linux client for Stremio (Personal Fork)"
arch=('x86_64')
url="https://github.com/AjmalShajahan/stremio-linux-shell"
license=('GPL-3.0-only')

depends=(
  'gtk4'
  'libadwaita'
  'mpv'
  'libepoxy'
  'openssl'
  'gettext'
  'hicolor-icon-theme'
)
makedepends=(
  'cargo'
  'git'
  'pkgconf'
  'cmake'
  'binutils'
  'licenses'
  'nodejs'
  'gtk4'
  'libadwaita'
  'mpv'
  'libepoxy'
  'gettext'
)

provides=('stremio-linux-shell' 'stremio')
conflicts=('stremio' 'stremio-linux-shell')
options=(!lto)
source=("git+https://github.com/AjmalShajahan/stremio-linux-shell.git#branch=refactor/gtk4")
sha256sums=('SKIP')

pkgver() {
  cd "stremio-linux-shell"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "stremio-linux-shell"
  git submodule update --init --recursive
  cargo fetch --locked
}

build() {
  cd "stremio-linux-shell"
  cargo build --release --locked
}

package() {
  cd "stremio-linux-shell"

  # TODO: Check if this is the Arch way to do it
  # Install the actual binary to libexec
  install -Dm755 "target/release/stremio-linux-shell" "$pkgdir/usr/libexec/stremio-linux-shell"

  # Create a wrapper script to set CEF_PATH and LD_LIBRARY_PATH
  install -Dm755 /dev/stdin "$pkgdir/usr/bin/stremio" <<'EOF'
#!/bin/sh
export SERVER_PATH="$HOME/.local/share/stremio/server.js"
export CEF_PATH="$HOME/.local/share/cef"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:$CEF_PATH"
export GDK_BACKEND="x11"
exec /usr/libexec/stremio-linux-shell "$@"
EOF

  install -Dm644 "data/com.stremio.Stremio.desktop" \
    "$pkgdir/usr/share/applications/com.stremio.Stremio.desktop"
  sed -i '/^[[:space:]]*DBusActivatable[[:space:]]*=[[:space:]]*true[[:space:]]*$/d' \
    "$pkgdir/usr/share/applications/com.stremio.Stremio.desktop"
  install -Dm644 "data/icons/com.stremio.Stremio.svg" \
    "$pkgdir/usr/share/icons/hicolor/scalable/apps/com.stremio.Stremio.svg"
  install -Dm644 "data/com.stremio.Stremio.metainfo.xml" \
    "$pkgdir/usr/share/metainfo/com.stremio.Stremio.metainfo.xml"

  install -Dm644 /usr/share/licenses/spdx/GPL-3.0-only.txt \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.txt"

	install -Dm644 data/server.js -t \
		"$HOME/.local/share/stremio/"
}
