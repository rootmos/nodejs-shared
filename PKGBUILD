pkgname=nodejs-shared
provides=('nodejs')
pkgver=23.4.0
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
source=("git+https://github.com/nodejs/node.git#tag=v$pkgver?signed"
        "https://gitlab.archlinux.org/archlinux/packaging/packages/nodejs/-/raw/main/0001-test-make-test-crypto-hash-compatible-with-OpenSSL-3.patch"
        "https://gitlab.archlinux.org/archlinux/packaging/packages/nodejs/-/raw/main/0002-test-adjust-OpenSSL-error-code-for-3.4.0.patch"
        "https://gitlab.archlinux.org/archlinux/packaging/packages/nodejs/-/raw/main/0001-test-disable-openssl-3.4.0-incompatible-tests.patch")
sha512sums=('1db1536af0c8ae154272aea1dba1c625c54790301a80ca0c363d4202a4f445f0637d40773c80b4ad5accdb53548c9f90e78b484604b5bd785d1b6798012d4d63'
            'dd3d2328c4b24228f0109aa3159989527bfa622497f685ce493fed8334ce5c367d07ebc442bb77b050559a4ec5eb0956661c658f6c9957bc40f1927d384cf728'
            '296a9117bee86ba141e0748e4857a5f2fb6e630f931f724ce1f3c0acfe166dfe1eb9d6bceaeb2ba06d23e8a937d02bb529aaf3ed458cd18b72fc745237e69b60'
            'a4f1218b6970b9beb4a0e3da5fb732e74722f014084423293be402b5876da7bba29c9aef770a1e34db7c8876b5dc7b150b44c32c5fd3ca4b741978342fcc636a')
validpgpkeys=(
  '8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600' # Michaël Zasso (Targos) <targos@protonmail.com>
  '890C08DB8579162FEE0DF9DB8BEAB4DFCF555EF4' # RafaelGSS <rafael.nunu@hotmail.com>
  'C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C' # Richard Lau <rlau@redhat.com>
  'C0D6248439F1D5604AAFFB4021D900FFDB233756' # Antoine du Hamel <duhamelantoine1995@gmail.com>
)

prepare() {
  cd node
  patch -Np1 -i ../0001-test-make-test-crypto-hash-compatible-with-OpenSSL-3.patch
  patch -Np1 -i ../0002-test-adjust-OpenSSL-error-code-for-3.4.0.patch
  patch -Np1 -i ../0001-test-disable-openssl-3.4.0-incompatible-tests.patch
}

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
  # Running an individual test: tools/test.py test/parallel/test-tls-psk-circuit.js
  # OpenSSL 3.4 broke a few tests in nodejs:
  # - https://github.com/nodejs/node/issues/56159
  # - https://github.com/nodejs/node/pull/56160
  # - test/parallel/test-tls-psk-circuit.js different error code produced
  make test
}

package() {
  cd node
  make DESTDIR="$pkgdir" install
  install -Dm644 LICENSE -t "$pkgdir"/usr/share/licenses/nodejs/

  cd "$pkgdir"/usr/lib
  ln -s libnode.so.* libnode.so
}

# vim:set ts=2 sw=2 et:
