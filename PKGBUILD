# $Id: PKGBUILD 169593 2012-10-24 04:24:19Z heftig $
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Hugo Doria <hugo@archlinux.org>

pkgname=deluge-patched
_pkgname=deluge
pkgver=1.3.5
pkgrel=2
pkgdesc="A BitTorrent client with multiple user interfaces in a client/server model with some patches"
arch=('any')
url="http://deluge-torrent.org/"
license=('GPL3')
conflicts=(deluge)
depends=(python2-xdg 'libtorrent-rasterbar>=0.15.0' twisted python2-pyopenssl
         xdg-utils python2-chardet desktop-file-utils hicolor-icon-theme
         python2-distribute)
makedepends=(intltool pygtk librsvg python2-mako)
optdepends=('python2-notify: libnotify notifications'
            'pygtk: needed for gtk ui'
            'librsvg: needed for gtk ui'
            'python2-mako: needed for web ui')
backup=(etc/conf.d/deluged)
install=deluge.install
source=(http://download.deluge-torrent.org/source/${_pkgname}-$pkgver.tar.bz2
        deluge.tmpfiles.conf deluged deluge-web deluged.service deluge-web.service deluged.conf
        fs31433.patch issue2169.patch)
md5sums=('f17ef6686f33e12694b44976e5ed7721'
         'c50385d32a2db0ef3f46b8caadb0e988'
         '443690c730263b76a465dc413f695a86'
         '37538a1b049b177e9ea1014331e29689'
         '6b831c889f365f58317dc4b78c167a62'
         'b3fff6601a5971bba89fa9a85dcf9ce8'
         '71d556cf7ce3bb59391797827347e80c'
         '65311330bd87440c50f2bb7251f46fcd'
         '234bc58749f83ddadfdb39fb633b6d64')

build() {
  cd ${_pkgname}-$pkgver

  # Fix moving to storage (FS#31433)
  patch -Np1 -i ../fs31433.patch
  # Fix dowload locatioin bug (http://dev.deluge-torrent.org/ticket/2169)
  patch -Np1 -i ../issue2169.patch

  python2 setup.py build
}

package() {
  cd ${_pkgname}-$pkgver
  python2 setup.py install --prefix=/usr --root="$pkgdir" --optimize=1
  install -Dm644 deluge/data/pixmaps/deluge.svg "$pkgdir/usr/share/pixmaps/deluge.svg"

  _dir="$pkgdir/usr/lib/python2.7/site-packages/deluge/ui"
  sed -i '1s/python$/&2/' "$_dir"/{Win32IconImagePlugin.py,web/gen_gettext.py}

  cd ..
  install -Dm644 deluge.tmpfiles.conf "$pkgdir/usr/lib/tmpfiles.d/deluge.conf"
  install -D deluged "$pkgdir/etc/rc.d/deluged"
  install -D deluge-web "$pkgdir/etc/rc.d/deluge-web"
  install -Dm644 deluged.service "$pkgdir/usr/lib/systemd/system/deluged.service"
  install -Dm644 deluge-web.service "$pkgdir/usr/lib/systemd/system/deluge-web.service"
  install -Dm644 deluged.conf "$pkgdir/etc/conf.d/deluged"

  install -d "$pkgdir/srv"
  install -d -m 775 -o 125 -g 125 "$pkgdir/srv/deluge"
}
