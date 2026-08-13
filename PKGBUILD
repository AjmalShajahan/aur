# Maintainer: envolution
# Contributor: Fabio 'Lolix' Loli <fabio.loli@disroot.org> -> https://github.com/FabioLolix
# Contributor: Talebian <talebian@sovietunion.xyz>
# shellcheck shell=bash disable=SC2034,SC2154

pkgname=bottles-git
_srcname=Bottles
pkgver=66.2.r0.g9f44ee77
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
            'e0b9ccd2ba012e733ce7517337d4b651cbdf0415a7ef3bae0caf717879b9d0f7'
            'a5d9948b303e51c045ef8ecab4ed89119e9a6b8353da690d41419acea0eb5311')

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
