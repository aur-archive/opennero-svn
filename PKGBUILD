# Maintainer: Isaac Aggrey <isaac.aggrey@gmail.com>

pkgname=opennero-svn
pkgver=1693
pkgrel=2
pkgdesc="Game platform for Artificial Intelligence research and education"
arch=('i686' 'x86_64')
url="https://code.google.com/p/opennero/"
license=('BSD')
depends=('boost-libs' 'libgl' 'libjpeg-turbo' 'libpng' 'libxxf86vm' 'python2' 'rsync')
makedepends=('cmake>=2.6' 'subversion' 'zlib')
optdepends=('wxpython: menu support in NERO Training and NERO Battle')
source=('OpenNERO.sh')
sha256sums=('5b4d8465b34185307c2747b5b08eda19fee882e0247fa6894bfb48aab7155fe5')

_svntrunk=http://opennero.googlecode.com/svn/trunk
_svnmod=opennero

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build/"

  #
  # BUILD HERE
  #

  cmake -DCMAKE_CXX_FLAGS:STRING=-fpermissive \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DPYTHON_INCLUDE_DIR:PATH=/usr/include/python2.7 \
    -DPYTHON_LIBRARY:FILEPATH=/usr/lib/libpython2.7.so "$srcdir/$_svnmod-build"
  make
}

package() {
  cd $srcdir/$_svnmod-build

  #install -dm755 $pkgdir/opt/
  cp -r dist/ $pkgdir/opt/$pkgname
  chown -R $USER $pkgdir/opt/$pkgname # OpenNERO needs to run in single directory
                                      # with files owned by user

  install -Dm755 ../../OpenNERO.sh $pkgdir/usr/bin/OpenNERO
  install -Dm644 "COPYING.txt" $pkgdir/usr/share/licenses/$pkgname/LICENSE
}

# vim:set ts=2 sw=2 et:
