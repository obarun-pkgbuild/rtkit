# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/rtkit
# 						Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# 						Contributor: Corrado Primier <bardo@aur.archlinux.org>

pkgname=rtkit
pkgver=0.11
pkgrel=6
pkgdesc="Realtime Policy and Watchdog Daemon"
arch=(x86_64)
url="http://git.0pointer.de/?p=rtkit.git"
license=(GPL 'custom:BSD')
depends=('dbus' 'polkit')
optdepends=('rtkit-s6serv: rtkit s6 service'
			'rtkit-runitserv: rtkit runit service')
install=rtkit.install
source=(http://0pointer.de/public/${pkgname}-$pkgver.tar.xz
        0001-SECURITY-Pass-uid-of-caller-to-polkit.patch)
md5sums=('a96c33b9827de66033d2311f82d79a5d'
         '70df212cba2a6366ff960b60d55858d3')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
  cd ${pkgname}-$pkgver
  patch -Np1 -i ../0001-SECURITY-Pass-uid-of-caller-to-polkit.patch
  autoreconf -fi
}

build() {
  cd ${pkgname}-$pkgver
  ./configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --libexecdir=/usr/lib/rtkit \
	--with-systemdsystemunitdir=no
  make

  ./rtkit-daemon --introspect > org.freedesktop.RealtimeKit1.xml
}

package() {
  cd ${pkgname}-$pkgver
  make DESTDIR="$pkgdir" install

  install -Dm644 org.freedesktop.RealtimeKit1.xml \
    "$pkgdir/usr/share/dbus-1/interfaces/org.freedesktop.RealtimeKit1.xml"

  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
  sed -ne '4,25p' rtkit.c >"$pkgdir/usr/share/licenses/${pkgname}/COPYING"
}
