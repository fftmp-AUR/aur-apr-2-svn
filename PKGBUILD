# Maintainer: fft <nobody@nowhere.no>

pkgname=libapr2-svn
pkgver=1904787
pkgrel=1
pkgdesc='Apache Portable Runtime library. Version 2 from SVN trunk.'
arch=('amd64')
url='https://apr.apache.org/'
control_fields=('Priority: optional' 'Section: libs')
depends=('libc6' 'expat') # libxml2 also supported
makedepends=('subversion' 'libexpat-dev')
optdepends=('python' 'sqlite')
source=("${pkgname}::svn+https://svn.apache.org/repos/asf/apr/apr/trunk/"
        'ship_find_apr.m4.patch'
        'fix-apr.pc.patch'
        'omit_extra_libs.patch'
        'dont_override_external_buildflags')
sha256sums=('SKIP'
            '268f65a263199901a07788a28e3e7bac74cd445dac677ca4e22a054d9e5315fd'
            '12595d331b48be9e44bd843635eb4f0f500bd213e197a551a9d383a28a24641f'
            '3d491d3af8fb5a75db4e085a17e5d8dcbe058bd256ef893ee779dc97fc9f8ad6'
            '5ac0bdc532479f6082d29115ac9d3ca24524fd8b97a556568755b88e5a68e3df')

pkgver() {
  cd "${pkgname}"
  local ver="$(svnversion)"
  printf "%s" "${ver//[[:alpha:]]}"
}

prepare() {
  cd "${pkgname}"
  svn revert -R ./
  patch -Np1 -i ../ship_find_apr.m4.patch
  patch -Np1 -i ../fix-apr.pc.patch
  patch -Np1 -i ../omit_extra_libs.patch
  patch -Np1 -i ../dont_override_external_buildflags
  ./buildconf
}

build() {
  cd "${pkgname}"
    ./configure --host=x86_64-linux-gnu --build=x86_64-linux-gnu \
    --enable-layout=Debian \
    --includedir=/usr/include/apr-2 \
    --libdir=/usr/lib/x86_64-linux-gnu \
    --with-installbuilddir=/usr/share/apr-2/build \
    --enable-nonportable-atomics \
    --enable-allocator-uses-mmap
  make
}

check() {
  cd "${pkgname}"
  make -j1 check
}

package() {
  cd "${pkgname}"
  make DESTDIR="${pkgdir}" install
  chmod a-x "${pkgdir}/usr/lib/x86_64-linux-gnu/libapr-2.la" "${pkgdir}/usr/lib/x86_64-linux-gnu/libapr-2.so.0.0.0"
  perl -p -i -e "s,^dependency_libs=.*,dependency_libs=''," "${pkgdir}/usr/lib/x86_64-linux-gnu/libapr-2.la"
  rm "${pkgdir}/usr/lib/x86_64-linux-gnu/apr.exp" # conflict with apr package and seems useless on linux
}
