# Maintainer: KosmX <kosmx dot mc at gmail dot com>
_canonical_name=lxd-ui

pkgname=incus-ui-canonical
pkgver=0.12
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
    'a82779ba405bda6bf0c704edbd3c153a7719197383928512e8a9519650662849'
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
    patch -p1 -i "${REPO}/patches/ui-canonical-0007-Improve-OpenFGA-support.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0008-Update-keys-that-aren-t-VM-specific.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0009-Fix-cluster-evacuation.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0010-Rename-user.ui_title-to-user.ui.title.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0011-Add-user.uid.sso_only.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0012-Skip-LXD-identity-API.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0013-Fix-network-forward-count-logic.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0014-Hide-Cluster-option-when-not-clustered.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0015-Add-optional-location-column.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0016-Make-migration-an-action.patch"
    patch -p1 -i "${REPO}/patches/ui-canonical-0017-Wait-for-projects-list-to-be-loaded.patch"
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
