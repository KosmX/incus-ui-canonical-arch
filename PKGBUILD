# Maintainer: KosmX <kosmx dot mc at gmail dot com>

pkgname=incus-ui-canonical
pkgver=0.14
pkgrel=2
epoch=
pkgdesc="lxd-ui rebranded to incus"
arch=('i686' 'x86_64')
url="https://github.com/zabbly/incus-ui-canonical" # There is no definite upstream for this.
license=('GPL-3.0-only')
makedepends=('git' 'yarn' 'npm')
changelog=
source=(
    "incus-ui::git+https://github.com/zabbly/incus-ui-canonical.git"
    "incus-override-ui.conf"
)
sha256sums=('SKIP'
            'fc1d6927fd984770d729641aabc25069f3fcca389d4f1ca813a17a33e27f7fba')

prepare() {
    cd "incus-ui"
    yarn install

}


build() {
    cd "incus-ui"
    yarn build
}

package() {
    depends+=(incus)

    install -Dm644 \
        "${srcdir}/incus-override-ui.conf" \
        "${pkgdir}/usr/lib/systemd/system/incus.service.d/ui.conf"

    install -dm755 "${pkgdir}/usr/share/incus-ui"
    cp -dR --preserve=timestamps "incus-ui/build/ui" -T "${pkgdir}/usr/share/incus-ui"
}
