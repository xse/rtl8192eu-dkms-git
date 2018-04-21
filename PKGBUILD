# Maintainer: xse <183953+xse@users.noreply.github.com>
# TODO : use pkgver(), .SRCINFO and so on

pkgname=rtl8192eu-dkms-git
__pkgname=${pkgname%-*}
_pkgname=${__pkgname%-*}
pkgver=4.4.1
pkgrel=1
pkgdesc="Kernel module for Realtek8192eu wireless thingy (dkms)"
arch=("x86_64" "armv6h") # Untested on armv7h and so on, should work tho.
url="https://github.com/Mange/rtl8192eu-linux-driver"
license=('GPL')
depends=("dkms")
makedepends=('linux-headers' 'git')
source=("git+https://github.com/Mange/rtl8192eu-linux-driver.git" "fix.patch" "dkms.conf" "blacklist-rtl8xxxu.conf")
sha256sums=("SKIP" "582f8e8f8bd513b598ada3a1ac625f188fbf313cddf018a440ca4cd8aaf9964c" "99a9d58760579afe27fc95643fbb1962926d0691f8a4879f1b86c31109193ec4" "dc6a9bfc6a796461da2219accc7a6ae755ea13253737630e1538f3d98aa7aff5")
install="$_pkgname.install"

prepare() {
	cd "rtl8192eu-linux-driver"
  	patch Makefile<../fix.patch
	if grep -q 'model name.*ARM' /proc/cpuinfo; then 
		sed -i -e 's/CONFIG_PLATFORM_I386_PC = y/CONFIG_PLATFORM_I386_PC = n/' -e 's/CONFIG_PLATFORM_ARM_RPI = n/CONFIG_PLATFORM_ARM_RPI = y/' Makefile
	fi
}

package() {
  cd "${srcdir}"

  local install_dir="${pkgdir}/usr/src/${_pkgname}-${pkgver}"

  # dkms.conf
  install -Dm644 dkms.conf "${install_dir}/dkms.conf"

  install -Dm644 blacklist-rtl8xxxu.conf "${pkgdir}/etc/modprobe.d/rtl8192eu.conf"

  # modify dkms.conf
  sed -e "s/@_PKGNAME@/${_pkgname}/" \
      -e "s/@PKGVER@/${pkgver}/" \
      -i "${install_dir}/dkms.conf"

  # move sources
  cd "rtl8192eu-linux-driver"

  for d in $(find . -type d)
  do
    install -dm755 "${install_dir}/$d"
  done

  for f in $(find . -type f)
  do
    install -m644 "$f" "${install_dir}/$f"
  done
}

