# $Id: PKGBUILD 82275 2013-01-14 09:20:19Z spupykin $
# Maintainer: Sergej Pupykin <pupykin.s+arch@gmail.com>

pkgname=hostapd-legacy
_realpkgname=hostapd
pkgver=1.1
pkgrel=2
pkgdesc="IEEE 802.11 AP, IEEE 802.1X/WPA/WPA2/EAP/RADIUS Authenticator"
arch=('i686' 'x86_64')
url="http://w1.fi/hostapd/"
license=('custom')
conflicts=('hostapd')
depends=('openssl' 'libnl')
backup=('etc/hostapd/hostapd.conf'
        'etc/conf.d/hostapd'
        'etc/hostapd/hlr_auc_gw.milenage_db'
        'etc/hostapd/hostapd.accept'
        'etc/hostapd/hostapd.deny'
        'etc/hostapd/hostapd.eap_user'
        'etc/hostapd/hostapd.radius_clients'
        'etc/hostapd/hostapd.sim_db'
        'etc/hostapd/hostapd.vlan'
        'etc/hostapd/hostapd.wpa_psk'
        'etc/hostapd/wired.conf')
install=hostapd.install
source=(http://w1.fi/releases/$_realpkgname-$pkgver.tar.gz
	config
	hostapd
	hostapd.conf.d
	hostapd.service)

md5sums=('e3ace8306d066ab2d24b4c9f668e2dd7'
         '5d7ee10b04e33f22c37be56a4c33dddb'
         'd570327c385f34a4af24d3a0d61cea19'
         'f169534b0f59b341f6df1a21e0344511'
         'a0a16879eed5e4e41ae6b225a4809955')

build() {
  cd $_realpkgname-$pkgver/hostapd
  cp ../../config .config
  sed -i 's#/etc/hostapd#/etc/hostapd/hostapd#' hostapd.conf
  export CFLAGS="$CFLAGS $(pkg-config --cflags libnl-3.0)"
  make
}

package() {
  # RC script
  install -D hostapd "$pkgdir/etc/rc.d/hostapd"
  install -Dm644 hostapd.conf.d "$pkgdir/etc/conf.d/hostapd"

  # Systemd unit
  install -Dm644 hostapd.service "$pkgdir/usr/lib/systemd/system/hostapd.service"

  cd $_realpkgname-$pkgver

  # License
  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$_realpkgname/COPYING"

  cd hostapd

  # Binaries
  install -d "$pkgdir/usr/bin"
  install -t "$pkgdir/usr/bin" hostapd hostapd_cli

  # Configuration
  install -d "$pkgdir/etc/hostapd"
  install -m644 -t "$pkgdir/etc/hostapd" \
    hostapd.{accept,conf,deny,eap_user,radius_clients,sim_db,vlan,wpa_psk} \
    wired.conf hlr_auc_gw.milenage_db

  # Man pages
  install -Dm644 hostapd.8 "$pkgdir/usr/share/man/man8/hostapd.8"
  install -Dm644 hostapd_cli.1 "$pkgdir/usr/share/man/man1/hostapd_cli.1"
}
