# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Bob Fanger < bfanger(at)gmail >
# Contributor: Filip <fila pruda com>, Det < nimetonmaili(at)gmail >

_linuxprefix=linux66-rt

_module=rtl8723bu
pkgname="${_linuxprefix}-${_module}"
pkgver=20250607
pkgrel=1
pkgdesc="Driver for RTL8723BU"
arch=('x86_64')
url="https://github.com/lwfinger/rtl8723bu"
license=('GPL')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}")
makedepends=('git' "${_linuxprefix}-headers")
provides=("${_module}")
_commit=8e13c4675c91f67404cdb18488c864ec7b54a985  # master
source=("git+https://github.com/lwfinger/rtl8723bu.git#commit=${_commit}"
        'blacklist-rtl8xxxu.conf')
sha256sums=('b1bdcede5d15c75366107129c2daae8d699afbe28042f228b19f5f081c8aeca2'
            '7c726ad04083c8e620bc11c837e5f51d3e9e2a5c3e19c333b2968eb39f1ef07e')

pkgver() {
    cd "${_module}"
    git show -s --date=format:'%Y%m%d' --format=%cd
}

prepare() {
    cd "${_module}"

    # do not compile with CONCURRENT_MODE
    sed -i 's/EXTRA_CFLAGS += -DCONFIG_CONCURRENT_MODE/#EXTRA_CFLAGS += -DCONFIG_CONCURRENT_MODE/g' Makefile
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
