pkgname=redis
pkgver=2.8.14
pkgrel=1
pkgdesc='Advanced key-value store'
arch=('x86_64')
url='http://redis.io/'
license=('BSD')
depends=('jemalloc')
backup=('etc/redis.conf'
        'etc/logrotate.d/redis')
install=redis.install
source=(http://download.redis.io/releases/redis-$pkgver.tar.gz
        redis.service
        redis.logrotate redis.tmpfiles.d
        redis.conf-sane-defaults.patch
        redis-2.8.11-use-system-jemalloc.patch)
md5sums=('5005c764bf8760ef661c8b198bb01780'
         'aec12c881dc2693754f85539ae8e0bc7'
         '9e2d75b7a9dc421122d673fe520ef17f'
         'dd9ab8022b4d963b2e5899170dfff490'
         'b1beae6954b780da261b4056fd7f918a'
         '2ae176578f538e37a67a463258302bc6')

prepare() {
  cd $pkgname-$pkgver
  patch -p1 -i ../redis.conf-sane-defaults.patch
  patch -p1 -i ../redis-2.8.11-use-system-jemalloc.patch
}

build() {
  make -C $pkgname-$pkgver
}

package() {
  cd $pkgname-$pkgver
  make PREFIX="$pkgdir"/usr install

  install -Dm644 COPYING "$pkgdir"/usr/share/licenses/redis/LICENSE
  install -Dm644 redis.conf "$pkgdir"/etc/redis.conf
  install -Dm644 ../redis.service "$pkgdir"/usr/lib/systemd/system/redis.service

  # files kept for compatibility with installations made before 2.8.13-2
  install -Dm644 ../redis.logrotate "$pkgdir"/etc/logrotate.d/redis
  install -Dm644 ../redis.tmpfiles.d "$pkgdir"/usr/lib/tmpfiles.d/redis.conf
}
