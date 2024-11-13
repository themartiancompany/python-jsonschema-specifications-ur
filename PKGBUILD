# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: George Rawlinson <grawlinson@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_Pkg=jsonschema
_pkg="${_Pkg}-specifications"
pkgname="${_py}-${_pkg}"
pkgver=2023.12.1
pkgrel=2
_pkgdesc=(
  'The JSON Schema meta-schemas'
  'and vocabularies, exposed as a Registry'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_http="https://github.com"
_ns="${_py}-${_Pkg}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}>=${_pynextver}"
  "${_py}-referencing"
)
makedepends=(
  'git'
  "${_py}-build"
  "${_py}-installer"
  "${_py}-hatchling"
  "${_py}-hatch-vcs"
)
checkdepends=(
  'python-pytest'
)
_commit='544e0ff86850af1c6d9e533c4b58b76c59542a76'
source=(
  "${pkgname}::git+${url}#commit=${_commit}"
)
b2sums=(
  'SKIP'
)

pkgver() {
  cd "${pkgname}"
  # https://github.com/python-jsonschema/jsonschema-specifications/issues/14
  git \
    describe \
    --tags | \
    sed \
      -e \
        's/^v//' \
      -e \
        's:\.0:.:'
}

build() {
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      build \
    --wheel \
    --no-isolation
}

check() {
  cd \
    "${pkgname}"
  PYTHONPATH="$(pwd)" \
  pytest \
    -v
}

package() {
  local \
    _site_packages
  _site_packages=$( \
    "${_py}" \
      -c \
        "import site; print(site.getsitepackages()[0])")
  cd \
    "${pkgname}"
  "${_py}" \
    -m \
      installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  # symlink license file
  install \
    -d \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "${site_packages}/jsonschema_specifications-${pkgver}.dist-info/licenses/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/COPYING"
}
