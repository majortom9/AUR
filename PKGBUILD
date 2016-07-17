# Maintainer: Tomasz Maciej Nowak <com[dot]gmail[at]tmn505>
# Contributor: Evangelos Foutras <evangelos@foutrelis.com>
# Contributor: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Thayer Williams <thayer@archlinux.org>
# Contributor: Hugo Doria <hugo@archlinux.org>
# Contributor: TuxSpirit <tuxspirit@archlinux.fr>
# Contributor: Daniel J Griffiths <ghost1227@archlinux.us>

pkgname=p7zip-gui
pkgver=16.02
pkgrel=1
pkgdesc='Graphic user interface (alpha quality) for the 7zip file archiver'
url='http://p7zip.sourceforge.net/'
license=('custom:unRAR' 'LGPL')
arch=('i686' 'x86_64')
depends=('p7zip' 'wxgtk')
optdepends=('desktop-file-utils: desktop entries')
makedepends=('python' 'webkitgtk2')
makedepends_i686=('nasm')
makedepends_x86_64=('yasm')
options=(!makeflags)
install='p7zip-gui.install'
source=("http://downloads.sourceforge.net/project/p7zip/p7zip/${pkgver}/p7zip_${pkgver}_src_all.tar.bz2"
        '7zFM.desktop')
sha256sums=('5eb20ac0e2944f6cb9c2d51dd6c4518941c185347d4089ea89087ffdd6e2341f'
            '8cb662ccbacd1badc2c41ff00618c53d1c7fb8bca5472cca4ac7bd7f619acb27')

prepare() {
	cd ${srcdir}/p7zip_${pkgver}
	if [[ ${CARCH} = x86_64 ]]; then
		cp makefile.linux_amd64_asm makefile.machine
	else
		cp makefile.linux_x86_asm_gcc_4.X makefile.machine
	fi
	sed -i 's/x86_64-linux-gnu//g' CPP/7zip/*/*/*.depend
	rm GUI/kde4/p7zip_compress.desktop # FS#43766
	cd Utils
	sed -i 's/_do_not_use//g' generate.py
	./generate.py
}

build() {
	cd ${srcdir}/p7zip_${pkgver}
	make 7zFM 7zG OPTFLAGS="${CFLAGS}"
}

package() {
	cd ${srcdir}/p7zip_${pkgver}
	make install \
		DEST_DIR="${pkgdir}" \
		DEST_HOME="/usr" \
		DEST_MAN="/usr/share/man"

	# remove files provided by p7zip package
	rm -fR ${pkgdir}/usr/lib/p7zip/{7z.so,Codecs}
	rm -R ${pkgdir}/usr/share/{doc,man}

	install -Dm644 GUI/p7zip_32.png ${pkgdir}/usr/share/icons/hicolor/32x32/apps/p7zip.png
	install -d ${pkgdir}/usr/share/{applications,kde4/services/ServiceMenus}
	cp GUI/kde4/* ${pkgdir}/usr/share/kde4/services/ServiceMenus
	install -d ${pkgdir}/usr/share/kservices5/ServiceMenus
	cp GUI/kde4/* ${pkgdir}/usr/share/kservices5/ServiceMenus
	cp ../7zFM.desktop ${pkgdir}/usr/share/applications
	ln -s 7zCon.sfx ${pkgdir}/usr/lib/p7zip/7z.sfx
	# disabled due FS#49303
	#install -d ${pkgdir}/usr/share/doc/p7zip/DOC/MANUAL
	#cp -r GUI/help/fm ${pkgdir}/usr/share/doc/p7zip/DOC/MANUAL
	chmod +x ${pkgdir}/usr/bin/p7zipForFilemanager
}
