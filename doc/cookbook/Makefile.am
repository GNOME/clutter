include $(top_srcdir)/build-aux/autotools/Makefile.am.silent

SUBDIRS = examples

NULL =

HTML_DIR = $(datadir)/gtk-doc/html
TARGET_DIR = $(HTML_DIR)/clutter-cookbook

XML_FILES = \
	actors.xml		\
	animations.xml		\
	events.xml		\
	introduction.xml	\
	text.xml		\
	textures.xml		\
	layouts.xml		\
	script.xml		\
	effects.xml		\
	$(NULL)

XSLTOPTS = \
	--stringparam html.stylesheet style.css 		\
	--stringparam chapter.autolabel 1 			\
	--stringparam appendix.autolabel 1 			\
	--stringparam section.autolabel 1 			\
	--stringparam gtkdoc.bookname "clutter-cookbook" 	\
	--stringparam gtkdoc.version @CLUTTER_API_VERSION@ 	\
	--stringparam callout.graphics 0			\
	--path $(top_srcdir)/doc/cookbook			\
	--xinclude

XSL_BASE_URI  = http://docbook.sourceforge.net/release/xsl/current
XSL_XHTML_URI = $(XSL_BASE_URI)/xhtml/docbook.xsl

HTML_FILES = $(top_builddir)/doc/cookbook/html/*.html
CSS_FILES = $(top_builddir)/doc/cookbook/html/*.css
IMAGE_FILES = \
	images/clutter-logo.png \
	images/effects-basic.png \
	images/effects-basic-background.png \
	images/effects-built-in.png \
	images/effects-custom-deform.png \
	images/effects-custom-deform-back-material.png \
	images/textures-reflection.png \
	images/actors-opacity.png \
	images/actors-opacity-container-affects-opacity.png \
	images/text-shadow.png \
	images/textures-sub-texture.png \
	images/layouts-stacking-diff-actor-sizes.png \
	images/events-pointer-motion-stacking.png	\
	images/layouts-bind-constraint-stage.png	\
	$(NULL)

VIDEO_FILES = \
	videos/animations-fading-out.ogv \
	videos/animations-fading-in-then-out.ogv \
	videos/animations-path.ogv \
	videos/animations-rotating-x-minus-45.ogv \
	videos/animations-rotating-y-45.ogv \
	videos/animations-rotating-z-90.ogv \
	videos/animations-rotating-x-minus-180-with-y-minus-96.ogv \
	videos/animations-rotating-x-minus-180-with-z-minus-96.ogv \
	videos/animations-rotating-x-centered.ogv \
	videos/animations-rotating-y-centered.ogv \
	videos/animations-rotating-z-centered.ogv \
	videos/animations-rotating-container-reverses-direction.ogv \
	videos/effects-custom-deform.ogv \
	videos/textures-split-go.ogv \
	videos/events-mouse-scroll.ogv \
	videos/textures-crossfade-two-textures.ogv \
	videos/animations-complex.ogv \
	videos/animations-reuse.ogv \
	videos/animations-moving-anchors.ogv \
	videos/animations-moving-depth.ogv \
	videos/animations-looping.ogv \
	$(NULL)

EXTRA_DIST = \
	clutter-cookbook.xml.in \
	common.xsl		\
	cookbook.xsl		\
	devhelp.xsl		\
	html.xsl		\
	recipe-template.xml	\
	ref-html-style.xsl	\
	style.css		\
	$(IMAGE_FILES) \
	$(XML_FILES) \
	$(VIDEO_FILES)

CLEANFILES = \
	pdf-build.stamp \
	html-build.stamp \
	clutter-cookbook.html

pdf-build.stamp: clutter-cookbook.xml $(XML_FILES)
	SP_ENCODING=XML SP_CHARSET_FIXED=YES \
	$(JW) -b pdf $(top_builddir)/doc/cookbook/clutter-cookbook.xml \
	&& mv -f clutter-cookbook.pdf clutter-cookbook-@CLUTTER_API_VERSION@.pdf \
	&& echo timestamp > $(@F)

html-build.stamp: clutter-cookbook.xml $(XML_FILES)
	if [ ! -d html ] ; then mkdir html ; fi && \
	if [ ! -d html/images ] ; then mkdir html/images ; fi && \
	if [ ! -d html/videos ] ; then mkdir html/videos ; fi && \
	if [ ! -d html/examples ] ; then mkdir html/examples ; fi && \
	$(XSLTPROC) $(XSLTOPTS) -o clutter-cookbook.html $(XSL_XHTML_URI) $(top_builddir)/doc/cookbook/clutter-cookbook.xml && \
	$(XSLTPROC) $(XSLTOPTS) -o html/ cookbook.xsl $(top_builddir)/doc/cookbook/clutter-cookbook.xml && \
	cp $(top_srcdir)/doc/cookbook/style.css html/ && \
	if [[ "$(VIDEO_FILES)" != "" ]] ; then \
		for file in `ls $(VIDEO_FILES)`; do \
			cp $$file html/videos/ ; \
		done \
	fi && \
	cp $(top_srcdir)/doc/cookbook/images/* html/images/ && \
	cp $(top_srcdir)/doc/cookbook/examples/*.c html/examples/ && \
	echo timestamp > $(@F)

if ENABLE_PDFS
#pdf_target = pdf-build.stamp
pdf_target =
else
pdf_target =
endif

if ENABLE_DOCS
all-local: html-build.stamp $(pdf_target)
else
all-local:
endif

clean-local:
	@rm -rf html ; \
	rm -f *.pdf ; \
	rm -f *.stamp

dist-hook:
	mkdir $(distdir)/html
	cp -r ./html/* $(distdir)/html

uninstall-local:
	@rm -rf $(DESTDIR)$(TARGET_DIR)
	@rm -f $(DESTDIR)$(TARGET_DIR)/clutter-cookbook.devhelp

install-data-local:
	installfiles=`echo $(top_builddir)/doc/cookbook/html/*`; \
	if test "$$installfiles" = '$(top_builddir)/doc/cookbook/html/*'; \
	then echo '-- Nothing to install' ; \
	else \
	  $(mkinstalldirs) $(DESTDIR)$(TARGET_DIR) ; \
	  for file in `ls $(HTML_FILES) $(CSS_FILES)`; do \
	    if [ -f $$file ]; then \
	      basefile=`echo $$file | sed -e 's,^.*/,,'`; \
	      $(INSTALL_DATA) $$file $(DESTDIR)$(TARGET_DIR)/$$basefile; \
	    fi \
	  done; \
	fi; \
	if [ -d $(top_srcdir)/doc/cookbook/images ]; \
	then \
	  $(mkinstalldirs) $(DESTDIR)$(TARGET_DIR)/images ; \
	  for file in `ls $(IMAGE_FILES)`; do \
	    if [ -f $$file ]; \
	    then \
	      basefile=`echo $$file | sed -e 's,^.*/,,'`; \
	      $(INSTALL_DATA) $$file $(DESTDIR)$(TARGET_DIR)/images/$$basefile; \
	    fi \
	  done; \
	fi; \
	if [ -d $(top_srcdir)/doc/cookbook/videos ]; \
	then \
	  $(mkinstalldirs) $(DESTDIR)$(TARGET_DIR)/videos ; \
	  for file in `ls $(VIDEO_FILES)`; do \
	    if [ -f $$file ]; \
	    then \
	      basefile=`echo $$file | sed -e 's,^.*/,,'`; \
	      $(INSTALL_DATA) $$file $(DESTDIR)$(TARGET_DIR)/videos/$$basefile; \
	    fi \
	  done; \
	fi; \
	$(INSTALL_DATA) $(top_builddir)/doc/cookbook/html/clutter-cookbook.devhelp $(DESTDIR)$(TARGET_DIR)/clutter-cookbook.devhelp

.PHONY : doc

COOKBOOK_BASE_PATH = /home/clutter/web/docs.clutter-project.org/docs/clutter-cookbook
COOKBOOK_VERSION = $(CLUTTER_API_VERSION)
COOKBOOK_PATH = $(COOKBOOK_BASE_PATH)/$(COOKBOOK_VERSION)

publish: html-build.stamp
	$(QUIET_CP)scp -r html/* clutter@clutter-project.org:$(COOKBOOK_PATH)

.PHONY : publish
