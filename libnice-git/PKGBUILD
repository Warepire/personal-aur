# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: William Díaz <wdiaz@archlinux.us>

pkgname=libnice-git
pkgver=0.1.14+117+g5496500
pkgrel=1
pkgdesc="An implementation of the IETF's draft ICE (for p2p UDP data streams)"
url="https://nice.freedesktop.org"
arch=(x86_64)
license=(LGPLv2.1)
depends=(glib2 gnutls)
makedepends=(gstreamer gtk-doc git)
optdepends=(gstreamer)
provides=(libnice)
conflicts=(libnice)
source=('git+https://gitlab.freedesktop.org/libnice/libnice.git')
sha256sums=('SKIP')

pkgver() {
  cd libnice
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd libnice
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd libnice
  ./configure --prefix=/usr --disable-static --with-gstreamer-0.10=no --enable-gtk-doc --enable-introspection=no
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool
  make
}

package() {
  cd libnice
  make DESTDIR="$pkgdir" install
}
