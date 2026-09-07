# Maintainer: envolution
# Contributor: Fabio 'Lolix' Loli <fabio.loli@disroot.org> -> https://github.com/FabioLolix
# Contributor: Talebian <talebian@sovietunion.xyz>
# shellcheck shell=bash disable=SC2034,SC2154

pkgname=bottles-git
_srcname=Bottles
pkgver=67.4.r0.g4a629bef
pkgrel=1
epoch=2
pkgdesc='Easily manage wine and proton prefixes'
arch=(any)
url="https://usebottles.com/"
license=(GPL-3.0-only)
depends=(
	cabextract
	dconf
	gtk4
	gtksourceview5
	hicolor-icon-theme
	icoextract
	imagemagick
	libadwaita
	libportal-gtk4
	p7zip
	patool
	python
	python-chardet
	python-gobject
	python-markdown
	python-orjson
	python-pathvalidate
	python-pycurl
	python-requests
	python-yaml
	python-yara
	xorg-xdpyinfo
	vkbasalt-cli
	fvs2
)
optdepends=(
	gvfs
	lib32-gamemode
	lib32-gnutls
	lib32-vkd3d
	lib32-vulkan-icd-loader
	vkd3d
	vulkan-icd-loader
	wine
	gamemode
	vmtouch
)
makedepends=(
	blueprint-compiler
	meson
	ninja
	git
)
provides=(bottles)
conflicts=(bottles)
source=(
	"${_srcname}::git+https://github.com/bottlesdevs/Bottles.git#branch=main"
	"disable-flatpak-check.patch"
	"native-fixes.patch"
)
sha256sums=('SKIP'
            '98e50e9dbdf2de22f5309efb38b2b831cfc6626c891d2d61e511f4dc721e6a4a'
            '2380f6f50957e10b1c960610cc2fe41522db01d4eb84feb6870b20095acd02b4')

pkgver() {
	cd "${srcdir}/${_srcname}"
	git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

# prepare() {
#   cd "Bottles"
#   [ -d build ] && rm -rf build
#   mkdir build
#   #  for now let's try bypass so the sourcecode can change without breaking our patch
#   sed -i 's/if not fs.is_file.*$/if false/' bottles/frontend/meson.build
#   sed -i '/if not Xdp.Portal.running_under_sandbox()/,/^            return$/s/^/#/' bottles/frontend/window.py
# }

# build() {
#   cd "Bottles"
#   arch-meson build
#   ninja -C build
# }

prepare() {
	# Fix warning about flatpak and sandbox environment
	patch --forward --directory="${srcdir}/${_srcname}" --strip=1 --input="${srcdir}/disable-flatpak-check.patch"
	# Apply native packaging fixes maintained outside upstream
	patch --forward --directory="${srcdir}/${_srcname}" --strip=1 --input="${srcdir}/native-fixes.patch"
}

build() {
	cd "${srcdir}/${_srcname}"
	meson setup --prefix='/usr' build
	ninja -C build
}

#check() {
#disable for now since we know it's failing for appstream issues
#  ninja test -C "Bottles/build" || true
#}

package() {
	cd "${srcdir}/${_srcname}"
	DESTDIR="$pkgdir/" ninja install -C build
}
# vim:set ts=2 sw=2 et:
