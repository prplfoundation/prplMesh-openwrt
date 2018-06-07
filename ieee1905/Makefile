#
# Copyright (C) 2018 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ieee1905
PKG_VERSION:=5ef9c70b7055dfcd1bd3c75f4ed297a768380443
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/prplfoundation/WT-382/archive/$(PKG_VERSION)/$(PKG_SOURCE)
PKG_MD5SUM:=5224d6720d28a820705f8c98a2783345b40b2113cd2f4bbedf1977f83ff2cd13
PKG_BUILD_DIR:=$(BUILD_DIR)/WT-382-$(PKG_VERSION)
PKG_LICENSE:=unknown
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/ieee1905
  SECTION:=net
  CATEGORY:=Network
  TITLE:=An implementation of the IEEE-1905.1 protocol
  URL:=https://github.com/prplfoundation/WT-382
  MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
  DEPENDS:=+libpcap +libopenssl @!BIG_ENDIAN
endef

define Package/ieee1905/description
 The IEEE 1905.1 protocol is a mechanism for home devices to discover each
 other and communicate, so that they can use optimal paths for transmission.
 This package implements the IEEE 1905.1 and 1905.1a protocol, as well as the
 Wi-Fi EasyMesh protocol.
endef

CCFLAGS:=\
	$(TARGET_CFLAGS) \
	-D_FLAVOUR_X86_GENERIC_ \
	-D_HOST_IS_LITTLE_ENDIAN_=$(if $(CONFIG_BIG_ENDIAN),0,1) \
	-include stdint.h \
	-DINT8U=uint8_t \
	-DINT16U=uint16_t \
	-DINT32U=uint32_t \
	-DINT8S=int8_t \
	-DINT16S=int16_t \
	-DINT32S=int32_t \
	-DENABLE_COLOR=0 \
	-DMAX_NETWORK_SEGMENT_SIZE=1500 \
	-D_GNU_SOURCE \
	-DSEND_EMPTY_TLVS \
	-DFIX_BROKEN_TLVS \
	-DSPEED_UP_DISCOVERY \
	-D_BUILD_NUMBER_='\"$(PKG_VERSION)\"' \
	-DREGISTER_EXTENSION_BBF \
	-Wall

MAKE_FLAGS+= \
	PLATFORM=linux \
	FLAVOUR=x86_generic \
	CC="$(TARGET_CC)" \
	CCFLAGS="$(CCFLAGS) `$(PKG_CONFIG) --cflags libcrypto`" \
	LDFLAGS="-lrt -lpthread -lpcap `$(PKG_CONFIG) --libs libcrypto`"

define Package/ieee1905/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/output/al_entity $(1)/usr/bin/
endef

$(eval $(call BuildPackage,ieee1905))
