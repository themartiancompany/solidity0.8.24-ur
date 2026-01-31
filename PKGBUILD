# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025, 2026  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Matheus
#     <matheusgwdl@protonmail.com>
#   Serge K
#     <arch@phnx47.net>
#   Nicola Squartini
#     <tensor5@gmail.com>

# shellcheck disable=SC2034
# shellcheck disable=SC2154
_os="$(
  uname \
    -o)"
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
_locale="$(
  locale |
    grep \
      "LANG=" |
      awk \
        -F \
          "=" \
        '{print $1}')"
if [[ ! -v "_en" ]]; then
  _en="true"
  if [[ "${_locale}" == "C.UTF-8" ]]; then
    _en="true"
  fi
fi
if [[ ! -v "_it" ]]; then
  _it="true"
  if [[ "${_locale}" == "it_IT.UTF-8" ]]; then
    _it="true"
  fi
fi
if [[ ! -v "_like" ]]; then
  if [[ "${_it}" == "true" ]]; then
    _like="mo-me-lo-segno"
  fi
  if [[ "${_en}" == "true" ]]; then
    _like="never-gonna-give-you-up"
  fi
fi

_boost_pkgver_get() {
  local \
    _modes=() \
    _pkgs=() \
    _cut_opts=()
  _modes+=(
    "Q"
    "S"
  )
  _pkgs+=(
    "boost-libs"
    "boost"
  )
  _cut_opts=(
    -d
      "-"
    -f
      "1"
    --complement
  )
  for _pkg in "${_pkgs[@]}"; do
    for _mode in "${_modes[@]}"; do
      _boost_pkgver="$(
        ( pacman \
            -"${_mode}"i \
            "${_pkg}" \
            2>"/dev/null" || \
          true ) |
          grep \
            "Version" |
            awk \
              '{print $3}' |
              rev |
                cut \
                  "${_cut_opts[@]}" |
              rev || \
        echo \
          "null")"
      if [[ "${_boost_pkgver}" != "" && \
            "${_boost_pkgver}" != "null" ]]; then
        break
      fi
    done
  done
}

if [[ ! -v "_boost_pkgver" ]]; then
  _boost_pkgver_get
fi
_boost_majver="${_boost_pkgver%.*}"
_boost_oldest="$(
  printf \
    "%s\n%s" \
    "1.89" \
    "${_boost_majver}" |
    sort \
      -V |
      head \
        -n \
          1)"
if [[ ! -v "_git" ]]; then
  _git="${_evmfs}"
