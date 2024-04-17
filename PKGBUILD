# Maintainer: KosmX <kosmx dot mc at gmail dot com>
_canonical_name=lxd-ui

pkgname=incus-ui-canonical
pkgver=0.8
pkgrel=2
epoch=
pkgdesc="lxd-ui rebranded to incus"
arch=('i686' 'x86_64')
url="https://github.com/KosmX/incus-ui-canonical-arch" # There is no definite upstream for this.
license=('GPL3')
depends=('incus')
makedepends=('git' 'yarn' 'npm' 'rsync')
changelog=
source=("git+https://github.com/zabbly/incus.git"
        "https://github.com/canonical/lxd-ui/archive/refs/tags/${pkgver}.tar.gz"
        "incus.service"
)
sha256sums=(
    'SKIP'
    '237b88debd2f6d55c073864b600734f35fa0894246bee898994180e95347fac4'
    '760c221d5105eae80665fa48d4195b0e6bf2b72106cb03d8eea9e4ffafa81411'
)
noextract=()
validpgpkeys=()
backup=('etc/systemd/system/incus.service')

prepare() {
    REPO="${PWD}"/incus

    #https://github.com/zabbly/incus/blob/75f9f3de023f13d1d54e133fb2ea60de8a0c3006/.github/workflows/builds.yml#L306
    cd "$_canonical_name-$pkgver"
    
    git init # https://github.com/KosmX/incus-ui-canonical-arch/issues/3
    
    git apply -p1 < "${REPO}/patches/ui-canonical-0001-Branding.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0002-Update-navigation.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0003-Update-certificate-generation.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0004-Remove-external-links.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0005-Remove-Canonical-image-servers.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0006-Remove-version-check.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0007-Improve-openfga.patch"
    sed -i -f "${REPO}/patches/ui-canonical-renames.sed" src/*/*.ts* src/*/*/*.ts* src/*/*/*/*.ts*

}

build() {
    cd "$_canonical_name-$pkgver"
    yarn install
    yarn build
}


package() {
    pushd "$_canonical_name-$pkgver"

    mkdir -p "$pkgdir/opt/incus/ui-canonical/"
    rsync -a build/ui/ "$pkgdir/opt/incus/ui/"
    popd
    mkdir -p "$pkgdir/etc/systemd/system/"
    cp incus.service "$pkgdir/etc/systemd/system/"
}
