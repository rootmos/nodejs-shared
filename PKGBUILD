pkgname=nodejs-shared
provides=('nodejs')
pkgver=23.1.0
pkgrel=1
pkgdesc='Evented I/O for V8 javascript'
arch=('x86_64')
url='https://nodejs.org/'
license=('MIT')
depends=(
  'brotli'
  'c-ares'
  'icu'
  'libnghttp2'
  'libnghttp3'
  'libngtcp2'
  'libuv'
  'openssl'
  'zlib'
) # 'http-parser' 'v8')
makedepends=(
  'git'
  'procps-ng'
  'python'
)
optdepends=('npm: nodejs package manager')
options=(!lto)
source=("git+https://github.com/nodejs/node.git#tag=v$pkgver?signed")
sha512sums=('360f9bb034cee992b7f7e3b067db84653ce9b4365ec2a993da7a1fb4c44af5ac561fc7090f107edf1c043a383cf4667ca082395a36ba84eedeb91e82e90c6536')
validpgpkeys=(
  '8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600' # Michaël Zasso (Targos) <targos@protonmail.com>
  '890C08DB8579162FEE0DF9DB8BEAB4DFCF555EF4' # RafaelGSS <rafael.nunu@hotmail.com>
  'C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C' # Richard Lau <rlau@redhat.com>
  'C0D6248439F1D5604AAFFB4021D900FFDB233756' # Antoine du Hamel <duhamelantoine1995@gmail.com>
)

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
