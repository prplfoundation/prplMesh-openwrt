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
PKG_SOURCE_DATE:=2018-11-16
PKG_SOURCE_VERSION:=cf26760f4da6e8ef7a04ff5b792d5a3d71018525
PKG_MIRROR_HASH:=b8b8878b013802da41410802dc50208051e335432a2e35fa6e6f39bfa36f3338

#PKG_VERSION:=1.0.0
#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=https://github.com/prplfoundation/prplMesh/archive/$(PKG_VERSION)/$(PKG_SOURCE)
#PKG_HASH:=07ad7ae4a2fd9a67adb4780f82c7fa3521f5706db7be6518d44358002a78b01d

PKG_LICENSE:=Apache-2.0
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
  DEPENDS:=+libnl-core +libnl-genl +libopenssl +libpthread +librt +libubox +libubus +rpcd +rpcd-mod-iwinfo @!BIG_ENDIAN
endef

define Package/prplmesh/description
 The IEEE 1905.1 protocol is a mechanism for home devices to discover each
 other and communicate, so that they can use optimal paths for transmission.
 This package implements the IEEE 1905.1 and 1905.1a protocol, as well as the
 WFA Multi-AP protocol.
endef


define Package/prplmesh/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/al_entity $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_DATA) ./files/prplmesh.config $(1)/etc/config/prplmesh
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/prplmesh.init $(1)/etc/init.d/prplmesh
endef

$(eval $(call BuildPackage,prplmesh))
