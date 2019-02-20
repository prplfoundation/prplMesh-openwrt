#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ieee1905
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/TechnicolorEDGM/ieee1905
PKG_SOURCE_DATE:=2019-02-19
PKG_SOURCE_VERSION:=xxxx
PKG_MIRROR_HASH:=xxxx

PKG_LICENSE:=BSD-2-Clause-Patent
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=IEEE 1905 Broadband Forum implementation with Multi-AP extensions
	DEPENDS:=+@KERNEL_POSIX_MQUEUE +libpthread +libpcap +libopenssl +librt +multiap_libplatform
	MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
endef

define Package/prplmesh/description
 The IEEE 1905.1 protocol is a mechanism for home devices to discover each
 other and communicate, so that they can use optimal paths for transmission.
 This package implements the IEEE 1905.1 and 1905.1a protocol, as well as the
 WFA Multi-AP protocol.
endef

ifeq ($(CONFIG_BIG_ENDIAN),y)
ENDIAN= big
else
ENDIAN= little
endif

define Build/Compile
	$(call Build/Compile/Default,PLATFORM="linux" FLAVOUR="arm_wrt1900acx" ENDIANNESS="$(ENDIAN)")
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output/*.so $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/al/src_independent/extensions/map/1905_lib.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/factory/interfaces/extensions/map/1905_lib_common.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/factory/interfaces/1905_cmdus.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/factory/interfaces/1905_tlvs.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/factory/interfaces/extensions/map/map_tlvs.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/common/interfaces/platform.h $(1)/usr/include/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/common/interfaces/packet_tools.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/output/*.so $(1)/usr/lib
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
