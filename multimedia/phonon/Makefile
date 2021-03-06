# $OpenBSD: Makefile,v 1.21 2016/12/25 14:34:39 zhuk Exp $

COMMENT =	multimedia layer for Qt4/KDE4
CATEGORIES =	multimedia devel
VERSION =	4.9.1
DISTNAME =	phonon-${VERSION}
HOMEPAGE =	http://phonon.kde.org/

# The phonon_s* are for KDE4-only; the phonon4qt5* are Qt5-only
# Renamed to avoid clashing with Qt4's one
# XXX major should be tweaked on any update, to unbreak
# IMPORTED_LINK_INTERFACE_LIBRARIES CMake feature.
SHARED_LIBS =	phonon_s		3.0
SHARED_LIBS +=	phonon_sexperimental	2.0
SHARED_LIBS +=	phonon4qt5		1.0
SHARED_LIBS +=	phonon4qt5experimental	1.0

MAINTAINER =	KDE porting team <openbsd-kde@googlegroups.com>

# LGPL v2.1
PERMIT_PACKAGE_CDROM =	Yes

MASTER_SITES =		${MASTER_SITE_KDE:=stable/phonon/${VERSION}/}
EXTRACT_SUFX =		.tar.xz

FLAVORS =		qt5
FLAVOR ?=

WANTLIB =		m pulse pulse-mainloop-glib
LIB_DEPENDS =		audio/pulseaudio

.if ${FLAVOR:Mqt5}
MODULES =		x11/qt5 \
			devel/kf5
LIB_DEPENDS +=		x11/qt5/qtdeclarative
PHONON_INCLUDE_DIR =	include/phonon4qt5
PKGNAME =		phonon-qt5-${VERSION}
FULLPKGNAME =		${PKGNAME}
WANTLIB +=		Qt5Core Qt5DBus Qt5Gui Qt5Widgets
.else
MODULES =		x11/kde4
MODKDE4_USE =
MODKDE4_LIB_LINKS =	Yes
PHONON_INCLUDE_DIR =	${MODKDE4_INCLUDE_DIR}
WANTLIB +=		QtDBus QtGui
.endif

# Clashes with the one provided by Qt itself.
CONFIGURE_ARGS +=	-DPHONON_BUILD_DESIGNER_PLUGIN:Bool=No \
			-DCMAKE_POLICY_DEFAULT_CMP0017=NEW \
			-DPHONON_INSTALL_QT_COMPAT_HEADERS=No

.if ${FLAVOR:Mqt5}
CONFIGURE_ARGS +=	-DPHONON_BUILD_PHONON4QT5=Yes \
			-DINCLUDE_INSTALL_DIR=include/phonon4qt5
.else
CONFIGURE_ARGS +=	-DBACKEND_INSTALL_DIR="${PREFIX}/lib/kde4/plugins/phonon_backend"
.endif

# Upstream says, tests need to be redone.
NO_TEST =		Yes

SUBST_VARS =		PHONON_INCLUDE_DIR

.include <bsd.port.mk>
