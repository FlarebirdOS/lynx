pkgname=lynx
pkgver=2.9.2
pkgrel=2
pkgdesc="A text browser for the World Wide Web"
arch=('x86_64')
url="https://lynx.invisible-island.net"
license=('GPL-2.0-only')
depends=(
    'brotli'
    'bzip2'
    'glibc'
    'libidn2'
    'ncurses'
    'openssl'
    'zlib'
)
backup=(etc/lynx.cfg)
options=('!lto')
source=(https://invisible-mirror.net/archives/lynx/tarballs/${pkgname}${pkgver}.tar.bz2)
sha256sums=(7374b89936d991669e101f4e97f2c9592036e1e8cdaa7bafc259a77ab6fb07ce)

build() {
    cd ${pkgname}${pkgver}

    local configure_args=(
        --sysconfdir=/etc/lynx
        --with-zlib
        --with-bzlib
        --with-ssl
        --with-screen=ncursesw
        --enable-locale-charset
        --datadir=/usr/share/doc/${pkgname}-${pkgver}
        --enable-ipv6
        --enable-nls
        --enable-externs
        --enable-default-colors
        ${configure_options}
    )

    ./configure "${configure_args[@]}"

    make
}

package() {
    cd ${pkgname}${pkgver}

    make DESTDIR=${pkgdir} install-full

    chgrp -v -R root ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}/lynx_doc

    sed -e '/#LOCALE/     a LOCALE_CHARSET:TRUE'     \
        -e '/#DEFAULT_ED/ a DEFAULT_EDITOR:vi'       \
        -e '/#PERSIST/    a PERSISTENT_COOKIES:TRUE' \
        -i ${pkgdir}/etc/lynx/lynx.cfg
}
