dnl Process this file with autoconf to produce a configure script.
AC_PREREQ(2.53)

AC_INIT([tango-icon-theme], [0.0.0],
        [https://bugs.freedesktop.org/enter_bug.cgi?product=tango])
AC_CONFIG_SRCDIR([index.theme.in.in])

AM_INIT_AUTOMAKE([1.9 tar-ustar])

PKG_PROG_PKG_CONFIG([0.19])
IT_PROG_INTLTOOL([0.40.0])

GETTEXT_PACKAGE="${PACKAGE}"
AC_SUBST(GETTEXT_PACKAGE)

ALL_LINGUAS=""
AM_GLIB_GNU_GETTEXT

# AM_GLIB_GNU_GETTEXT above subst. $DATADIRNAME
# this is the directory where the *.{mo,gmo} files are installed
localedir='${prefix}/${DATADIRNAME}/locale'
AC_SUBST(localedir)

# Workaround to make aclocal get the right flags
AC_SUBST(ACLOCAL_AMFLAGS, "\${ACLOCAL_FLAGS}")

# Define the toplevel path here
AC_SUBST(themedir, "\${datadir}/icons/Tango")

# Icon sizes we want to install
AC_SUBST([icon_sizes], [8x8 16x16 22x22 32x32 48x48])

# Allow icon mapping to be disabled
AC_ARG_ENABLE([icon-mapping],
	AC_HELP_STRING([--enable-icon-mapping],
			[Enable compatibility symlinks [default=auto]]),
	enable_mapping=$enableval,
	enable_mapping=yes)

ICONMAP="true"
if test "x$enable_mapping" != "xno"; then
   UTILS_REQUIRED=0.8.7

   AC_MSG_CHECKING([icon-naming-utils >= $UTILS_REQUIRED])
   PKG_CHECK_EXISTS(icon-naming-utils >= $UTILS_REQUIRED,
                    have_utils=yes, have_utils=no)
   if test "x$have_utils" = "xyes"; then
      UTILS_PATH="`$PKG_CONFIG --variable=program_path icon-naming-utils`"
      ICONMAP="$UTILS_PATH/icon-name-mapping"
      AC_MSG_RESULT([yes])
   else
      AC_MSG_RESULT([no])
      AC_MSG_ERROR([icon-naming-utils >= $UTILS_REQUIRED is required to build
                    and install gnome-icon-theme])
   fi
fi
AC_SUBST(ICONMAP)

# PERL and Inkscape for render-pixmaps.pl
AC_ARG_ENABLE([rendering],
	AC_HELP_STRING([--disable-rendering], [disable icon rendering]),
	[enable_rendering=$enableval], [enable_rendering=yes])

if test "x$enable_rendering" = "xyes"; then
   AC_PATH_PROG([INKSCAPE], [inkscape])
   AC_PATH_PROG([PERL], [perl])
   if test -z "`$PERL -v | fgrep 'v5.' 2> /dev/null`" -a -z "`$PERL -v | fgrep 'version 5.' 2> /dev/null`"; then
      AC_MSG_RESULT([perl 5.x required for render-pixmaps.pl])
   fi
   AC_MSG_CHECKING([for XML::Parser])
   if `$PERL -e "require XML::Parser" 2>/dev/null`; then
      AC_MSG_RESULT([ok])
   else
	AC_MSG_RESULT([XML::Parser perl module is required for render-pixmaps.pl])
   fi
fi
AM_CONDITIONAL(ENABLE_RENDERING, test "x$enable_rendering" = "xyes")


AC_CONFIG_FILES([
Makefile
po/Makefile.in
svg/Makefile
])

AC_OUTPUT
