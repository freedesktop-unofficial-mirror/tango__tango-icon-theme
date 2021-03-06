DISTCHECK_CONFIGURE_FLAGS = --disable-rendering --disable-icon-mapping

SUBDIRS = po svg

theme_in_files = index.theme.in.in
theme_DATA = $(theme_in_files:.theme.in.in=.theme)

THEME_DIRS=$(shell SIZES="$(render_sizes)"; for size in $$SIZES; do for dir in `find $$size/* -type d`; do printf "$$dir,"; done; done | sed -e "s/,$$//g")

%.theme.in: %.theme.in.in Makefile
	for size in $(render_sizes); do \
		make -C svg $$size; \
	done
	dirs="$(THEME_DIRS)"; \
	sed -e "s|\@THEME_DIRS\@|$$dirs|g" < $< > $@; \
	for dir in `echo $$dirs | sed -e "s/,/ /g"`; do \
		sizefull="`dirname $$dir`"; \
		size="`echo $$sizefull | sed -e 's/x.*$$//g'`"; \
		context="`basename $$dir`"; \
		echo "[$$dir]" >> $@; \
		echo "Size=$$size" >> $@; \
		if test "$$context" = "actions"; then \
			echo "Context=Actions" >> $@; \
		fi; \
		if test "$$context" = "animations"; then \
			echo "Context=Animations" >> $@; \
		fi; \
		if test "$$context" = "apps"; then \
			echo "Context=Applications" >> $@; \
		fi; \
		if test "$$context" = "categories"; then \
			echo "Context=Categories" >> $@; \
		fi; \
		if test "$$context" = "devices"; then \
			echo "Context=Devices" >> $@; \
		fi; \
		if test "$$context" = "emblems"; then \
			echo "Context=Emblems" >> $@; \
		fi; \
		if test "$$context" = "emotes"; then \
			echo "Context=Emotes" >> $@; \
		fi; \
		if test "$$context" = "intl"; then \
			echo "Context=International" >> $@; \
		fi; \
		if test "$$context" = "mimetypes"; then \
			echo "Context=MimeTypes" >> $@; \
		fi; \
		if test "$$context" = "places"; then \
			echo "Context=Places" >> $@; \
		fi; \
		if test "$$context" = "status"; then \
			echo "Context=Status" >> $@; \
		fi; \
		echo "Type=Threshold" >> $@; \
		echo "" >> $@; \
	done

@INTLTOOL_THEME_RULE@

render_in_files = render-bitmaps.pl.in
render_script = $(render_in_files:.pl.in=.pl)

if ENABLE_RENDERING
%.pl: %.pl.in
	sed -e "s#\@PERL\@#$(PERL)#g" \
	-e "s#\@INKSCAPE\@#$(INKSCAPE)#g" < $< > $@
	chmod +x $@
endif

# we don't want to install mo files, all translations are already stored
# in theme files
install-data-local:
	$(MAKE) -C po uninstall
	if [ -h $(DESTDIR)$(themedir) ]; then \
		rm -f $(DESTDIR)$(themedir); \
	fi
	if [ -d $(DESTDIR)$(themedir) ]; then \
		touch $(DESTDIR)$(themedir); \
	fi

EXTRA_DIST=			\
	$(theme_in_files)	\
	$(render_in_files)

CLEANFILES = 			\
	$(theme_DATA)		\
	$(render_script)

MAINTAINERCLEANFILES =			\
	Makefile.in			\
	aclocal.m4			\
	config.guess			\
	config.h.in			\
	config.sub			\
	configure			\
	depcomp				\
	install-sh			\
	intltool-*.in			\
	libtool				\
	ltmain.sh			\
	missing				\
	mkinstalldirs			\
	po/Makefile.in.in		\
	po/$(GETTEXT_PACKAGE).pot
