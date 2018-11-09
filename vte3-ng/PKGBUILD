# $Id$
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=vte3-ng-with-nN-cursor-move-patch
pkgver=0.54.2
pkgrel=1
pkgdesc="Virtual Terminal Emulator widget for use with GTK3"
url="https://wiki.gnome.org/Apps/Terminal/VTE"
arch=(x86_64)
license=(LGPL)
options=(!emptydirs)
depends=(gtk3 pcre2 gnutls vte-common)
makedepends=(intltool gobject-introspection vala glade git gtk-doc gperf)
conflicts=(vte3)
provides=(vte3)
_commit=6c37e5164aef8b2ac3dea02f6c1a3152dad93fda  # tags/0.54.2
source=("git+https://gitlab.gnome.org/GNOME/vte.git/#commit=$_commit"
        "0001-expose-functions-for-pausing-unpausing-output.patch"
        "0002-expose-function-for-setting-cursor-position.patch"
        "0003-add-function-for-setting-the-text-selections.patch"
        "0004-add-functions-to-getset-block-selection-mode.patch"
        "0005-expose-function-for-getting-the-selected-text.patch"
        "0006-Add-function-to-get-selection-position.patch")
sha256sums=('SKIP'
            '51af803b893c8269a943ff520d934488073c1d28d4bf6693d386d790ec84541a'
            '604f0594c2ed9fef97483a721f8e8abf067a6a2a2fc1fd492d012e24861e0d75'
            '3f2486b08e4452421d8de58826b1cf99925514af2b95abaf1b34d081bc3eb1d8'
            'e74d000a729eabf31f4aabd08433790a7700fcccb6f302916992fb750f3d28ea'
            'a6d90ac8d391d6906dbed22041c597176366501ff625a1b222f1fcf9f83e78f4'
            'fe60f2db4c2afb21da509a109cbdee7a83d7c2cf63839fb8a39f5263bd65dece')

pkgver() {
  cd vte
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd vte

  # https://bugzilla.gnome.org/show_bug.cgi?id=778926
  git cherry-pick -n 809e79770b4dea34d64574710ce429a86855fdb2

  patch -p1 -i ${srcdir}/0001-expose-functions-for-pausing-unpausing-output.patch
  patch -p1 -i ${srcdir}/0002-expose-function-for-setting-cursor-position.patch
  patch -p1 -i ${srcdir}/0003-add-function-for-setting-the-text-selections.patch
  patch -p1 -i ${srcdir}/0004-add-functions-to-getset-block-selection-mode.patch
  patch -p1 -i ${srcdir}/0005-expose-function-for-getting-the-selected-text.patch
  patch -p1 -i ${srcdir}/0006-Add-function-to-get-selection-position.patch

  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd vte

  ./configure --prefix=/usr --sysconfdir=/etc --libexecdir=/usr/lib/vte \
    --localstatedir=/var --disable-static --enable-introspection --enable-glade-catalogue --enable-gtk-doc

  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package(){
  cd vte
  make DESTDIR="$pkgdir" install

  rm "$pkgdir"/etc/profile.d/vte.sh
}
