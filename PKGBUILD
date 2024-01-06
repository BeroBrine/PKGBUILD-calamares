#built by abhishek from snippets from aur version such as patches

pkgname=calamares
pkgver=3.3.0
pkgrel=0.1
pkgdesc="Distribution-independent installer framework"
arch=('x86_64')
license=("GPL")
url="https://github.com/calamres/calamares"
depends=('kconfig5' 'kcoreaddons5' 'kiconthemes5' 'ki18n5' 'kio5' 'solid5' 'yaml-cpp' 'kpmcore>=22.04.0' 'mkinitcpio-openswap'
         'boost-libs' 'ckbcomp' 'hwinfo' 'qt5-svg' 'polkit-qt5' 'gtk-update-icon-cache' 'plasma-framework5'
         'qt5-xmlpatterns' 'squashfs-tools' 'libpwquality' 'appstream-qt5' 'icu' 'python' 'qt5-webview'
         'ttf-comfortaa')
makedepends=('extra-cmake-modules' 'qt5-tools' 'qt5-translations' 'git' 'boost' 'kparts5' 'kdbusaddons5')
backup=('usr/share/calamares/modules/bootloader.conf'
        'usr/share/calamares/modules/displaymanager.conf'
        'usr/share/calamares/modules/initcpio.conf'
        'usr/share/calamares/modules/unpackfs.conf')

source=(
	"$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
	"calamares.desktop"
	"calamares_polkit"
	"49-nopasswd-calamares.rules"
	"paru-support.patch"
	"flag.patch"
	)

sha256sums=(
	'252f0097e3191ffc557b022f34ef23d24b939f1141efd483db0ab1ee9dc0fb76'
	'b9e65ab87f69b2d3f8f8eaea60c78625aef57dd336314ab75389f31a447445be'
	'c176b28007bd1c1f23d8dbb2c936fa54d0c01bacfb67290ddad597606c129df3'
	'56d85ff6bf860b9559b8c9f997ad9b1002f3fccc782073760eca505e3bddd176'
	'f00c90bd87d6dfd73b3ec53fa9a145ac25234676be41604807f05f05a4bf5bbb'
	'0830c8fe57c94a63ef87b6a025eb729b4f098a9e46e729b63415f4d3a2755762'
	)

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}" || return 
  sed -i 's/"Install configuration files" OFF/"Install configuration files" ON/' "${srcdir}/${pkgname}-${pkgver}/CMakeLists.txt"
  sed -i "s|\${CALAMARES_VERSION_MAJOR}.\${CALAMARES_VERSION_MINOR}.\${CALAMARES_VERSION_PATCH}|${pkgver}-${pkgrel}|g" CMakeLists.txt
  sed -i "s|CALAMARES_VERSION_RC 1|CALAMARES_VERSION_RC 0|g" CMakeLists.txt
  git apply --verbose ../paru-support.patch

}

build() {
	cd "${srcdir}/${pkgname}-${pkgver}" || return
	mkdir -p build
	cd build || return
	cmake .. \
		-DCMAKE_BUILD_TYPE=Release \
		-DCMAKE_INSTALL_PREFIX=/usr \
		-DCMAKE_INSTALL_LIBDIR=lib \
		-DWITH_QT"${qt}"=ON \
		-DWITH_PYTHONQT=OFF \
		-DWITH_KF5DBus=OFF \
		-DBoost_NO_BOOST_CMAKE=ON \
		-DWEBVIEW_FORCE_WEBKIT=OFF \
		-DSKIP_MODULES="webview \
						tracking \
						interactiveterminal \
						initramfs \
						initramfscfg \
						dracut \
						dracutlukscfg \
						dummyprocess \
						dummypython \
						dummycpp \
						dummypythonqt \
						services-openrc \
						keyboardq \
						localeq \
						welcomeq"
	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}/build" || return
	make DESTDIR="$pkgdir" install
	install -Dm644 "${srcdir}/calamares.desktop" "$pkgdir/etc/xdg/autostart/calamares.desktop"
	install -Dm755 "${srcdir}/calamares_polkit" "$pkgdir/usr/bin/calamares_polkit"
	install -Dm644 "${srcdir}/49-nopasswd-calamares.rules" "$pkgdir/etc/polkit-1/rules.d/49-nopasswd-calamares.rules"
	chmod 750 "$pkgdir"/etc/polkit-1/rules.d
}
