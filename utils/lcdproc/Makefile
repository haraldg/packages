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
PKG_VERSION:=0.5.8
PKG_RELEASE:=1

# Use this for official releasees
PKG_MD5SUM:=d01cc4eab61d0c08a870b08f7dde417822d3301c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)rc1.tar.gz
PKG_SOURCE_URL:=https://github.com/lcdproc/lcdproc/releases/download/v$(PKG_VERSION)rc1/

# Use this to test versions still under development
#PKG_SOURCE_PROTO:=git
#PKG_SOURCE_URL:=https://github.com/lcdproc/lcdproc.git
#PKG_SOURCE_VERSION:=9f5641c99ca08d1276b528da9970aa34100a3f09
#PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.gz
#PKG_FIXUP:=autoreconf

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
  $(call Package/lcdproc/Default)
  TITLE:=LCD Display server
endef

define LCDPROC_CORE_DRIVERS_TEXT
bayrad CFontzPacket CFontz CwLnx ea65 EyeboxOne glk icp_a106 imonlcd imon irtrans joy lb216 lcdm001
lcterm linux_input MD8800 ms6931 mtc_s16209x MtxOrb NoritakeVFD Olimex_MOD_LCD1x9 pyramid
rawserial serialPOS serialVFD sli SureElec text tyan vlsys_m428 yard2LCD
endef

LCDPROC_CORE_DRIVERS = $(strip $(LCDPROC_CORE_DRIVERS_TEXT))

define Package/lcdproc-server/description
$(call Package/lcdproc/Default-description)

This package contains the server and a core set of display drivers
without external dependencies:
$(LCDPROC_CORE_DRIVERS_TEXT)
endef

define Package/lcdproc-server/conffiles
/etc/LCDd.conf
endef


define Package/lcdproc-drivers
  $(call Package/lcdproc/Default)
  TITLE:=LCD Display extra drivers
  DEPENDS:=+lcdproc-server +libncurses +libusb +libftdi +@GPIO_SUPPORT:libugpio
endef

define LCDPROC_OTHER_DRIVERS_TEXT
curses futaba glcd hd44780 IOWarrior picolcd shuttleVFD
endef

LCDPROC_OTHER_DRIVERS = $(strip $(LCDPROC_OTHER_DRIVERS_TEXT))

define Package/lcdproc-drivers/description
$(call Package/lcdproc/Default-description)

This package contains display drivers with external dependencies:
$(LCDPROC_OTHER_DRIVERS_TEXT)
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
	$(CP) $(foreach driver,$(LCDPROC_CORE_DRIVERS),$(PKG_BUILD_DIR)/server/drivers/$(driver).so) $(1)/usr/lib/lcdproc
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/LCDd.conf $(1)/etc/
	$(INSTALL_BIN) ./files/LCDd $(1)/etc/init.d
endef

define Package/lcdproc-drivers/install
	$(INSTALL_DIR) $(1)/usr/lib/lcdproc
	$(CP) $(foreach driver,$(LCDPROC_OTHER_DRIVERS),$(PKG_BUILD_DIR)/server/drivers/$(driver).so) $(1)/usr/lib/lcdproc
endef


$(eval $(call BuildPackage,lcdproc-clients))
$(eval $(call BuildPackage,lcdproc-server))
$(eval $(call BuildPackage,lcdproc-drivers))
