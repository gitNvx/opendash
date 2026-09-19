# Maintainer: Jotalea <main@jotalea.com.ar>
pkgname=opendash
pkgver=0.1.0
pkgrel=1
pkgdesc='OpenDash, an open-source Geometry Dash-like game (SDL3 port)'
arch=('x86_64' 'aarch64')
url='https://github.com/gitNvx/opendash'
license=('unknown')
depends=('sdl3' 'sdl3_mixer' 'mpg123' 'json-c' 'curl' 'zlib')
makedepends=('git' 'python' 'pkgconf')
# Builds from the local checkout (committed state only; commit before building).
# Bump pkgrel for each rebuild you want pacman to see as an upgrade.
source=("$pkgname::git+file://$startdir")
sha256sums=('SKIP')

build() {
  cd "$pkgname"
  make
}

package() {
  cd "$pkgname"

  # The game loads assets relative to its working directory (romfs/ and
  # platform/sdl/assets_build/) and writes saves to ./sdl_fs, so the launcher
  # runs it from a per-user data dir that links to the shared assets.
  install -Dm755 output/gd3ds-sdl "$pkgdir/usr/lib/$pkgname/$pkgname"

  install -d "$pkgdir/usr/share/$pkgname/platform/sdl"
  cp -r romfs "$pkgdir/usr/share/$pkgname/romfs"
  cp -r platform/sdl/assets_build "$pkgdir/usr/share/$pkgname/platform/sdl/assets_build"

  install -Dm644 resources/icon.png "$pkgdir/usr/share/pixmaps/$pkgname.png"

  install -Dm755 /dev/stdin "$pkgdir/usr/bin/$pkgname" <<'LAUNCHER'
#!/bin/sh
data="${XDG_DATA_HOME:-$HOME/.local/share}/opendash"
share=/usr/share/opendash
mkdir -p "$data/platform/sdl"
ln -sfn "$share/romfs" "$data/romfs"
ln -sfn "$share/platform/sdl/assets_build" "$data/platform/sdl/assets_build"
cd "$data" || exit 1
exec /usr/lib/opendash/opendash "$@"
LAUNCHER

  install -Dm644 /dev/stdin "$pkgdir/usr/share/applications/$pkgname.desktop" <<DESKTOP
[Desktop Entry]
Type=Application
Name=OpenDash
Comment=$pkgdesc
Exec=$pkgname
Icon=$pkgname
Categories=Game;ArcadeGame;
DESKTOP
}
