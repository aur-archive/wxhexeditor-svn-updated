# Maintainer: Xiao-Long Chen <chenxiaolong@cxl.epac.to>

pkgname=wxhexeditor-svn-updated
pkgver=307
pkgrel=1
pkgdesc="a free hex editor / disk editor for Linux, Windows and MacOSX"
arch=('i686' 'x86_64')
url="http://wxhexeditor.sourceforge.net/"
license=('GPL')
depends=('udis86' 'mhash' 'wxgtk')
makedepends=('subversion')
provides=('wxhexeditor')
conflicts=('wxhexeditor')

_svntrunk=https://wxhexeditor.svn.sourceforge.net/svnroot/wxhexeditor/trunk
_svnmod=wxhexeditor

build() {
  cd "${srcdir}"
  msg "Connecting to SVN server...."

  if [[ -d "${_svnmod}/.svn" ]]; then
    (cd "${_svnmod}" && svn up -r "${pkgver}")
  else
    svn co "${_svntrunk}" --config-dir ./ -r "${pkgver}" "${_svnmod}"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "${srcdir}/${_svnmod}-build"
  cp -r "${srcdir}/${_svnmod}" "${srcdir}/${_svnmod}-build"
  cd "${srcdir}/${_svnmod}-build"

  # Use shared udis86 because I can't get the included udis86 to compile 
  sed -i -e '/cd udis86/d' -e '/if libudis86/d' -e 's/-Iudis86/-I\/usr\/include\/libudis86/' -e '/LDFLAGS/s/^\(LDFLAGS.*libs`\)$/\1 -ludis86/' -e '/LIBS/s/udis86.*libudis86.a//' -e '/udis86.*libudis86.a:/d' makefile

  # Use shared mhash because I can't get the included mhash to compile
  sed -i -e '/CXXFLAGS/s/mhash\/include/\/usr\/include\/mutils/' -e '/LIBS/s/mhash.*libmhash.a//' -e '/cd mhash/d' -e '/if libmhash/d' -e '/LDFLAGS/s/^\(LDFLAGS.*libs`\)/\1 -lmhash/' -e '/mhash.*libmhash.a:/d' makefile

  make ${MAKEFLAGS}
}

package() {
  cd "${srcdir}/${_svnmod}-build"
  make PREFIX="${pkgdir}/usr/" install
}

# vim:set ts=2 sw=2 et:
