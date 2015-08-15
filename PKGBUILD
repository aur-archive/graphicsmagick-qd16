# Maintainer: kayou <treboloc at laposte dot net>

pkgname=graphicsmagick-qd16
_realname=graphicsmagick
pkgver=1.3.18
pkgrel=1
pkgdesc="Image processing system with options for photivo-hg"
arch=('i686' 'x86_64')
url="http://www.graphicsmagick.org/"
license=('MIT')
makedepends=('perl')
depends=('bzip2' 'freetype2' 'ghostscript' 'jasper' 'lcms2' 'xz' 'libtiff' 'libwmf' 'libsm' 'libxml2' 'libltdl')
provides=('graphicsmagick')
conflicts=('graphicsmagick')
options=('!libtool')
source=("http://downloads.sourceforge.net/project/${_realname}/${_realname}/${pkgver}/GraphicsMagick-${pkgver}.tar.xz")
sha1sums=('085c23666adcf88585119cb6aea7efe5c58481d4')                                                                                                                                                    

build() {
  cd "${srcdir}/GraphicsMagick-$pkgver"

  ./configure \
	  --prefix=/usr \
	  --with-perl \
	  --enable-shared \
	  --disable-static \
	  --with-gs-font-dir=/usr/share/fonts/Type1 \
	  --with-quantum-depth=16 \
	  
  make
}

package() {
  cd "${srcdir}/GraphicsMagick-$pkgver"

  make DESTDIR="${pkgdir}" install

  # Install MIT license
  install -Dm644 "Copyright.txt" "${pkgdir}/usr/share/licenses/$_realname/Copyright.txt"

  # Install perl bindings
  # The patching was introduced in order to build perl module without installing package itself and
  # not to introduce unnecessary path into LD_RUN_PATH
  cd PerlMagick
  sed -i -e "s:'LDDLFLAGS'  => \"\(.*\)\":'LDDLFLAGS'  => \"-L${pkgdir}/usr/lib \1\":" Makefile.PL
  perl Makefile.PL INSTALLDIRS=vendor PREFIX=/usr DESTDIR="${pkgdir}"
  sed -i -e "s/LDLOADLIBS =/LDLOADLIBS = -lGraphicsMagick/" Makefile
  make
  make install

  # Remove perllocal.pod and .packlist
  rm -rf "${pkgdir}/usr/lib/perl5/core_perl"
  rm "${pkgdir}/usr/lib/perl5/vendor_perl/auto/Graphics/Magick/.packlist"
}
