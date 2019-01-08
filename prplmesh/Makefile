#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=prplmesh
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/prplfoundation/prplMesh.git
PKG_SOURCE_DATE:=2018-12-18
PKG_SOURCE_VERSION:=56f2c3531066e461be8911b488b54193fb6b88e0
PKG_MIRROR_HASH:=d200bf556842b0c91da53ec3a514f8c80c39e7e9b6b7f8c72bbef0275252c233

PKG_LICENSE:=BSD-2-Clause-Patent
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS += -DOPENWRT=TRUE

define Package/prplmesh
  SECTION:=net
  CATEGORY:=Network
  TITLE:=An implementation of the Wi-Fi Alliance Multi-AP Specification
  URL:=https://github.com/prplfoundation/prplMesh
  MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
  DEPENDS:=+ebtables +libnl-core +libnl-genl +libopenssl +libpthread +librt +libubox +libubus +rpcd +rpcd-mod-iwinfo @!BIG_ENDIAN
endef

define Package/prplmesh/description
 The IEEE 1905.1 protocol is a mechanism for home devices to discover each
 other and communicate, so that they can use optimal paths for transmission.
 This package implements the IEEE 1905.1 and 1905.1a protocol, as well as the
 WFA Multi-AP protocol.
endef


define Package/prplmesh/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/prplmesh $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_DATA) ./files/prplmesh.config $(1)/etc/config/prplmesh
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/prplmesh.init $(1)/etc/init.d/prplmesh
endef

$(eval $(call BuildPackage,prplmesh))
