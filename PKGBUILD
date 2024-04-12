pkgname=nodejs-shared
provides=('nodejs')
pkgver=21.7.2
_commit=d4a1d050c0b22f0455bb6eb53eca8f7e7da937d2
pkgrel=1
pkgdesc='Evented I/O for V8 javascript'
arch=('x86_64')
url='https://nodejs.org/'
license=('MIT')
options=(!lto)
depends=('icu' 'libuv' 'libnghttp2' 'libnghttp3' 'libngtcp2' 'openssl' 'zlib' 'brotli' 'c-ares') # 'http-parser' 'v8')
makedepends=('git' 'python' 'procps-ng')
optdepends=('npm: nodejs package manager')
source=("git+https://github.com/nodejs/node.git#commit=$_commit")
sha512sums=('488d9f320dc183d27bd6ec770d9070da39cebe135a2582cbb44c4577d68addea3ffe15548ce17e02004e0ecd54bd55c7e7e5b2fbc8b3dac61fbbdad29df30cef')

build() {
  cd node

  # /usr/lib/libnode.so uses malloc_usable_size, which is incompatible with fortification level 3
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  ./configure --shared \
    --prefix=/usr \
    --without-npm \
    --with-intl=system-icu \
    --shared-libuv \
    --shared-nghttp2 \
    --shared-nghttp3 \
    --shared-ngtcp2 \
    --shared-openssl \
    --shared-zlib \
    --shared-brotli \
    --shared-cares
    # --shared-v8
    # --shared-http-parser

  make
}

check() {
  cd node
  make test || :
}

package() {
  cd node
  make DESTDIR="$pkgdir" install
  install -Dm644 LICENSE -t "$pkgdir"/usr/share/licenses/nodejs/

  cd "$pkgdir"/usr/lib
  ln -s libnode.so.* libnode.so
}

# vim:set ts=2 sw=2 et:
