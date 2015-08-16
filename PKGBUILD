pkgname=mingw-w64-gtkmm
pkgver=2.24.4
pkgrel=1
pkgdesc="C++ bindings for gtk2 (mingw-w64)"
arch=(any)
url="http://www.gtkmm.org"
license=("LGPL")
makedepends=(mingw-w64-gcc mingw-w64-pkg-config)
depends=(mingw-w64-crt mingw-w64-atkmm mingw-w64-pangomm mingw-w64-gtk2)
options=(staticlibs !strip !buildflags)
source=("http://ftp.gnome.org/pub/GNOME/sources/gtkmm/${pkgver%.*}/gtkmm-$pkgver.tar.xz")
md5sums=('b9ac60c90959a71095f07f84dd39961d')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

build() {
  for _arch in ${_architectures}; do
    export CFLAGS="-O2 -pipe"
    export CXXFLAGS="$CFLAGS"
    export CPPFLAGS="$CPPFLAGS -D_REENTRANT"
    unset LDFLAGS
    mkdir -p "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    "${srcdir}"/${pkgname#mingw-w64-}-${pkgver}/configure \
      --prefix=/usr/${_arch} \
      --build=$CHOST \
      --host=${_arch} \
      --enable-shared \
      --enable-static \
      --disable-documentation
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${pkgname}-${pkgver}-build-${_arch}"
    make DESTDIR="$pkgdir" install
    find "$pkgdir/usr/${_arch}" -name '*.exe' -o -name '*.bat' -o -name '*.def' -o -name '*.exp' | xargs -rtl1 rm
    find "$pkgdir/usr/${_arch}" -name '*.dll' | xargs -rtl1 ${_arch}-strip --strip-unneeded
    find "$pkgdir/usr/${_arch}" -name '*.a' -o -name '*.dll' | xargs -rtl1 ${_arch}-strip -g
  done
}