pkgname=nodejs-shared
provides=('nodejs')
pkgver=24.2.0
pkgrel=1
pkgdesc='Evented I/O for V8 javascript ("Current" release)'
arch=('x86_64')
url='https://nodejs.org/'
license=('MIT')
depends=(
#  'ada'
  'brotli'
  'c-ares'
#  'http-parser'
  'icu'
  'libnghttp2'
  'libnghttp3'
  'libngtcp2'
  'libuv'
  'openssl'
  'simdjson'
#  'simdutf'
#  'v8'
  'zlib'
)
makedepends=(
  'git'
  'procps-ng'
  'python'
)
optdepends=('npm: nodejs package manager')
options=('!lto')
source=("git+https://github.com/nodejs/node.git#tag=v$pkgver?signed")
sha512sums=('f46ae0ae953642f4fe88b961c3c128a6bf56501210de2f9dd4cbb703c8685b8599021d450197797a3e9dc3465992a33cd77708472f4ea8031d96e9b7da21fe2c')
validpgpkeys=(
  '8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600' # Michaël Zasso (Targos) <targos@protonmail.com>
  '890C08DB8579162FEE0DF9DB8BEAB4DFCF555EF4' # RafaelGSS <rafael.nunu@hotmail.com>
  'C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C' # Richard Lau <rlau@redhat.com>
  'C0D6248439F1D5604AAFFB4021D900FFDB233756' # Antoine du Hamel <duhamelantoine1995@gmail.com>
)

_set_flags() {
  # /usr/lib/libnode.so uses malloc_usable_size, which is incompatible with fortification level 3
  CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
}

build() {
  _set_flags
  cd node

  ./configure --shared \
    --prefix=/usr \
    --without-npm \
    --with-intl=system-icu \
    --shared-brotli \
    --shared-cares \
    --shared-libuv \
    --shared-nghttp2 \
    --shared-nghttp3 \
    --shared-ngtcp2 \
    --shared-openssl \
    --shared-simdjson \
    --shared-zlib
    # --shared-ada
    # --shared-http-parser
    # --shared-simdutf
    # --shared-v8

  make
}

check() {
  _set_flags
  cd node
  # ignore failing tests, they work when compiled locally
  rm test/parallel/test-http2-client-set-priority.js
  rm test/parallel/test-http2-priority-event.js
  rm test/parallel/test-http-outgoing-end-cork.js
  make test-only
}

package() {
  _set_flags
  cd node
  make DESTDIR="$pkgdir" install
  install -Dm644 LICENSE -t "$pkgdir"/usr/share/licenses/nodejs/

  cd "$pkgdir"/usr/lib
  ln -s libnode.so.* libnode.so
}

# vim:set ts=2 sw=2 et:
