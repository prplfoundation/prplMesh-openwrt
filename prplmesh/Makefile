#
# Copyright (C) 2019 PRPL Foundation
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=prplmesh
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/prplfoundation/prplMesh.git
PKG_SOURCE_DATE:=2019-11-14
PKG_SOURCE_VERSION:=7f287c810ea16483567560cf63c5c23b216e8e0c
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=BSD-2-Clause-Patent
PKG_LICENSE_FILES:=LICENSE

PKG_PRPL_SUBDIR:=prplMesh

PKG_BUILD_DEPENDS:=python/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/prplmesh
  SECTION:=net
  CATEGORY:=Network
  TITLE:=An implementation of the Wi-Fi Alliance EasyMesh
  URL:=https://github.com/prplfoundation/prplMesh
  MAINTAINER:=Arnout Vandecappelle (Essensium/Mind) <arnout@mind.be>
  # Need to figure out how to make proper hostapd/wpa-supplicant dependency
  DEPENDS:=+libstdcpp +libuuid +libzmq-nc \
           +getopt +libncurses +libreadline +terminfo
endef

define Package/prplmesh/description
  This package adds WiFi Alliance EasyMesh support to OpenWRT. Wi-Fi EasyMesh
  networks employ multiple access points that work together to form a unified
  network that provides smart, efficient Wi-Fi throughout the home and outdoor
  spaces.
endef

define Package/prplmesh/config
	source "$(SOURCE)/Config.in"
endef

define Build/Prepare
	# PrplMesh is expected to be one level deep in the source
	# tree as it is typically downloaded and built as a part
	# of a multi-project repo and it stores all the build artefacts
	# in '../build' folder. If building with USE_SOURCE_DIR point to
	# the multi-project repo folder (the check below will skip moving the files)
	$(Build/Prepare/Default)
	mkdir -p $(PKG_BUILD_DIR)/$(PKG_PRPL_SUBDIR)
	[[ -L "$(PKG_BUILD_DIR)" ]] || ( cd $(PKG_BUILD_DIR)/$(PKG_PRPL_SUBDIR) && \
	find .. -maxdepth 1 ! -name '$(PKG_PRPL_SUBDIR)' ! -name '..' -exec mv {} ./ \; )
endef

define Build/Configure
	# Skipping the configure step for now
endef

define Build/Compile
	# Mess with python till we can just use cmake
	python$(PYTHON_VERSION) -m pip show pyyaml > /dev/null || python$(PYTHON_VERSION) -m pip install pyyaml
	CMAKE_SYSTEM_PROCESSOR="$(ARCH)"
	TOOLCHAIN_DIR="$(TOOLCHAIN_DIR)"
	BUILD_DIR="$(BUILD_DIR)"
	TARGET_CROSS="$(TARGET_CROSS)"
	# Build the agent
	cd $(PKG_BUILD_DIR) && \
	CMAKE_SYSTEM_PROCESSOR="$(ARCH)" \
	TOOLCHAIN_DIR="$(TOOLCHAIN_DIR)" \
	BUILD_DIR="$(BUILD_DIR)"         \
	TARGET_CROSS="$(TARGET_CROSS)"   \
	CMAKE_TOOLCHAIN_FILE=./prplMesh/tools/cmake/toolchain/owrt.cmake \
	python$(PYTHON_VERSION) \
	./$(PKG_PRPL_SUBDIR)/tools/maptools.py build map -f MSGLIB=zmq CMAKE_BUILD_TYPE=$(CONFIG_PRPLMESH_BUILD_TYPE)
endef

define Build/Install
	# Strip and install only agent and/or controller files to $(PKG_INSTALL_DIR)
endef

define Package/prplmesh/install
	# Install files necessary for ipk
	#$(INSTALL_DIR) $(1)/usr/sbin/
	#$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/prplmesh $(1)/usr/sbin/
	#$(INSTALL_DIR) $(1)/etc/config/
	#$(INSTALL_DATA) ./files/prplmesh.config $(1)/etc/config/prplmesh
	#$(INSTALL_DIR) $(1)/etc/init.d/
	#$(INSTALL_BIN) ./files/prplmesh.init $(1)/etc/init.d/prplmesh
endef

$(eval $(call BuildPackage,prplmesh))
