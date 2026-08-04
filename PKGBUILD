# Maintainer: KUHTOXO https://aur.archlinux.org/account/kuhtoxo
# Maintainer: Zoddo <archlinux+aur@zoddo.fr>
# Contributor: void09 <sgmihai at gmail dot com>
# Contributor: taotieren <admin@taotieren.com>
# Contributor: Leon Möller <jkhsjdhjs at totally dot rip>

pkgbase=rustdesk-nightly-bin
pkgname=(rustdesk-nightly-bin)
pkgver=1.4.9
pkgrel=1
pkgdesc="Yet another remote desktop software, written in Rust. Nightly binary build. Works out of the box, no configuration required."
url="https://github.com/rustdesk/rustdesk"
license=('AGPL-3.0-only')
arch=('x86_64' 'aarch64')
provides=('rustdesk')
conflicts=('rustdesk' 'rustdesk-bin')
depends=(
    'gtk3'
    'xdotool'
    'libxcb'
    'libxfixes'
    'alsa-lib'
    'libva'
    'libvdpau'
    'pam'
    'gst-plugins-base'
    'gst-plugin-pipewire'
)
optdepends=(
    'libappindicator-gtk3: tray icon'
    'libayatana-appindicator: tray icon'
)
options=('!strip' '!lto' '!debug')
source_x86_64=("rustdesk-${pkgver}-${pkgrel}-${CARCH}.pkg.tar.zst::${url}/releases/download/nightly/rustdesk-${pkgver}-0-${CARCH}.pkg.tar.zst")
source_aarch64=("rustdesk-${pkgver}-${pkgrel}-aarch64.rpm::${url}/releases/download/nightly/rustdesk-${pkgver}-0.aarch64.rpm")
sha256sums_x86_64=('59cadb206b1a287ed2897b5c6d14296e4e8868f48f671ec3dcb9515ddd975b9c')
sha256sums_aarch64=('82b07beab83ad6bf7c12cbfd581b0375a5156ccf01d696ab53e5624d54b53c12')

install=$pkgname.install

package() {
    install -d "${pkgdir}/usr/share/" "${pkgdir}/usr/bin/"
    cp -r "${srcdir}/usr/share/rustdesk/" "${pkgdir}/usr/share/"
    cp -r "${srcdir}/usr/share/icons/" "${pkgdir}/usr/share/"

    ln -s "/usr/share/rustdesk/rustdesk" "${pkgdir}/usr/bin/rustdesk"

    install -Dm 644 "${srcdir}/usr/share/rustdesk/files/rustdesk.service" "${pkgdir}/usr/lib/systemd/system/rustdesk.service"
    install -Dm 644 "${srcdir}/usr/share/rustdesk/files/rustdesk.desktop" "${pkgdir}/usr/share/applications/rustdesk.desktop"
    install -Dm 644 "${srcdir}/usr/share/rustdesk/files/rustdesk-link.desktop" "${pkgdir}/usr/share/applications/rustdesk-link.desktop"

    # Remove useless files
    rm -r "${pkgdir}/usr/share/rustdesk/files/"
}
