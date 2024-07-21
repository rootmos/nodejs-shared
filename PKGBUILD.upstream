# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Daniel M. Capella <polyzen@archlinux.org>
# Contributor  Bartłomiej Piotrowski <bpiotrowski@archlinux.org>
# Contributor: Thomas Dziedzic < gostrc at gmail >
# Contributor: James Campos <james.r.campos@gmail.com>
# Contributor: BlackEagle < ike DOT devolder AT gmail DOT com >
# Contributor: Dongsheng Cai <dongsheng at moodle dot com>
# Contributor: Masutu Subric <masutu.arch at googlemail dot com>
# Contributor: TIanyi Cui <tianyicui@gmail.com>

pkgname=nodejs
pkgver=22.5.1
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
sha512sums=('83cba0ef3d32ad765cedca0b722e65ae822a118e3ecd6e9e14059d503b0a83db52a6ec09a08340d1378d94417c628ec61b75fd95f9d381c058ca0c6ddf3dc3da')
validpgpkeys=(
  '8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600' # Michaël Zasso (Targos) <targos@protonmail.com>
  '890C08DB8579162FEE0DF9DB8BEAB4DFCF555EF4' # RafaelGSS <rafael.nunu@hotmail.com>
  'C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C' # Richard Lau <rlau@redhat.com>
)

build() {
  cd node

  # /usr/lib/libnode.so uses malloc_usable_size, which is incompatible with fortification level 3
  export CFLAGS="${CFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"
  export CXXFLAGS="${CXXFLAGS/_FORTIFY_SOURCE=3/_FORTIFY_SOURCE=2}"

  ./configure \
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
}

# vim:set ts=2 sw=2 et:
