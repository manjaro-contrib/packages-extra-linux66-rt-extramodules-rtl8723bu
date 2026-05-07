# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Bob Fanger < bfanger(at)gmail >
# Contributor: Filip <fila pruda com>, Det < nimetonmaili(at)gmail >

_linuxprefix=linux66-rt

_module=rtl8723bu
pkgname="${_linuxprefix}-${_module}"
pkgver=20260507
pkgrel=1
pkgdesc="Driver for RTL8723BU"
arch=('x86_64')
url="https://github.com/dini/rtl8723bu"
license=('GPL')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=('git' "${_linuxprefix}-headers")
provides=("${_module}")
_commit=447ea628fce35aebc5cb857a788be042479284ff  # master
source=("git+https://github.com/dini/rtl8723bu.git#commit=${_commit}"
        'blacklist-rtl8xxxu.conf')
sha256sums=('73149a69f2f5c3136b530f325834efc2d6c2bd3d73ad3c8a199d8e8a1753bdcc'
            '7c726ad04083c8e620bc11c837e5f51d3e9e2a5c3e19c333b2968eb39f1ef07e')

pkgver() {
    cd "${_module}"
    git show -s --date=format:'%Y%m%d' --format=%cd
}

prepare() {
    cd "${_module}"

    # do not compile with CONCURRENT_MODE
    sed -i 's/ccflags-y += -DCONFIG_CONCURRENT_MODE/#ccflags-y += -DCONFIG_CONCURRENT_MODE/g' Makefile
}

build() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

     cd "${_module}"

    # avoid using the Makefile directly -- it doesn't understand
    # any kernel but the current.
    make clean
    make -C /usr/lib/modules/${_kernver}/build M="$srcdir/${_module}" modules
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    install -Dm644 "blacklist-rtl8xxxu.conf" "${pkgdir}/usr/lib/modprobe.d/${_linuxprefix}-blacklist-rtl8xxxu.conf"
    install -Dm644 "${_module}"/*.ko -t "$pkgdir/usr/lib/modules/${_kernver}/extramodules/"
    find "${pkgdir}" -name '*.ko' -exec zstd --rm -19 {} +
}
