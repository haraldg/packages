#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# No copyright by Harald Geyer as filling in a template doesn't constitute
# an original work in the sense of copyright law.

include $(TOPDIR)/rules.mk

PKG_MAINTAINER:=Harald Geyer <harald@ccbib.org>

PKG_NAME:=lcdproc
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING
PKG_VERSION:=0.5dev
PKG_RELEASE:=1

# Use this for official releasees
#PKG_MD5SUM:=62fe0fbf9ee57ffaa58a982f602f596a
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=

# Use this to test versions still under development
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/haraldg/lcdproc.git
PKG_SOURCE_VERSION:=f4c5141560949ac7a4433461214f5e07129374b9
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/lcdproc/Default
  SECTION:=utils
  CATEGORY:=Utilities
  URL:=http://lcdproc.org/
endef

define Package/lcdproc/Default-description
LCDProc is a daemon and clients for displaying system information
on various displays.
endef


define Package/lcdproc-clients
  $(call Package/lcdproc/Default)
  TITLE:=LCD Display clients
endef

define Package/lcdproc-clients/description
$(call Package/lcdproc/Default-description)

This package contains the clients distributed by the official lcdproc
project:
* lcdproc -- displays system information
* lcdexec -- displays a menu structure to execute commands
* lcdvc -- shows the content of the system console
endef

define Package/lcdproc-clients/conffiles
/etc/lcdproc.conf
/etc/lcdexec.conf
/etc/lcdvc.conf
endef


define Package/lcdproc-server
  $(call Package/liblcdproc/Default)
  TITLE:=LCD Display server
endef

define Package/lcdproc-server/description
$(call Package/lcdproc/Default-description)

This package contains the server and a core set of display drivers
without external dependencies.
endef

define Package/lcdproc-server/conffiles
/etc/LCDd.conf
endef


define Package/lcdproc-drivers
  $(call Package/lcdproc/Default)
  TITLE:=LCD Display extra drivers
  DEPENDS:=+lcdproc-server +libncurses +libusb +libftdi
endef

define Package/lcdproc-drivers/description
$(call Package/lcdproc/Default-description)

This package contains display drivers with external dependencies.
endef


define Build/Configure
	$(call Build/Configure/Default,--disable-libX11 --disable-libhid --enable-drivers=all)
endef

define Package/lcdproc-clients/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/clients/lcdproc/lcdproc $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/clients/lcdexec/lcdexec $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/clients/lcdvc/lcdvc $(1)/usr/bin/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/clients/lcdproc/lcdproc.conf $(1)/etc/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/clients/lcdexec/lcdexec.conf $(1)/etc/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/clients/lcdvc/lcdvc.conf $(1)/etc/
	$(INSTALL_BIN) ./files/lcd* $(1)/etc/init.d
endef

define Package/lcdproc-server/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/usr/lib/lcdproc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/server/LCDd $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/server/drivers/linux_input.so $(1)/usr/lib/lcdproc/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/LCDd.conf $(1)/etc/
	$(INSTALL_BIN) ./files/LCDd $(1)/etc/init.d
endef

define Package/lcdproc-drivers/install
	$(INSTALL_DIR) $(1)/usr/lib/lcdproc
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/server/drivers/*.so $(1)/usr/lib/lcdproc/
endef


$(eval $(call BuildPackage,lcdproc-clients))
$(eval $(call BuildPackage,lcdproc-server))
$(eval $(call BuildPackage,lcdproc-drivers))
