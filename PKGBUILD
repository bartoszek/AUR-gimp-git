# Maintainer: Iru Cai <mytbk920423@gmail.com>
# Maintainer: Alexander Hunziker <alex.hunziker@gmail.com>
# Contributor: Alessio Biancalana <dottorblaster@gmail.com>

pkgname=gimp-git
epoch=1
pkgver=2.9.4.1760.g2a7a53b384
pkgrel=1
pkgdesc="GNU Image Manipulation Program"
arch=('i686' 'x86_64')
url="http://www.gimp.org"
license=('GPL' 'LGPL')
depends=('pygtk' 'lcms2>=2.2' 'libwmf>=0.2.8' \
	'libgexiv2' 'librsvg>=2.16.1' 'desktop-file-utils' \
	'libexif>=0.6.15' 'libart-lgpl>=2.3.19' 'dbus-glib' 'gtk-doc' \
	'babl>=0.1.27' 'gegl-git>=0.3.15' 'openexr>=1.6.1' 'libmypaint>=1.3.0')
makedepends=('git' 'gutenprint>=5.0.0' 'intltool>=0.40.1' \
                   'gnome-python>=2.16.2' 'poppler>=0.12.4' \
                   'alsa-lib>=1.0.0' 'libxslt')
optdepends=('gutenprint: for sophisticated printing only as gimp has built-in cups print support'
            'poppler-glib: for pdf support'
            'alsa-lib: for MIDI event controller module'
            'curl: for URI support'
            'ghostscript: for postscript support')
options=('!libtool' '!makeflags')
provides=('gimp')
conflicts=('gimp')
source=(git://git.gnome.org/gimp
	linux.gpl)
md5sums=('SKIP'
         'bb27bc214261d36484093e857f015f38')

_gitname=gimp

pkgver() {
    cd $_gitname
    git describe --always | sed -e 's/GIMP_//' -e 's/[_-]/./g'
}

build() {
  cd "$srcdir/$_gitname"

  if [ -f /usr/lib/pkgconfig/libmypaint-1.3.pc ]; then
	  sed -i 's/libmypaint /libmypaint-1.3 /g' configure.ac
  fi

  PYTHON=/usr/bin/python2 ./autogen.sh --prefix=/usr --sysconfdir=/etc \
    --enable-mp --enable-gimp-console --enable-gimp-remote \
    --enable-python --with-gif-compression=lzw --with-libcurl \
    --without-aa --without-hal --without-gvfs --without-gnomevfs
  make
}

package() {
  cd "$srcdir/$_gitname"
  make DESTDIR="$pkgdir/" install
#  sed -i 's|#!/usr/bin/env python|#!/usr/bin/env python2|' "${pkgdir}"/usr/lib/gimp/2.0/plug-ins/*.py
  install -D -m644 "${srcdir}/linux.gpl" "${pkgdir}/usr/share/gimp/2.0/palettes/Linux.gpl"

  ln -s gimp-2.9 ${pkgdir}/usr/bin/gimp
  ln -s gimp-console-2.9 ${pkgdir}/usr/bin/gimp-console
}

