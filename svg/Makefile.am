
icon_in_files = $(shell SRCDIR="$(srcdir)"; for i in `find $$SRCDIR -name "*.svg"`; do printf "$$i "; done)
icon_cache = $(icon_in_files:.svg=.rendercache)
icon_files = $(shell BUILDDIR="$(builddir)"; for i in `find $$BUILDDIR -name "*.rendercache"`; do cat $$i; done)

if ENABLE_RENDERING
$(top_builddir)/render-bitmaps.pl: $(top_srcdir)/render-bitmaps.pl.in
	(cd $(top_builddir) && make render-bitmaps.pl)

%.rendercache: %.svg $(top_builddir)/render-bitmaps.pl
	for size in $(render_sizes); do \
		$(top_builddir)/render-bitmaps.pl -s $$size -o $(top_builddir) $< > $@; \
	done

dist-hook: $(icon_cache)
	for size in $(render_sizes); do \
		cp -a $(top_builddir)/$$size $(top_distdir)/$$size; \
	done
else
%.rendercache: %.svg

dist-hook: $(icon_cache)
	for size in $(render_sizes); do \
		cp -a $(top_srcdir)/$$size $(top_distdir)/$$size; \
	done
endif

all: $(icon_cache)
$(render_sizes): $(icon_cache)

install-data-local:
	for size in $(render_sizes); do \
		for dir in `if [ -d $(top_srcdir)/$$size ]; then find $(top_srcdir)/$$size/* -type d; fi`; do \
			for file in $(icon_files); do \
				context="`dirname $$file`"; \
				$(mkdir_p) $(DESTDIR)$(themedir)/$$size/$$context; \
				$(install_sh_DATA) $(top_srcdir)/$$size/$$file $(DESTDIR)$(themedir)/$$size/$$file; \
			done; \
		done; \
	done

uninstall-local:
	for size in $(render_sizes); do \
		for dir in `if [ -d $(top_srcdir)/$$size ]; then find $(top_srcdir)/$$size/* -type d; fi`; do \
			for file in $(icon_files); do \
				rm -f $(DESTDIR)$(themedir)/$$size/$$file; \
			done; \
		done; \
	done

EXTRA_DIST =			\
	$(icon_in_files)	\
	$(icon_cache)


MAINTAINERCLEANFILES =		\
	Makefile.in		\
	$(icon_cache)
