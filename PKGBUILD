# Maintainer: KosmX <kosmx dot mc at gmail dot com>

pkgname=incus-ui-canonical
pkgver=0.14
pkgrel=1
epoch=
pkgdesc="lxd-ui rebranded to incus"
arch=('i686' 'x86_64')
url="https://github.com/KosmX/incus-ui-canonical-arch" # There is no definite upstream for this.
license=('GPL-3.0-only')
makedepends=('git' 'yarn' 'npm')
changelog=
source=(
    "lxd-ui-${pkgver}.tar.gz::https://github.com/canonical/lxd-ui/archive/refs/tags/${pkgver}.tar.gz"
    "incus-zabbly::git+https://github.com/zabbly/incus.git#branch=daily"
    "incus-override-ui.conf"
)
sha256sums=('e54e681d1ae40b90a57df00b126deaf085b1697154db442d9eaa1980a26e091e'
            'SKIP'
            '9df2976d3f4397d90c9ddc4ecf71195cf8c6fa13b280342ffa9b62ce36a54673')

prepare() {
    cd "lxd-ui-${pkgver}"

    # https://github.com/zabbly/incus/blob/75f9f3de023f13d1d54e133fb2ea60de8a0c3006/.github/workflows/builds.yml#L306
    GIT_DIR= git apply "${srcdir}/incus-zabbly/patches"/ui-canonical-*.patch
    find src -type f -name '*.ts*' -exec sed -f "${srcdir}/incus-zabbly/patches/ui-canonical-renames.sed" -i {} \+
}

build() {
    cd "lxd-ui-${pkgver}"
    yarn install
    yarn build
}

package() {
    depends+=(incus)

    install -Dm644 \
        "${srcdir}/incus-override-ui.conf" \
        "${pkgdir}/usr/lib/systemd/system/incus.service.d/ui.conf"

    install -dm755 "${pkgdir}/opt/incus/ui"
    cp -dR --preserve=timestamps "lxd-ui-${pkgver}/build/ui" -T "${pkgdir}/opt/incus/ui"
}