fi
if [[ ! -v "_git_service" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    _git_service="gitlab"
  elif [[ "${_boost_oldest}" != "1.89" ]]; then
    _git_service="github"
  fi
fi
if [[ ! -v "_git_http" ]]; then
  if [[ "${_git_service}" == "gitlab" ]]; then
    _git_http="${_git_service}"
  elif [[ "${_git_service}" == "github" ]]; then
    _git_http="${_git_service}"
  fi
fi
if [[ "${_os}" == "Android" ]]; then
  _compiler="clang"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler="gcc"
fi
if [[ ! -v "_cmake_generator" ]]; then
  _cmake_generator="make"
  # _cmake_generator="ninja"
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    _archive_format="tar.gz"
  fi
fi
_pkg=solidity
pkgver="0.8.24"
pkgbase="${_pkg}${pkgver}"
pkgname=(
  "${pkgbase}"
)
_0_8_24_commit=""
_bundle_commit="142aa62e6805505b6a06cbeeec530f5c8bf0bfdd"
_0_8_24_1_commit=""
pkgrel=1
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
# In late 2025 or 2026, according
# to Github, Solidity publishing
# namespace seems to have been moved
# from 'ethereum' to 'argotorg'
# _ns="ethereum"
# Despite this, most Solidity versions requires
# changes to be built with Boost versions
# later than 1.83 which are only published
# on The Martian Company namespaces.
if [[ ! -v "_ns" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    _ns="themartiancompany"
  elif [[ "${_boost_oldest}" != "1.89" ]]; then
    _ns="argotorg"
  fi
fi
if [[ "${_ns}" == "themartiancompany" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _commit="${_bundle_commit}"
    elif [[ "${_evmfs}" == "false" ]]; then
      _commit="${_0_8_24_1_commit}"
    fi
  fi
elif [[ "${_ns}" == "argotorg" ]]; then
  _commit="${_0_8_24_commit}"
fi
_http="https://${_git_http}.com"
url="${_http}/${_ns}/${_pkg}"
license=(
  "GPL-3.0-or-later"
)
depends=()
_boost_makedepends=(
  "boost"
)
if [[ "${_os}" == "Android" ]]; then
  _boost_pkgname="boost"
  _boost_makedepends+=(
    "boost-headers"
    "boost-static"
  )
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _boost_pkgname="boost-libs"
fi
depends=(
  "${_boost_pkgname}"
  "fmt"
  "nlohmann-json"
  "range-v3"
)
optdepends=(
  "cvc4: SMT checker"
  "z3: SMT checker"
)
makedepends=(
  "${_boost_makedepends[@]}"
  "cmake>3.10"
  "${_compiler}"
  "${_cmake_generator}"
  "fmt"
  "nlohmann-json"
  "range-v3"
)
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
if [[ "${_evmfs}" == "true" ]]; then
  makedepends+=(
    "evmfs"
  )
fi
checkdepends=(
  "evmone"
)
group=(
  "ethereum"
  "hip"
)
provides=(
  "${_pkg}=${pkgver}"
  "solc=${pkgver}"
)
conflicts=(
  "solc${pkgver}"
  "${_pkg}-bin"
  "${_pkg}-git"
)
if [[ "${_git}" == "false" ]]; then
  if [[ "${_boost_oldest}" == "1.89" ]]; then
    _tag="${_commit}"
    _tag_name="commit"
    _tarname="${_pkg}-${_tag}"
  elif [[ "${_boost_oldest}" != "1.89" ]]; then
    _tag="${pkgver}"
    _tag_name="pkgver"
    _tarname="${_pkg}_${_tag}"
  fi
elif [[ "${_git}" == "true" ]]; then
  _tag="${_commit}"
  _tag_name="commit"
  _tarname="${_pkg}-${_tag}"
fi
_0_8_24_1_tarname="${_pkg}-${_0_8_24_1_commit}"
_tarfile="${_tarname}.${_archive_format}"
_0_8_24_1_tarfile="${_0_8_24_1_tarname}.${_archive_format}"
_bundle_sum="77860b58f9d6c4a9a9cb1ceaae7ebe5d856f91f3ccd96f67d5ea6a019d79d1fb"
_bundle_sig_sum="7f737e7a88fdb8e96b428974592def4bbdf5bf24656b12ac5af76084b7fca095"
_0_8_24_1_sum=""
_0_8_30_1_sig_sum=""
_github_sum="SKIP"
_github_sig_sum="SKIP"
_gitlab_sum="SKIP"
_gitlab_sig_sum="SKIP"
_github_release_sum=""
_github_release_sha512_sum='24c3eb63b8c6ab8cae22fe26d7ce58002c34fa4e7371841833ba87363af2706826f5f86a0ef6945d7073fe52458e6f55f31b86af3469a6b039854aca36ebb869'
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _sum="${_gitlab_sum}"
      _sig_sum="${_gitlab_sig_sum}"
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="SKIP"
    _sig_sum="SKIP"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
      elif [[ "${_git_service}" == "gitlab" ]]; then
        _sum="${_gitlab_sum}"
        _sig_sum="${_gitlab_sig_sum}"
    fi
  fi
fi
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_kid_ns="0x926acb6aA4790ff678848A9F1C59E578B148C786"
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_bundle_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_bundle_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
_0_8_24_1_uri="${_evmfs_dir}/${_0_8_24_1_sum}"
_0_8_24_1_src="${_0_8_24_1_tarfile}::${_0_8_24_1_uri}"
_0_8_24_1_sig_uri="${_evmfs_dir}/${_0_8_24_1_sig_sum}"
_0_8_24_1_sig_src="${_0_8_24_1_tarfile}.sig::${_0_8_24_1_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "false" ]]; then
    makedepends+=(
      "ur"
      "${_like}"
    )
    _src=""
    _sum=""
  elif [[ "${_git}" == "true" ]]; then
    _src="${_evmfs_src}"
    _sum="${_bundle_sum}"
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_bundle_sig_sum}"
    )
    if [[ "${_boost_oldest}" == "1.89" ]]; then
      source+=(
        "${_0_8_24_1_src}"
        "${_0_8_24_1_sig_src}"
      )
      sha256sums+=(
        "${_0_8_24_1_sum}"
        "${_0_8_24_1_sig_sum}"
      )
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${url}/archive/${_commit}.${_archive_format}"
        _sum="SKIP"
      elif [[ "${_tag_name}" == "pkgver" ]]; then
        _uri="${url}/releases/download/v${pkgver}/${_tarname}.tar.gz"
        _sum="${_github_release_sum}"
        sha512sums=(
          "${_github_release_sha512_sum}"
        )
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
if [[ -v "_src" ]]; then
  source+=(
    "${_src}"
  )
  sha256sums+=(
    "${_sum}"
  )
fi
validpgpkeys=(
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)

_git_unbundle() {
  local \
    _tarname="${1}" \
    _bundle \
    _repo \
    _msg=()
  _bundle="${srcdir}/${_tarname}.bundle"
  _repo="${srcdir}/${_tarname}"
  _msg=(
    "Cloning '${_bundle}' into '${_repo}'."
  )
  msg \
    "${_msg[*]}"
  git \
    clone \
      "${_bundle}" \
      "${_repo}" || \
  git \
    -C \
      "${_repo}" \
      pull || \
  true
}

_git_unbundle_update() {
  local \
    _repo="${1}" \
    _bundle="${2}" \
    _repo \
    _msg=()
  _bundle_name="$(
    basename \
      "${_bundle}")"
  _msg=(
    "Updating '${_repo}' from '${_bundle}'."
  )
  msg \
    "${_msg[*]}"
  git \
    -C \
      "${_repo}" \
      remote \
        add \
          "${_bundle_name}" \
	  "${_bundle}" ||
  true
  git \
    -C \
      "${_repo}" \
    pull \
      "${_bundle_name}" || \
  true
}

prepare() {
  local \
    _commit_hash
  if [[ "${_evmfs}" == "true" ]]; then
    if [[ "${_git}" == "false" ]]; then
      ur \
        "${_like}"
    elif [[ "${_git}" == "true" ]]; then
      _git_unbundle \
        "${_tarname}"
      if [[ "${_boost_oldest}" == "1.89" ]]; then
        _git_unbundle_update \
          "${srcdir}/${_tarname}" \
          "${srcdir}/${_pkg}-${_0_8_24_1_commit}"
      fi
    fi
  fi
  if [[ ! -e "${srcdir}/${_tarname}/commit_hash.txt" ]]; then
    _commit_hash="${_0_8_24_commit:0:8}"    
    echo \
      "${_commit_hash}" > \
      "${srcdir}/${_tarname}/commit_hash.txt"

  fi
}

_bin_get() {
  local \
    _cc \
    _bin
  _cc="$(
    command \
      -v \
      "cc" \
      "g++" \
      "clang++" | \
      head \
        -n \
          1)"
  _bin="$(
    dirname \
      "${_cc}")"
  echo \
    "${_bin}"
}

_verlte() {
  printf \
    '%s\n' \
    "${1}" \
    "${2}" | \
    sort \
      -C \
      -V
}

_verlt() {
  ! _verlte \
      "${2}" \
      "${1}"
}

_compile() {
  local \
    _tests="${1}" \
    _cmake_opts=() \
    _cc \
    _cxx \
    _cxx_compiler \
    _cxxflags=() \
    _flags=()
  _cc="$(
    command \
      -v \
      "${_compiler}")"
  _cxxflags=(
    "${CXXFLAGS}"
  )
  if [[ "${_os}" == "Android" ]]; then
    _cxxflags+=(
      -Wno-unused-but-set-variable
    )
  fi
  if [[ "${_boost_oldest}" != "1.89" ]]; then
      _cxxflags+=(
        -Wno-deprecated-declarations
      )
  if [[ "${_compiler}" == "gcc" ]]; then
    _cxx_compiler="g++"
  elif [[ "${_compiler}" == "clang" ]]; then
   _cxx_compiler="${_compiler}++"
  fi
  _cxx="$(
    command \
      -v \
      "${_cxx_compiler}")"
  _cmake_opts=(
    # --trace-expand 
    # -G
    #   "Ninja"
    -D
      CMAKE_BUILD_TYPE="Release"
    -D
      CMAKE_INSTALL_PREFIX="/usr/"
    -D
      CMAKE_EXECUTABLE_SUFFIX="${pkgver}"
    -D
      ONLY_BUILD_SOLIDITY_LIBRARIES="OFF"
    -D
      PEDANTIC="ON"
    -D
      PROFILE_OPTIMIZER_STEPS="OFF"
    -D
      SOLC_LINK_STATIC="OFF"
    -D
      SOLC_STATIC_STDLIBS="OFF"
    -D
      STRICT_NLOHMANN_JSON_VERSION="OFF"
    -D
      STRICT_Z3_VERSION="OFF"
    -D
      TESTS="${_tests}"
    -D
      USE_LD_GOLD="OFF"
    -D
      USE_SYSTEM_LIBRARIES="OFF"
    -S
      "${srcdir}/${_tarname}/"
    -Wno-dev
  )
  _flags+=(
    CC="${_cc}"
    CXX="${_cxx}"
    CXXFLAGS="${_cxxflags[*]}"
  )
  export \
    ${_flags[@]}
  ${_flags[@]} \
  cmake \
    -B \
      "${srcdir}/${_tarname}/build/" \
    "${_cmake_opts[@]}"
  ${_flags[@]} \
  cmake \
    --build \
      "${srcdir}/${_tarname}/build/" # \
    # 2>&1 > \
    # "${srcdir}/build.log"
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
  ls
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
  "${srcdir}/${_tarname}/build/test/soltest" \
    -p \
      true -- \
    --testpath \
      "${srcdir}/${_tarname}/test/"
  _compile \
    "OFF"
}

package() {
  local \
    _binaries=() \
    _bin
  _binaries=(
    "solc"
    "yul-phaser"
  )
  # Assure that the directories exist.
  mkdir \
    -p \
      "${pkgdir}/usr/share/doc/${pkgname}/"
  # Install the software.
  DESTDIR="${pkgdir}/" \
  cmake \
    --install \
      "${srcdir}/${_tarname}/build/"
  # Rename the binaries because
  # CMAKE_EXECUTABLE_SUFFIX doesn't work
  for _bin in "${_binaries[@]}"; do
    mv \
      "${pkgdir}/usr/bin/${_bin}" \
      "${pkgdir}/usr/bin/${_bin}${pkgver}" || \
      true
  done
  # Install the documentation.
  install \
    -Dm644 \
    "${srcdir}/${_tarname}/README.md" \
    "${pkgdir}/usr/share/doc/${pkgname}/"
  cp \
    -r \
    "${srcdir}/${_tarname}/docs/"* \
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
