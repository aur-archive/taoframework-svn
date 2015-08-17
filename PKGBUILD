pkgname=taoframework-svn
pkgver=237
pkgrel=1
pkgdesc="A collection of bindings to facilitate cross-platform game-related development utilizing the .NET platform"
arch=('any')
url="http://sourceforge.net/projects/taoframework/"
license=('MIT')
makedepends=('mono>=1.1' 'subversion')
conflicts=(taoframework)
provides=(taoframework)
_svntrunk='https://taoframework.svn.sourceforge.net/svnroot/taoframework/trunk'
_svnmod='taoframework'

build() {
  cd "$srcdir"
  
  if [ -d "${_svnmod}/.svn" ]; then
    (cd "$_svnmod" && svn up -r $pkgver)
  else
    svn co "$_svntrunk" --config-dir ./ -r $pkgver $_svnmod
  fi

  msg 'SVN checkout done or server timeout'

  rm -rf "${_svnmod}-build"
  cp -r "$_svnmod" "${_svnmod}-build"
  cd "${_svnmod}-build"
  
  sed -i 's|monodocer --assembly:$$x/$$x.dll --path:doc/$$x;|monodocer -assembly:$$x/$$x.dll -path:doc/$$x;|' "src/Makefile.am"
  sed -i 's|nunit|mono-nunit|' "tests/Ode/Makefile.am" "tests/Sdl/Makefile.am"

  sed -i 's|liblua5.1.so.0|liblua.so.5.1|' "src/Tao.Lua/Tao.Lua.dll.config"
  sed -i 's|libode.so.0debian1|libode.so.1|' "src/Tao.Ode/Tao.Ode.dll.config"
  sed -i 's|libopenal.so.0|libopenal.so.1|' "src/Tao.OpenAl/Tao.OpenAl.dll.config"
  sed -i 's|libphysfs-1.0.so.0|libphysfs.so.1|' "src/Tao.PhysFs/Tao.PhysFs.dll.config"
  sed -i 's|libSDL_gfx.so.4|libSDL_gfx.so.13|' "src/Tao.Sdl/Tao.Sdl.dll.config"

  chmod 755 ./bootstrap
  ./bootstrap

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var
  make
}

package() {
  cd "$srcdir/${_svnmod}-build"

  make DESTDIR="$pkgdir/" install

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}
