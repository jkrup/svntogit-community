# Maintainer: Daniel Micay <danielmicay@gmail.com>
# Contributor: Arch Haskell Team <>
# Contributor: Lex Black <autumn-wind at web dot de>

_hkgname=pandoc-types
pkgname=haskell-pandoc-types
pkgver=1.10
pkgrel=1
pkgdesc="Types for representing a structured document"
url="http://johnmacfarlane.net/pandoc"
license=("GPL")
arch=('i686' 'x86_64')
makedepends=('ghc')
depends=('haskell-containers' 'haskell-syb')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install=$pkgname.install
sha256sums=('e65b983aece74d57db53c6f611f92b9df9dd876e5f022f3a8612c092d6db78f5')

build() {
  cd "$srcdir/$_hkgname-$pkgver"
  runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
    --prefix=/usr --docdir=/usr/share/doc/$pkgname \
    --libsubdir=\$compiler/site-local/\$pkgid
  runhaskell Setup build
  runhaskell Setup haddock
  runhaskell Setup register --gen-script
  runhaskell Setup unregister --gen-script
  sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
  cd "$srcdir/$_hkgname-$pkgver"
  install -D -m744 register.sh "$pkgdir/usr/share/haskell/$pkgname/register.sh"
  install    -m744 unregister.sh "$pkgdir/usr/share/haskell/$pkgname/unregister.sh"
  install -d -m755 "$pkgdir/usr/share/doc/ghc/html/libraries"
  ln -s /usr/share/doc/$pkgname/html "$pkgdir/usr/share/doc/ghc/html/libraries/$_hkgname"
  runhaskell Setup copy --destdir="$pkgdir"
  install -D -m644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  rm -f "$pkgdir/usr/share/doc/$pkgname/LICENSE"
}
