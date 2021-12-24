pkgname=emacs-pgtk-git
provides='emacs'
pkgver='_git'
pkgrel=0
pkgdesc="The extensible, customizable, self-documenting real-time display editor"
arch="all !s390x !mips !mips64"
url="https://www.gnu.org/software/emacs/emacs.html"
license="GPL-3.0-or-later"
depends="hicolor-icon-theme wayland desktop-file-utils"
makedepends="git autoconf automake linux-headers gawk
	librsvg-dev giflib-dev libxpm-dev gtk+3.0-dev alsa-lib-dev
	glib-dev fontconfig-dev libpng-dev cairo-dev texinfo
	libxml2-dev pango-dev tiff-dev libjpeg-turbo-dev ncurses-dev
	ncurses-libs gnutls-dev libxaw-dev gmp-dev harfbuzz-dev jansson-dev"
	
source="alloc.patch"
_pkgname='emacs'
_repo="git://git.sv.gnu.org/emacs.git"
prepare() {
    git clone --depth=1 "${_repo}" "$srcdir/${_pkgname}"
    default_prepare
    cd "$srcdir/${_pkgname}"
    ./autogen.sh	
}

build() {
	cd "$srcdir/_pkgname"
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

