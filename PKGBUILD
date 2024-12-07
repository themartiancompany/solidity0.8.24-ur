# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Contributor: Matheus <matheusgwdl@protonmail.com>
# Contributor: Serge K <arch@phnx47.net>
# Contributor: Nicola Squartini <tensor5@gmail.com>

# shellcheck disable=SC2034
# shellcheck disable=SC2154
_pkg="solidity"
pkgver="0.8.24"
pkgname="${_pkg}${pkgver}"
pkgrel="1"
pkgdesc="Smart contract programming language."
arch=(
  "x86_64"
  "i686"
  "aarch64"
  "arm"
  "armv7l"
  "armv6l"
  "mips"
  "powerpc"
  "pentium4"
)
_http="https://github.com"
_ns="ethereum"
url="${_http}/${_ns}/${pkgname}"
license=(
  "GPL-3.0-or-later"
)
depends=(
  "boost-libs"
)
optdepends=(
  "cvc4: SMT checker"
  "z3: SMT checker"
)
makedepends=(
  "boost"
  "cmake"
)
checkdepends=(
  "evmone"
)
provides=(
  "solc=${pkgver}"
)
conflicts=(
  "solc"
  "solidity-bin"
  "solidity-git"
)
source=(
  "${pkgname}-v${pkgver}.tar.gz::${url}/releases/download/v${pkgver}/${pkgname}_${pkgver}.tar.gz"
)
sha512sums=(
  '2ddce3edfc1d570fb42d19d3164f5f7316d511bd3020c711b8176410b39432b7e137806bc63e23bb6c7381ab880c7e7e667217ab4cd8d92a6ad7e2ab145a194f'
)

_compile() {
  local \
    _tests="${1}" \
    _cmake_opts=()
  _cmake_opts=(
    -D CMAKE_BUILD_TYPE="None"
    -D CMAKE_INSTALL_PREFIX="/usr/"
    -D CMAKE_EXECUTABLE_SUFFIX="${pkgver}"
    -D ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    -D PEDANTIC="ON"
    -D PROFILE_OPTIMIZER_STEPS="OFF"
    -D SOLC_LINK_STATIC="OFF"
    -D SOLC_STATIC_STDLIBS="OFF"
    -D STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D STRICT_Z3_VERSION="OFF"
    -D TESTS="${_tests}"
    -D USE_LD_GOLD="OFF"
    -D USE_SYSTEM_LIBRARIES="OFF"
    -S "${srcdir}/${pkgname}_${pkgver}/"
    -Wno-dev
  )
  cmake \
    -B \
      "${srcdir}/${pkgname}_${pkgver}/build/" \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      "${srcdir}/${pkgname}_${pkgver}/build/"
}

build()
{
  local \
    _tests_switch=() \
    _tests_switch_status
  _tests_switch=(
    "OFF"
    # "ON"
  )
  for _tests_switch_status \
    in "${_tests_switch[@]}"; do
    _compile \
      "${_tests_switch_status}"
  done
}

check()
{
  _compile \
    "ON"
  "${srcdir}/${pkgname}_${pkgver}/build/test/soltest" \
    -p \
      true -- \
    --testpath \
      "${srcdir}/${pkgname}_${pkgver}/test/"
  _compile \
    "OFF"
}

package()
{
  # Assure that the directories exist.
  mkdir \
    -p \
      "${pkgdir}/usr/share/doc/${pkgname}/"
  # Install the software.
  DESTDIR="${pkgdir}/" \
  cmake \
    --install \
      "${srcdir}/${pkgname}_${pkgver}/build/"
  # Install the documentation.
  install \
    -Dm644 \
    "${srcdir}/${pkgname}_${pkgver}/README.md" \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  cp \
    -r \
    "${srcdir}/${pkgname}_${pkgver}/docs/"* \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      d \
    -exec \
      chmod \
        755 \
	{} +
  find \
    "${pkgdir}/usr/share/doc/${pkgname}/" \
    -type \
      f \
    -exec \
      chmod \
      644 \
      {} +
}
