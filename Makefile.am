AUTOMAKE_OPTIONS = subdir-objects
SUBDIRS = 
EXTRA_DIST = pkg

dist_noinst_DATA = biosdevname.rules.in biosdevname.spec.fedora biosdevname.spec.suse biosdevname.1



CLEANFILES = version src/bios_dev_name.h
install-data-local:
	mkdir -p $(DESTDIR)@RULEDIR@
	$(INSTALL_DATA) $(top_srcdir)/biosdevname.rules.in $(DESTDIR)@RULEDEST@

uninstall-local:
	rm -f $(DESTDIR)@RULEDEST@

include src/Makefile.am

# extra space at beginning to work around automake... :(
 ifndef SPEC_FILE
 SPEC_FILE=biosdevname.spec.fedora
 endif
rpm: dist
	mkdir -p dist/{SOURCES,BUILD,RPMS,SRPMS,SPECS}
	cp $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz dist/SOURCES
	cp $(SPEC_FILE) dist/SPECS/@PACKAGE_NAME@.spec
	rpmbuild --define "_topdir $$PWD/dist" -ba --nodeps dist/SPECS/@PACKAGE_NAME@.spec ;
	rm -rf dist/{SOURCES,BUILD,SPECS}

srpm: dist
	mkdir -p dist/{SOURCES,BUILD,RPMS,SRPMS,SPECS}
	cp $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz dist/SOURCES
	cp $(SPEC_FILE) dist/SPECS/@PACKAGE_NAME@.spec
	rpmbuild --define "_topdir $$PWD/dist" -bs --nodeps dist/SPECS/@PACKAGE_NAME@.spec ;
	rm -rf dist/{SOURCES,BUILD,SPECS}

deb: dist
	oldpwd=`pwd` ; \
	tmp_dir=`mktemp -d /tmp/$(PACKAGE_NAME).XXXXXXXX` ; \
cp $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz $${tmp_dir}/$(PACKAGE_NAME)_$(PACKAGE_VERSION).orig.tar.gz ; \
	tar -C $${tmp_dir} -xzf $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz ; \
	cd $${tmp_dir}/$(PACKAGE_NAME)-$(PACKAGE_VERSION) ; \
	mv pkg/debian debian ; \
	rmdir pkg/ ; \
	pdebuild --buildresult $$oldpwd/.. ; \
	cd - ;\
	rm -rf $${tmp_dir}

sdeb: dist
	oldpwd=`pwd` ; \
	tmp_dir=`mktemp -d /tmp/biosdevname.XXXXXXXX` ; \
	cp $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz $${tmp_dir}/$(PACKAGE_NAME)_$(PACKAGE_VERSION).orig.tar.gz ; \
	tar -C $${tmp_dir} -xzf $(PACKAGE_NAME)-$(PACKAGE_VERSION).tar.gz ; \
	mv $${tmp_dir}/$(PACKAGE_NAME)-$(PACKAGE_VERSION)/pkg/debian $${tmp_dir}/$(PACKAGE_NAME)-$(PACKAGE_VERSION)/debian ; \
	rmdir pkg/ ; \
	cd $${tmp_dir}/$(PACKAGE_NAME)-$(PACKAGE_VERSION) ; \
	dpkg-buildpackage -S -sa -rfakeroot -k92F0FC09 ; \
	mv ../biosdevname_* $$oldpwd/.. ; \
	cd - ;\
	rm -rf $${tmp_dir}
