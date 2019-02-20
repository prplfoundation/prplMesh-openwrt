#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=multiap_libplatform
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/TechnicolorEDGM/libmultiap_platform
PKG_SOURCE_DATE:=2019-02-19
PKG_SOURCE_VERSION:=xxxx
PKG_MIRROR_HASH:=xxxx

PKG_LICENSE:=BSD-2-Clause-Patent
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=MultiAP Agent
	DEPENDS:=+libpthread +libuv +libuci +libc +jansson +libubus +libubox +transformer-tch
	MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
endef

define Package/$(PKG_NAME)/description
   prplMesh / Technicolor Multiap platform abstraction library
endef

define Build/Compile
	$(call Build/Compile/Default,PLATFORM="openwrt" FLAVOUR="openwrt")
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/libplatform_map.so $(1)/usr/lib/
endef


define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/libplatform_map.so $(1)/usr/lib/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
