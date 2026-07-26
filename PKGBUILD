# Maintainer: Your Name <you@example.com>

pkgname=glorious-core
pkgver=2.1.17
pkgrel=1
pkgdesc="Glorious CORE keyboard and mice configuration software"
arch=('x86_64')
url="https://www.gloriousgaming.com/"
license=('custom')
install=glorious-core.install
depends=('wine')
makedepends=('innoextract' 'unzip')
source=(
    "GloriousCORE_${pkgver}_Setup.zip::https://gloriouscore.nyc3.digitaloceanspaces.com/CORE2/app/GloriousCORE_${pkgver}_Setup.zip"
    LICENSE
    "glorious-core"
    "glorious-core.desktop"
    "99-gmmk3.rules"
)
sha256sums=('9202873ee95abf5014df6825be297632c8aba7eff1d201a2cdd4b82f4d3d18ab'
            '4e62cdc8876ed5103892d06e640f5742af0dfba0db32666b00f2747a636adc51'
            'be748fe4b439ea46fd23d344e806e1c8c63b35f49a0f5cb9e20a9cb497eb951c'
            'bd8fca819c8f104acbd4664cde357e25e42f712bed41358e0722451e2465199e')

options=(!strip)

prepare() {
    cd "$srcdir"

    unzip -o "GloriousCORE_${pkgver}_Setup.zip"

    rm -rf extract

    innoextract \
        --output-dir extract \
        "GloriousCORE_${pkgver}_Setup.exe"
}

package() {
    install -dm755 "$pkgdir/usr/share/glorious-core"

    cp -a extract/app/. \
        "$pkgdir/usr/share/glorious-core"

    rm -rf "$pkgdir/usr/share/glorious-core/Driver"

    rm -f \
    "$pkgdir/usr/share/glorious-core/resources/public/v3-updater/"*.exe

    # Windows-only drivers
    rm -rf "$pkgdir/usr/share/glorious-core/Driver"

    chmod +x \
        "$pkgdir/usr/share/glorious-core/Glorious Core.exe"

    install -Dm755 \
        "$srcdir/glorious-core" \
        "$pkgdir/usr/bin/glorious-core"

    install -Dm644 \
        "$srcdir/glorious-core.desktop" \
        "$pkgdir/usr/share/applications/glorious-core.desktop"

    install -Dm644 \
        "$srcdir/99-gmmk3.rules" \
        "$pkgdir/usr/lib/udev/rules.d/99-gmmk3.rules"

    install -Dm644 \
        "$pkgdir/usr/share/glorious-core/icon.ico" \
        "$pkgdir/usr/share/pixmaps/glorious-core.ico"
}
