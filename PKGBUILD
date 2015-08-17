# $Id$
# Maintainer: Christian Kampka <christian@kampka.net>

pkgname=rsyslog-relp
pkgver=8.8.0
pkgrel=1
pkgdesc="An enhanced multi-threaded syslogd with a focus on security and reliability - build with support for the Relp protocol"
url="http://www.rsyslog.com/"
arch=('i686' 'x86_64')
license=('GPL3')
replaces=('rsyslog')
depends=('zlib' 'libestr' 'libee' 'json-c' 'systemd' 'liblogging')
makedepends=('postgresql-libs>=8.4.1' 'libmariadbclient' 'net-snmp' 'gnutls'
	     'python-docutils' 'librelp>=1.2.7')
optdepends=('postgresql-libs: PostgreSQL Database Support'
	    'libmariadbclient: MySQL Database Support'
	    'net-snmp'
	    'gnutls')
backup=('etc/rsyslog.conf'
	'etc/logrotate.d/rsyslog')
options=('strip' 'zipman')
source=("http://www.rsyslog.com/files/download/rsyslog/rsyslog-$pkgver.tar.gz"
	'rsyslog.logrotate'
	'rsyslog.conf')
md5sums=('188088dc496fb0a121edb8816d1fac83'
         '0d990373f5c70ddee989296007b4df5b'
         'd61dd424e660eb16401121eed20d98bc')

build() {
  local pkgname="rsyslog"
  cd ${srcdir}/${pkgname}-${pkgver}
  ./configure --prefix=/usr \
              --sbindir=/usr/bin \
              --enable-mysql \
              --enable-pgsql \
              --enable-mail \
              --enable-imfile \
              --enable-snmp \
              --enable-gnutls \
              --enable-inet \
              --enable-imjournal \
              --enable-omjournal \
              --enable-mmsequence \
              --enable-relp \
              --with-systemdsystemunitdir=/usr/lib/systemd/system
  make
}

package() {
  local pkgname=rsyslog
  cd ${srcdir}/${pkgname}-${pkgver}
  make install DESTDIR=${pkgdir}
  # Install Daemons and Configuration Files
  install -D -m644 $srcdir/${pkgname}.conf ${pkgdir}/etc/${pkgname}.conf
  install -D -m644 $srcdir/${pkgname}.logrotate ${pkgdir}/etc/logrotate.d/${pkgname}

  # fix location of systemctl and remove start precondition
  sed -i "$pkgdir/usr/lib/systemd/system/rsyslog.service" \
    -e 's@/bin/systemctl@/usr&@' \
    -e '/^ExecStartPre/d'
}
