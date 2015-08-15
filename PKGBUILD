# This is an example PKGBUILD file. Use this as a start to creating your own,
# and remove these comments. For more information, see 'man PKGBUILD'.
# NOTE: Please fill out the license field for your package! If it is unknown,
# then please put 'unknown'.

# See http://wiki.archlinux.org/index.php/VCS_PKGBUILD_Guidelines
# for more information on packaging from Mercurial(hg) sources.

# Creator: Andreas Amereller <andreas.amereller.dev@googlemail.com>
# Modified for HL2230 by: Nathan Hourt <nat.hourt@gmail.com>

printermodel=hl2230
upperprintermodel=HL2230

pkgname=brother-$printermodel
pkgver=2.0.4
pkgrel=3
pkgdesc="Brother HL-2230 CUPS driver"
arch=('i686' 'x86_64')
url="http://www.brother.com"
license=('custom:Brother Industries')
groups=()
depends=('glibc' 'cups')
makedepends=('rpmextract')
provides=()
conflicts=()
replaces=()
backup=()
options=()
install="$pkgname.install"
source=("$printermodel.patch"
  "http://www.brother.com/pub/bsc/linux/dlf/${printermodel}lpr-2.1.0-1.i386.rpm"
  "http://www.brother.com/pub/bsc/linux/dlf/cupswrapper$upperprintermodel-2.0.4-2.i386.rpm"
  "Brother-HL-2030-hl1250.ppd::https://www.openprinting.org/ppd-o-matic.php?driver=hl1250&printer=Brother-HL-2030&.submit=Generate+PPD+file"
  "$pkgname.install")
noextract=()
md5sums=('116304f633dfe8cdd97454e11c44a378'
         '2d5b95521e27339a3ebbc44fbcd8053d'
         'dc4656f74bc82b3647572132b4df9531'
         '13a2b9f9b5894664067fd9694f2fb9f7'
         '76f936d99e4169e3b764b8d1612ae430')

if [[ -z "$CARCH" ]]; then
  echo ":: PKGBUILD could not detect your architecture. Some dependencies may be missing"
else
  if [[ "$CARCH" == "x86_64" ]]; then
    depends=("${depends[@]}" 'lib32-glibc')
  fi
fi

build() {
  cd $srcdir || return 1
  for n in *.rpm; do
    rpmextract.sh $n || return 1
  done;
  patch -p0 < ../$printermodel.patch
  $srcdir/usr/local/Brother/Printer/$upperprintermodel/cupswrapper/cupswrapper$upperprintermodel-2.0.4
}

package() {
  mkdir -p $pkgdir/usr/share
  cp -R $srcdir/usr/local/Brother/Printer/$upperprintermodel $pkgdir/usr/share/brother
  rm $pkgdir/usr/share/brother/cupswrapper/cupswrapper$upperprintermodel-2.0.4
  rm $pkgdir/usr/share/brother/inf/setupPrintcap2
  install -m 644 -D $srcdir/Brother-HL-2030-hl1250.ppd $pkgdir/usr/share/cups/model/$upperprintermodel.ppd
  install -m 755 -D wrapper $pkgdir/usr/lib/cups/filter/brlpdwrapper$upperprintermodel
}

# vim:set ts=2 sw=2 et:
