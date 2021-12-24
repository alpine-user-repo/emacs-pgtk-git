_pkgname=emacs-feature-pgtk
pkgname=emacs-pgtk-git
provides='emacs'
pkgver='_git'
pkgrel=0
pkgdesc="The extensible, customizable, self-documenting real-time display editor"
arch="all !s390x !mips !mips64"
url="https://www.gnu.org/software/emacs/emacs.html"
license="GPL-3.0-or-later"
depends="hicolor-icon-theme wayland desktop-file-utils"
makedepends="autoconf automake linux-headers gawk
	librsvg-dev giflib-dev libxpm-dev gtk+3.0-dev alsa-lib-dev
	glib-dev fontconfig-dev libpng-dev cairo-dev texinfo
	libxml2-dev pango-dev tiff-dev libjpeg-turbo-dev ncurses-dev
	ncurses-libs gnutls-dev libxaw-dev gmp-dev harfbuzz-dev jansson-dev"
	
source="https://github.com/emacs-mirror/emacs/archive/feature/pgtk.zip
        alloc.patch"
builddir="$srcdir"/"$_pkgname"
prepare() {
    	cd "$builddir"
        default_prepare
	./autogen.sh	
}

build() {
	cd "$builddir"
	CFLAGS=-fno-pie \
	LDFLAGS=-no-pie \
	./configure \
		--build=$CBUILD \
		--host=$CHOST \
		--prefix=/usr \
		--sysconfdir=/etc \
		--libexecdir=/usr/lib \
		--localstatedir=/var \
		--without-makeinfo \
		--with-gameuser=:games \
		--with-gpm \
		--with-harfbuzz \
		--with-json \
		--with-pgtk \
		--with-cairo \
		--with-xft \
		--with-tiff=no \
		--with-jpeg=yes
	make
}

package() {
	mkdir -p "$pkgdir"
	make DESTDIR="$pkgdir" install

	mv "$pkgdir"/usr/bin/ctags "$pkgdir"/usr/bin/ctags.emacs
	rm -rf "$pkgdir"/usr/share/info \
		"$pkgdir"/usr/share/man

	find "$pkgdir"/usr/share/emacs/ -exec chown root:root {} \;
	find "$pkgdir"/usr/lib -perm -g+s,g+x ! -type d -exec chmod g-s {} \;
	# fix perms on /var/games
	chmod 775 "$pkgdir"/var/games
	chmod 775 "$pkgdir"/var/games/emacs
	chmod 664 "$pkgdir"/var/games/emacs/*
	chown -R root:games "$pkgdir"/var/games
}

sha512sums="
6e3f367c9d47d78603b4f6d1199f762587fc02cc4e00dcb3b69f069ffda7aeba2c6c4bee50b5971ca09ad3650d972209ceca4cf1b396687af9fa0e2485eb5029  pgtk.zip
2ecb19c5038dc98b8e1e974104e9d268345e3eb4e18668253c9ea083e146b6e664ff912cb70660e1e4caff7c87a58d49c3423749eb014526aab8a871597650cf  alloc.patch
"
