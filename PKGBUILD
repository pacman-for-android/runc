# Maintainer: Sébastien "Seblu" Luttringer

pkgname=runc
pkgver=0.1.1
pkgrel=2
pkgdesc='CLI tool for managing OCI compliant containers'
arch=('x86_64')
url='https://runc.io/'
license=('Apache')
depends=('glibc' 'libseccomp')
makedepends=('git' 'go' 'go-md2man')
source=("git+https://github.com/opencontainers/runc#tag=v$pkgver")
md5sums=('SKIP')

prepare() {
  cd runc
  # apply patch from the source array (should be a pacman feature)
  local filename
  for filename in "${source[@]}"; do
    if [[ "$filename" =~ \.patch$ ]]; then
      msg2 "Applying patch ${filename##*/}"
      patch -p1 -N -i "$srcdir/${filename##*/}"
    fi
  done
  :
}

build() {
  mkdir -p src/github.com/opencontainers
  cd src/github.com/opencontainers
  ln -fs "$srcdir/runc"
  cd runc
  export GOPATH="$srcdir" BUILDTAGS='seccomp'
  make
  man/md2man-all.sh 2>/dev/null
}

#check() {
#  cd runc
#  make test
#}

package() {
  cd runc
  install -Dm755 runc "$pkgdir/usr/bin/runc"
  # man pages
  install -dm755 "$pkgdir/usr/share/man"
  mv man/man*/ "$pkgdir/usr/share/man"
}

# vim:set ts=2 sw=2 et:
