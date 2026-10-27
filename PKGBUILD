# Maintainer: Your Name <you@example.com>

pkgname=glorious-core
pkgver=2.1.23
pkgrel=1
pkgdesc="Glorious CORE keyboard and mice configuration software (runs via Wine)"
arch=('x86_64')
url="https://www.gloriousgaming.com/"
install=glorious-core.install
license=('LicenseRef-custom')
depends=('wine' 'hicolor-icon-theme')
# Upstream's installer uses a newer Inno Setup than the official innoextract (1.9) supports
makedepends=('innoextract-crazy-max' 'unzip' 'icoutils')
optdepends=('winetricks: fixes for rendering/runtime issues in the Wine prefix')
options=('!strip' '!debug')
noextract=("GloriousCORE_${pkgver}_Setup.zip")
source=(
    "GloriousCORE_${pkgver}_Setup.zip::https://gloriouscore.nyc3.digitaloceanspaces.com/CORE2/app/GloriousCORE_${pkgver}_Setup.zip"
    "LICENSE"
    "glorious-core"
    "glorious-core.desktop"
    "70-glorious.rules"
)
sha256sums=('edfbd8d3fc778c26f24130527a2b188a694b8154ae3476c5105f4513db415aba'
            '84ee84c3c7977e215e5e5afdc22468d6115bd427eecc8b5f1bf6365cfae6163c'
            'e923312aa4b8b1b5a5004d563beb8d8d643c989f21d7efd4fffbc3d59e29968a'
            '0108a9504928c1658f058d04b77cd00da63dc100affcde5003e8f049eb034e93'
            '321fd28620e6fee8bdc8e3e663afbd4acd3fde4a3e0d14f2e1962dbff3307bbd')

prepare() {
    cd "$srcdir"

    unzip -o "GloriousCORE_${pkgver}_Setup.zip"

    rm -rf extract icons
    innoextract --output-dir extract "GloriousCORE_${pkgver}_Setup.exe"

    # Convert the Windows icon to PNGs for the hicolor theme
    mkdir icons
    icotool -x -o icons extract/app/icon.ico
}

package() {
    cd "$srcdir"

    local _dest="$pkgdir/usr/share/glorious-core"

    # Application files
    install -dm755 "$_dest"
    cp -a extract/app/. "$_dest"

    # Windows-only driver installers and the Windows self-updater
    rm -rf "$_dest/Driver"
    rm -f "$_dest/resources/public/v3-updater/"*.exe

    chmod 755 "$_dest/Glorious Core.exe"

    # Launcher, desktop entry, udev rules, license
    install -Dm755 glorious-core "$pkgdir/usr/bin/glorious-core"
    install -Dm644 glorious-core.desktop "$pkgdir/usr/share/applications/glorious-core.desktop"
    install -Dm644 70-glorious.rules "$pkgdir/usr/lib/udev/rules.d/70-glorious.rules"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

    # Icons (icotool names files like icon_1_256x256x32.png)
    local f size
    for f in icons/*.png; do
        size=$(grep -oE '[0-9]+x[0-9]+' <<< "${f##*/}" | head -n1)
        [[ -n $size ]] || continue
        install -Dm644 "$f" \
            "$pkgdir/usr/share/icons/hicolor/$size/apps/glorious-core.png"
    done
}
