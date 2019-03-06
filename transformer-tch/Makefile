#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=transformer-tch
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/dirkfeytons/transformer
PKG_SOURCE_DATE:=2019-02-19
PKG_SOURCE_VERSION:=10c3370fe9c1c4a77a55106adb11ef2169f94f84
# PKG_MIRROR_HASH:=xxxx

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/transformer-tch
	SECTION:=transformer
	CATEGORY:=Technicolor
	SUBMENU:=Management
	TITLE:=Transformer
	DEPENDS:=+lua +luafilesystem +lua +libubus-lua +lsqlite3 +libopenssl +luabitop
	MAINTAINER:=Technicolor <linuxgw@technicolor.com>
endef

define Package/transformer-cli-tch
	SECTION:=transformer
	CATEGORY:=Technicolor
	SUBMENU:=Management
	TITLE:=Transformer CLI
	DEPENDS:=+lua +luabitop
	MAINTAINER:=Technicolor <linuxgw@technicolor.com>
endef

define Package/transformer-tch/description
  A daemon written in Lua for providing TR-069 datamodels as defined in TR-106 (IGD, Device:2, ...).
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/include/libtransformer.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libtransformer.so $(1)/usr/lib/
endef

define Package/transformer-tch/install
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/libtransformer.so $(1)/usr/lib
endef

$(eval $(call BuildPackage,transformer-tch))
