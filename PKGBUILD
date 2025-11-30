# Maintainer: envolution
# Contributor: Fabio 'Lolix' Loli <fabio.loli@disroot.org> -> https://github.com/FabioLolix
# Contributor: Talebian <talebian@sovietunion.xyz>
# shellcheck shell=bash disable=SC2034,SC2154

pkgname=bottles-git
_pkgname=Bottles
pkgver=60.1.r0.g30699169
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
	python-fvs
	python-gobject
	python-markdown
	python-orjson
	python-pathvalidate
	python-pycurl
	python-requests
	python-steamgriddb
	python-yaml
	webkit2gtk
	xorg-xdpyinfo
	vkbasalt-cli
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
	"git+https://github.com/bottlesdevs/Bottles.git"
	"disable-flatpak-check.patch"
)
sha256sums=(
	"SKIP"
	"48cda3b83ff9f9c8e57996cfb1202fdf492d071c41bc2d223b6bcf8571adb590"
)

pkgver() {
	cd "Bottles"
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
	patch --forward --directory="${srcdir}/${_pkgname}" --strip=1 --input="${srcdir}/disable-flatpak-check.patch"
}

build() {
	cd "${srcdir}/${_pkgname}"
	meson setup --prefix='/usr' build
	ninja -C build
}

#check() {
#disable for now since we know it's failing for appstream issues
#  ninja test -C "Bottles/build" || true
#}

package() {
	cd "Bottles"
	DESTDIR="$pkgdir/" ninja install -C build
}
# vim:set ts=2 sw=2 et:
