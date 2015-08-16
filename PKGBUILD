# Contributor: Sietse van der Molen <sietse@vdmolen.eu> 

pkgname=ncmpcpp-xdg-config-git
pkgver=20130624
pkgrel=2
pkgdesc="An almost exact clone of ncmpc with some new features. Latest git pull patched to use XDG_CONFIG_HOME" 
arch=('i686' 'x86_64')
url="http://unkart.ovh.org/ncmpcpp/"
license=('GPL2')
depends=('ncurses' 'libmpdclient>=2.1')
makedepends=('git' 'boost-libs')
optdepends=('curl: fetch lyrics'
'taglib: tag editor'
'fftw: frequency spectrum mode visualization')
conflicts=('ncmpcpp')
#install=${pkgname}.install
source=(xdg.patch)
md5sums=('c05e8e868c80f8521e68610ee7d26e46')

_gitroot="git://repo.or.cz/ncmpcpp.git"
_gitname="ncmpcpp"

build() {
  cd "$srcdir"
  msg "Connecting to repo.or.cz GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi
  msg "GIT checkout done or server timeout"

  msg "Patching"
  patch -p0 -i $srcdir/xdg.patch

  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  cp -r "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  ./autogen.sh BOOST_LIB_SUFFIX='' \
	  --prefix=/usr \
	  --enable-clock \
	  --enable-outputs \
	  --enable-visualizer
  make || return 1
  make DESTDIR="$pkgdir/" install
} 
