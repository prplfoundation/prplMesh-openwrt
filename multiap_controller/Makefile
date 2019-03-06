#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=multiap_controller
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/TechnicolorEDGM/multiap_controller
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
	DEPENDS:=+@KERNEL_POSIX_MQUEUE +libpthread +libuv +multiap_libplatform +ieee1905
	MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
endef

define Package/$(PKG_NAME)/description
   prplMesh / Technicolor Multiap controller
endef

ifeq ($(CONFIG_BIG_ENDIAN),y)
ENDIAN=big
else
ENDIAN=little
endif

define Build/Compile
        $(call Build/Compile/Default,PLATFORM="openwrt" CONTROLLER_VERSION=$(PKG_VERSION) ENDIANNESS="$(ENDIAN)" ROAMING_ENGINE="default")
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/multiap_controller $(1)/usr/bin
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
