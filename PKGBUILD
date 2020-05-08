# Maintainer: Rudy Matela <rudy@matela.com.br>
# Contributor: Ivan Shapovalov <intelfx@intelfx.name>
# Contributor: _le34n$ <4le34n@gmail.com>
# Contributor: Starfry <archlinux@jelmail.com>
# Original PKGBUILD : https://aur.archlinux.org/packages/nfdump/
# Modified by 2igosha

pkgname=nfdump
pkgver=master
pkgrel=1
pkgdesc="A set of tools to collect and process netflow data."
arch=('i686' 'x86_64')
url="https://github.com/phaag/nfdump/"
license=('BSD')
depends=('rrdtool')
makedepends=()
source=("nfdump-master.tar.gz::https://github.com/phaag/nfdump/archive/master.tar.gz"
        'sysusers'
        'tmpfiles'
        'service'
        'servicepcap'
        'netctl')
md5sums=('SKIP'
         '80401774329a767d43f724c48055d4a4'
         'df2664d100e5fbf424073031882162bd'
         'c71e463a9356ded3430b9c836e29e0fa'
         'e1f0f7902307c688160eb64e4f3f8be9'
         '72f8bc645a6645ff0d0b467ab5bdfcbf'
     )

prepare() {
	cd "$pkgname-$pkgver"

	sed -re 's|-lnfdump|libnfdump.la|g' -i bin/Makefile.am
}

build() {
	cd "$pkgname-$pkgver"

	./autogen.sh  # needed since 1.6.17
	./configure --prefix=/usr \
	  --enable-nfprofile \
	  --enable-nftrack \
	  --enable-sflow \
      --enable-nfpcapd

	make
}

package() {
	cd "$pkgname-$pkgver"

	make DESTDIR="$pkgdir" install
	install -Dm644 COPYING \
		"$pkgdir/usr/share/licenses/nfdump/COPYING"
	install -Dm644 ../sysusers "$pkgdir/usr/lib/sysusers.d/$pkgname.conf"
	install -Dm644 ../tmpfiles "$pkgdir/usr/lib/tmpfiles.d/$pkgname.conf"
	install -Dm644 ../service "$pkgdir/usr/lib/systemd/system/nfcapd@.service"
	install -Dm644 ../servicepcap "$pkgdir/usr/lib/systemd/system/nfpcapd@.service"
    install -Dm644 ../netctl "$pkgdir/etc/netctl/examples/$pkgname"
}
