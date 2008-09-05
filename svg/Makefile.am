
icon_in_files =
icon_files = $(icon_in_files:.svg=.png)

if ENABLE_RENDERING
$(top_builddir)/render-bitmaps.pl:
	(cd $(top_builddir) && make render-bitmaps.pl)

$(icon_sizes):
	for size in $(icon_sizes); do \
		for file in $(icon_in_files); do \
			$(top_builddir)/render-bitmaps.pl -s $$size -o $(top_builddir) $(srcdir)/$$file; \
		done; \
	done

dist-hook: $(top_builddir)/render-bitmaps.pl
	for size in $(icon_sizes); do \
		for file in $(icon_in_files); do \
			$(top_builddir)/render-bitmaps.pl -s $$size -o $(top_builddir) $(srcdir)/$$file; \
		done; \
		cp -a $(top_builddir)/$$size $(distdir)/$$size; \
	done
else
dist-hook:
	for size in $(icon_sizes); do \
		for file in $(icon_files); do \
			for context in $(top_builddir)/$$size/*; do \
				if [ ! -f $(top_builddir)/$$size/$$context/$$file ]; then \
					echo "ERROR: Icon $$file not rendered at size $$size."; \
					exit 1; \
				fi; \
			done; \
		done; \
		cp -a $(top_builddir)/$$size $(distdir)/$$size; \
	done
endif

install-data-local:
	for size in $(icon_sizes); do \
		for file in $(icon_files); do \
			for context in $(top_srcdir)/$$size/*; do \
				$(mkdir_p) $(DESTDIR)$(themedir)/$$size/$$context; \
				$(install_sh_DATA) $(top_srcdir)/$$size/$$context/$$file $(DESTDIR)$(themedir)/$$size/$$context/$$file; \
			done; \
		done; \
	done

uninstall-local:
	for size in $(icon_sizes); do \
		for file in $(icon_files); do \
			for context in $(top_srcdir)/$$size/*; do \
				rm -f $(DESTDIR)$(themedir)/$$size/$$context/$$file; \
			done; \
		done; \
	done

EXTRA_DIST =			\
	$(icon_in_files)

MAINTAINERCLEANFILES =		\
	Makefile.in
