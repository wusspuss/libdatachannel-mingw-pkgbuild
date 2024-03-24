# Maintainer: Some One <some.one@some.email.com>

_realname=libdatachannel
pkgbase=mingw-w64-libdatachannel
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=0.20.2
pkgrel=1
pkgdesc="C/C++ WebRTC network library featuring Data Channels, Media Transport, and WebSockets"
arch=('x86_64')
mingw_arch=('mingw64' 'ucrt64' 'clang64' 'clangarm64')
depends=("${MINGW_PACKAGE_PREFIX}-gcc-libs" "${MINGW_PACKAGE_PREFIX}-openssl" )
url="https://github.com/paullouisageneau/$_realname"
license=('MPL2')
makedepends=("${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja"
             "${MINGW_PACKAGE_PREFIX}-cc"
             'git')
# _commit='ff9ee68d7e31784c6fea3c864a235ad1f32ff026'
source=("git+https://github.com/paullouisageneau/$_realname.git#tag=v$pkgver")
sha256sums=('SKIP')

prepare() {
  cd "${srcdir}/${_realname}"

  git submodule update --init --recursive --depth 1
}

build() {
  mkdir -p "${srcdir}/build-${MSYSTEM}" && cd "${srcdir}/build-${MSYSTEM}"

  declare -a extra_config
  if check_option "debug" "n"; then
    extra_config+=("-DCMAKE_BUILD_TYPE=Release")
  else
    extra_config+=("-DCMAKE_BUILD_TYPE=Debug")
  fi

  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
    "${MINGW_PREFIX}"/bin/cmake.exe \
      -GNinja \
      -DCMAKE_INSTALL_PREFIX="${MINGW_PREFIX}" \
      "${extra_config[@]}" \
      -DBUILD_{SHARED,STATIC}_LIBS=ON \
      ../${_realname} \
      -DUSE_GNUTLS=0 \
      -DUSE_NICE=0 \

  "${MINGW_PREFIX}"/bin/cmake.exe --build .
}

package() {
  cd "${srcdir}/build-${MSYSTEM}"

  DESTDIR="${pkgdir}" "${MINGW_PREFIX}"/bin/cmake.exe --install .

  install -Dm644 "${srcdir}/${_realname}/LICENSE" "${pkgdir}${MINGW_PREFIX}/share/licenses/${_realname}/LICENSE"
}
