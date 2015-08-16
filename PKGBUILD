# Maintainer: josephgbr <rafael.f.f1@gmail.com>

pkgname=lib32-icu
pkgver=49.1.2
_ver=49_1_2
pkgrel=1
pkgdesc="International Components for Unicode library (32 bit)"
arch=('x86_64')
url="http://www.icu-project.org/"
license=('custom:icu')
depends=('lib32-gcc-libs' 'icu')
makedepends=('gcc-multilib')
source=(http://download.icu-project.org/files/icu4c/${pkgver}/icu4c-${_ver}-src.tgz
        'icu.8198.revert.icu5431.patch')
md5sums=('bbc609fe5237202d7abf016141012a45'
         'ebd5470fc969c75e52baf4af94a9ee82')

build() {
  export CC='gcc -m32'
  export CXX='g++ -m32'
  export PKG_CONFIG_PATH='/usr/lib32/pkgconfig'
  
  cd icu/source
  # fix Malayalam encoding https://bugzilla.redhat.com/show_bug.cgi?id=654200
  patch -Rp3 -i "${srcdir}"/icu.8198.revert.icu5431.patch
  ./configure --prefix=/usr --sysconfdir=/etc --mandir=/usr/share/man --libdir=/usr/lib32
  make
}

package() {
  cd icu/source
  make -j1 DESTDIR="${pkgdir}" install
  rm -rf "${pkgdir}/usr"/{include,sbin,share}
  
  # keep icu-config-32
  find "${pkgdir}/usr/bin" -type f -not -name icu-config -delete
  mv "${pkgdir}/usr/bin"/icu-config{,-32}
  
  install -d m644 "${pkgdir}/usr/share/licenses"
  ln -s icu "${pkgdir}/usr/share/licenses/${pkgname}"
}

