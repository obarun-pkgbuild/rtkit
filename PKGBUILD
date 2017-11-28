# Maintainer: Eric Vidal <eric@obarun.org>


pkgname=rtkit
pkgver=0.11+8+ge0a51fe
pkgrel=2
pkgdesc="Realtime Policy and Watchdog Daemon"
arch=(x86_64)
url="https://github.com/heftig/rtkit"
license=(GPL 'custom:BSD')
depends=('dbus' 'polkit')
makedepends=(git)
_commit=e0a51fe262ff7167622a8e58f876a663b9471124 # master
source=("git+https://github.com/heftig/rtkit#commit=$_commit"
		'rtkit.sysusers'
		'keep-daemon-independent.patch')
sha256sums=('SKIP'
            'fd2ec923f003147f1a6ced0a5e11f652bc743edb5a5912f4ad54cc0d89162a75'
            '7972c38584729f41a5d95f40c753267acc3c9492fc18f40475e9f0278f448d67')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/^v//;s/-/+/g'
}

prepare() {
  cd $pkgname
  # Keep daemon independant
  patch -Np1 -i ${srcdir}/keep-daemon-independent.patch
  ./autogen.sh
}

build() {
  cd ${pkgname}
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
  cd ${pkgname}
  
  make DESTDIR="$pkgdir" install

  install -Dt "$pkgdir/usr/share/dbus-1/interfaces" -m644 org.freedesktop.RealtimeKit1.xml

  install -Dm644 "$srcdir/rtkit.sysusers" "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"

  sed -ne '4,25p' rtkit.c |
    install -Dm644 /dev/stdin "$pkgdir/usr/share/licenses/$pkgname/COPYING"
  install -Dt "$pkgdir/usr/share/licenses/$pkgname" -m644 LICENSE
}
