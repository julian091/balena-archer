# Maintainer: Julian Kalman <kalmanjulian08@gmail.com>

pkgname=balena-etcher
_pkgname=etcher
pkgver=1.7.9
pkgrel=1
epoch=2
pkgdesc='Flash OS images to SD cards & USB drives, safely and easily'
arch=('x86_64' 'i686' 'armv7h' 'aarch64')
_github_url='https://github.com/balena-io/etcher'
url='https://balena.io/etcher'
license=(Apache)
depends=("electron" "gtk3" "libxtst" "libxss" "nss" "alsa-lib" "glib2" "polkit" "libusb")
makedepends=("npm" "python" "git" "jq" "patch" "nodejs<17")
optdepends=("libnotify: for notifications")
conflicts=("${_pkgname}"
  "${_pkgname}-git"
  "${_pkgname}-bin"
)
options=('!strip')
source=("etcher::git+https://github.com/balena-io/${_pkgname}.git#tag=v${pkgver}"
        "${pkgname}-electron.sh"
        "${pkgname}-electron.desktop"
        )
sha256sums=('SKIP'
            '13651534055dd72dd622ff8ac3d56ee818008dfa1f680b5c4ad7f7ef14698800'
            'c950d9578f9cf60998c920bb60c6617559963f06a4918e7072fdc706b0ef5754')

prepare() {
  cd "${_pkgname}"
  git submodule init
  git submodule update || cd "${srcdir}/${_pkgname}/scripts/resin" && git checkout --
}

build() {
  cd "${_pkgname}"
  make electron-develop
  npm run webpack
  npm prune --production
}

package() {
  cd "${_pkgname}"

  _appdir="${pkgdir}/usr/lib/${pkgname}"
  install -d "${_appdir}"

  install package.json "${_appdir}"
  cp -a {lib,generated,node_modules} "${_appdir}"
  install -D assets/icon.png "${_appdir}/assets/icon.png"
  install -D lib/gui/app/index.html "${_appdir}/lib/gui/app/index.html"

  install -Dm755 "${srcdir}/${pkgname}-electron.sh" "${pkgdir}/usr/bin/${pkgname}-electron"
  install -Dm644 "${srcdir}/${pkgname}-electron.desktop" \
    "${pkgdir}/usr/share/applications/${pkgname}-electron.desktop"

  for size in 16x16 32x32 48x48 128x128 256x256 512x512; do
    install -Dm644 "assets/iconset/${size}.png" \
      "${pkgdir}/usr/share/icons/hicolor/${size}/apps/${pkgname}-electron.png"
  done

  find "${pkgdir}" -name package.json -print0 | xargs -r -0 sed -i '/_where/d'
}

# vim
