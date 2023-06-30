# Maintainer: Alexey Pavlov <alexpux@gmail.com>

pkgname=('apr-util' 'apr-util-devel')
pkgver=1.6.3
pkgrel=2
pkgdesc="The Apache Portable Runtime"
arch=('i686' 'x86_64')
url="https://apr.apache.org/"
makedepends=('apr-devel' 'libexpat-devel' 'libsqlite-devel' 'autotools' 'gcc')
options=('!libtool')
license=('spdx:Apache-2.0')
source=("https://archive.apache.org/dist/apr/apr-util-${pkgver}.tar.bz2"
        plugins.patch
		fix-dll-build.patch)
sha256sums=('a41076e3710746326c3945042994ad9a4fcac0ce0277dd8fea076fec3c9772b5'
            '3280d6ed8e577b626e60d495856d16f6944c3144c495fe9ed8cad6b39332824c'
			'b33b18e612f54ea15c9303aede19e4a2b9ec2550fd081add61d13eff6446d44a')

prepare() {
  cd "${srcdir}/apr-util-${pkgver}"

  patch -p1 -i ${srcdir}/plugins.patch
  patch -p1 -i ${srcdir}/fix-dll-build.patch

  autoreconf -fi
}

build() {
  cd "${srcdir}/apr-util-${pkgver}"

  ./configure \
    --build=${CHOST} \
    --prefix=/usr \
    --with-apr=/usr \
    --with-expat=/usr \
    --without-gdbm \
    --without-ldap \
    --without-pgsql \
    --with-sqlite3=/usr

  make -j1 LDFLAGS="${LDFLAGS} -no-undefined"
  make DESTDIR="${srcdir}/dest" install
}

check() {
  cd "${srcdir}/apr-util-${pkgver}"
  make -j1 check
}

package_apr-util() {
  depends=('apr' 'expat' 'libsqlite')
  groups=('libraries')

  mkdir -p ${pkgdir}/usr/bin
  cp -f ${srcdir}/dest/usr/bin/*.dll ${pkgdir}/usr/bin/
}

package_apr-util-devel() {
  pkgdesc="Libapr-util headers and libraries"
  groups=('development')
  depends=("apr-util=${pkgver}" "apr-devel" "libexpat-devel" "libsqlite-devel")
  options=('staticlibs')

  mkdir -p ${pkgdir}/usr/bin
  cp -f ${srcdir}/dest/usr/bin/*-config ${pkgdir}/usr/bin/
  cp -rf ${srcdir}/dest/usr/include ${pkgdir}/usr/
  cp -rf ${srcdir}/dest/usr/lib ${pkgdir}/usr/
}
