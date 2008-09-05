
icon_in_files = $(shell for i in `find . -name "*.svg"`; do printf "$$i "; done)
icon_files = $(icon_in_files:.svg=.png)

if ENABLE_RENDERING
$(top_builddir)/render-bitmaps.pl: $(top_srcdir)/render-bitmaps.pl.in
	(cd $(top_builddir) && make render-bitmaps.pl)

%.png: %.svg $(top_builddir)/render-bitmaps.pl
	for size in $(render_sizes); do \
		$(top_builddir)/render-bitmaps.pl -s $$size -o $(top_builddir) $<; \
	done
	touch $@

dist-hook: $(icon_files)
	for size in $(render_sizes); do \
		cp -a $(top_builddir)/$$size $(top_distdir)/$$size; \
	done
else
%.png: %.svg
	touch $@

dist-hook: $(icon_files)
	for size in $(render_sizes); do \
		cp -a $(top_srcdir)/$$size $(top_distdir)/$$size; \
	done
endif

all: $(icon_files)
$(render_sizes): $(icon_files)

install-data-local:
	for size in $(render_sizes); do \
		for file in $(icon_files); do \
			for dir in `find $(top_srcdir)/$$size/* -type d`; do \
				context="`basename $$dir`"; \
				$(mkdir_p) $(DESTDIR)$(themedir)/$$size/$$context; \
				$(install_sh_DATA) $(top_srcdir)/$$size/$$context/$$file $(DESTDIR)$(themedir)/$$size/$$context/$$file; \
			done; \
		done; \
	done

uninstall-local:
	for size in $(render_sizes); do \
		for file in $(icon_files); do \
			for dir in `find $(top_srcdir)/$$size/* -type d`; do \
				context="`basename $$dir`"; \
				rm -f $(DESTDIR)$(themedir)/$$size/$$context/$$file; \
			done; \
		done; \
	done

EXTRA_DIST =			\
	$(icon_in_files)

CLEANFILES =			\
	$(icon_files)

MAINTAINERCLEANFILES =		\
	Makefile.in
