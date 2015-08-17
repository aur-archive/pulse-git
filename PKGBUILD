#!/bin/bash
# Maintainer: Odd Stråbø <oddstr13 at openshell dot no>

pkgname=pulse-git
pkgver=0.1.1.0.g61b44ca
pkgrel=6
pkgdesc="Synchronise your files without also sharing them with a stranger in the cloud. A fork of Syncthing."
arch=('i686' 'x86_64' 'armv7h' 'armv6h')
url="https://ind.ie/pulse/"
license=('GPL3')
depends=("bash")
optdepends=(
    "xmllib2: Allows application menu item to work even with changed GUI host:port."
)

provides=("pulse=${pkgver}-${pkgrel}")

conflicts=("pulse")

source=(
    "${pkgname}::git+https://source.ind.ie/project/pulse.git"
    "https://ind.ie/assets/images/pulse-logo.svg"
    "pulse@.service"
    "pulse-git.install"
    "pulse-gui.sh"
)
install=pulse-git.install

md5sums=(
    "SKIP"
    "dec2bd9210df7f85700e89211c1b70e8"
    "24311856130d27c553c9b66f395282dc"
    "853a70ba027382ec222be6767701d013"
    "b62fc8e6644a071184e57257bd9c1167"
)

sha1sums=(
    "SKIP"
    "2da4b7689ad70e823ff2e286a8d6af18524f9116"
    "1a5650dcc86456f7f1cc0fea8f2b8407d1e37c1b"
    "7f92812168800f0781c7ca1921f2ed6750c42c23"
    "c2538fb6e387656f0e14e5b847d742afda879cbf"
)

sha256sums=(
    "SKIP"
    "75e8f1ee6554d7dffe5b5e57538f7f866242e5f47e6b0b70b9a8e3277def813c"
    "36b919300086bec9be5b8e394039dcf435b9f5717ef36f31f2c97cdc836e9184"
    "03b5d3db0925249c965b5c7caa3da365e143f63068573cf8d75882ec9764c03e"
    "b122a49a104c8957a4617dd847d2294e828769436f722a6104243d25988018a7"
)

makedepends=('git' 'go' 'gendesk')

case "$CARCH" in
  "i686")
    _goarch="386"
    ;;
  "x86_64")
    _goarch="amd64"
    ;;
  "armv7h")
    _goarch="armv7"
    ;;
  "armv6h")
    _goarch="armv6"
    ;;
esac

_pkgver() {
    cd "${srcdir}/${pkgname}"
    git describe --tags --long
}

pkgver() {
    _pkgver | sed -r 's/-/./g'
}

build() {
    gendesk -f -n --pkgname "$pkgname" --pkgdesc "$pkgdesc" --name "Pulse" --terminal=false --categories="Network;Application;" --exec "/usr/bin/pulse-gui"

    mkdir -p "${srcdir}/source.ind.ie/project/"

    if [ -d "${srcdir}/source.ind.ie/project/pulse" ]; then
        rm -rf "${srcdir}/source.ind.ie/project/pulse"
    fi

    cp -r "${srcdir}/${pkgname}" "${srcdir}/source.ind.ie/project/pulse"

    cd "${srcdir}/source.ind.ie/project/pulse"

    go run build.go -goarch "${_goarch}" -version "$(_pkgver)" build
}

package() {
    install -Dm644 pulse@.service "${pkgdir}/usr/lib/systemd/system/pulse@.service"
    install -Dm755 pulse-gui.sh "${pkgdir}/usr/bin/pulse-gui"

    mkdir -p "${pkgdir}/usr/bin"

    cd "${srcdir}/source.ind.ie/project/pulse"

    go run build.go -goarch "${_goarch}" -version "$(_pkgver)" install

    cp -v ./bin/pulse "${pkgdir}/usr/bin/pulse"

    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"

    install -Dm644 README.md "${pkgdir}/usr/share/doc/pulse/README.md"
    install -Dm644 protocol/PROTOCOL.md "${pkgdir}/usr/share/doc/pulse/protocol/PROTOCOL.md"
    install -Dm644 protocol/DISCOVERY.md "${pkgdir}/usr/share/doc/pulse/protocol/DISCOVERY.md"


    cd "${srcdir}"
    install -Dm644 "$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"
    install -Dm644 "pulse-logo.svg" "$pkgdir/usr/share/pixmaps/$pkgname.svg"

    #echo "what?"
    #exit 1
}

