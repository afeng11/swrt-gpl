# Convert swrt build environment variable to qsdk
PKG_BUILD_DIR=$(shell pwd)/source
STAGING_DIR=$(STAGEDIR)

INSTALL_HEADERS=bufrd.h \
	bufwr.h \
	cmd.h \
	csh.h \
	dbg.h \
	evloop.h \
	interface.h \
	split.h

HYFICOMMON_MAKEOPTS:= \
        TOOLPREFIX=$(CROSS_COMPILE) \
        GWINCLUDE=$(STAGING_DIR)/usr/include/qca/ \
        QCACFLAGS="$(TARGET_CFLAGS)"\
        QCALDFLAGS="-Wl,--gc-sections $(TARGET_LDFLAGS)"\
        INSTALL_ROOT=$(PKG_BUILD_DIR)/install

all:
ifneq ($(wildcard $(SDK_DIR_PLATFORM)/libhyficommon/*.c),)
	ln -sf $(SDK_DIR_PLATFORM)/libhyficommon source
	mkdir -p $(PKG_BUILD_DIR)/install
	mkdir -p $(PKG_BUILD_DIR)/install/include
	mkdir -p $(PKG_BUILD_DIR)/install/lib
	$(MAKE) -C $(PKG_BUILD_DIR) $(strip $(HYFICOMMON_MAKEOPTS))
else
	mkdir -p $(PKG_BUILD_DIR)
	cp prebuilt/libhyficommon.so $(PKG_BUILD_DIR)/libhyficommon.so
endif
	touch stamp-h1

install:
	@[ -d $(INSTALLDIR)/usr/lib ] || install -d $(INSTALLDIR)/usr/lib
	install -m 755 -D $(PKG_BUILD_DIR)/libhyficommon.so $(INSTALLDIR)/usr/lib
	@$(STRIP) $(INSTALLDIR)/usr/lib/libhyficommon.so

clean:

stage:
ifneq ($(wildcard $(SDK_DIR_PLATFORM)/libhyficommon/*.c),)
	@[ -d $(STAGEDIR)/usr/include/hyficommon ] || install -d $(STAGEDIR)/usr/include/hyficommon
	@for i in $(INSTALL_HEADERS); do install -m 755 -D $(PKG_BUILD_DIR)/install/include/$$i $(STAGEDIR)/usr/include/hyficommon ; done
	install -m 755 -D $(PKG_BUILD_DIR)/libhyficommon.so $(STAGEDIR)/usr/lib/
endif
