# Maintainer: Jan Boelsche <jan@lagomorph.de>
# Contributor: Pavan RIkhi <pavan.rikhi@gmail.com>
# Contributor: Lukas Fleischer <lfleischer@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>

# NOTE: in this theme, the default cursor is completely transparent (invisible)
# This is useful on kiosk systems in case there's no other way to get rid of the cursor
# (all other cursors are unchanged)

# To Change the Colors of the Generated Cursor Files, Edit the FILL_COLOR &
# OUTLINE_COLOR Variables in the `build()` Function.

pkgname=adwaita-invisible-default-cursor
pkgver=3.30.1
pkgrel=2
pkgdesc="GNOME's Adwaita Theme with ivisible default cursor"
url="https://git.gnome.org/browse/adwaita-icon-theme"
arch=(any)
license=(LGPL3 CCPL:cc-by-sa)
depends=(hicolor-icon-theme gtk-update-icon-cache librsvg)
makedepends=(intltool git gtk3 gnome-common python2 python2-pillow inkscape xorg-xcursorgen imagemagick)
source=("https://github.com/GNOME/adwaita-icon-theme/archive/$pkgver.tar.gz")
sha256sums=('3b4a5e97f5583d7493cbba4ced00654c34d60006f4efed69be49f3fefadb982f')

_sourcefolder="adwaita-icon-theme-$pkgver"

prepare() {
  cd "$_sourcefolder"
  autoreconf -fvi
}
  
build() {
  cd "$_sourcefolder"

  # Some Colors from the Molokai colorscheme you can pick
  BACKGROUND="#1B1D1E"
  #BLACK="#232526"
  #WHITE="#F8F8F0"
  #RED="#FF0000"
  #ORANGE="#FD971F"
  #MAGENTA="#F92672"
  #CYAN="#66D9EF"
  _GREEN="#A6E22E"

  # Set Desired Filll & Outline Colors
  FILL_COLOR="${_GREEN}"
  OUTLINE_COLOR="${BACKGROUND}"

  # Replace the Colors in the Source SVG
  cd src/cursors/
  TEMP_VAR="42IPROBABLYDONTEXISTINTHESVGFILE9001"
  sed -i -e "s/#ffffff/${TEMP_VAR}/g"   \
         -e "s/#bebebe/${TEMP_VAR}/g"   \
         -e "s/#dedede/${TEMP_VAR}/g"   \
         adwaita.svg
  sed -i -e "s/#000000/${FILL_COLOR}/g" \
         -e "s/#484848/${FILL_COLOR}/g" \
         -e "s/#313131/${FILL_COLOR}/g" \
         adwaita.svg
  sed -i "s/${TEMP_VAR}/${OUTLINE_COLOR}/g" adwaita.svg

  # Generate PNGs from the SVG
  CORE_COUNT="$(grep -c '^core id' /proc/cpuinfo)"
  ./renderpngs.py -r -s -t -o -m 32 -a -c -n "${CORE_COUNT}" adwaita.svg

  # Generate XCursor files from the PNGS
  cd pngs
  mkdir originals
  mv 24x24 32x32 48x48 64x64 96x96 originals/   # Not sure why these are still black...
  cp -r 24x24_s1 24x24
  cp -r 32x32_s1 32x32
  cp -r 48x48_s1 48x48
  cp -r 64x64_s1 64x64
  cp -r 96x96_s1 96x96

  # overwrite standard cursor with fully transparent png
  # NOTE: for some reason the directory names are misleading!
  convert -size 32x32 xc:none 24x24/left_ptr.png
  convert -size 48x48 xc:none 32x32/left_ptr.png
  convert -size 64x64 xc:none 48x48/left_ptr.png
  convert -size 96x96 xc:none 64x64/left_ptr.png
  convert -size 128x128 xc:none 96x96/left_ptr.png

  ./make.sh
  cd ../../..

  # Now build the Theme
  ./configure --prefix=/usr
  make
}

package() {
  cd "$_sourcefolder"
  make DESTDIR="$pkgdir" install
  mv ${pkgdir}/usr/share/icons/{Adwaita,Adwaita-hidden-cursor}
  rm -rf ${pkgdir}/usr/share/pkgconfig
}
