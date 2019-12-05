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
PKG_SOURCE_VERSION:=3b813b454220f6ee9d8cee9623598e33e34f0f75
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=BSD-2-Clause-Patent
PKG_LICENSE_FILES:=LICENSE

# prplMesh sources subfolder (the build environment expects the code
# to be in the subfolder under that name)
PKG_PRPL_SUBDIR:=prplMesh

# Install location of the bin and share folders. They have to be .../bin and
# .../share due to some of the relative paths being hardcoded in the prplMesh
# code.
PKG_PRPL_BIN_PATH:=/opt/beerocks/bin
PKG_PRPL_SHARE_PATH:=/opt/beerocks/share
# Install location for the libraries
PKG_PRPL_LIB_PATH:=/usr/lib
# Where to find/store the agent and controller configuration files
PKG_PRPL_CFG_PATH:=/etc/beerocks
# Where we want scripts to be installed
PKG_PRPL_SCRIPTS_PATH:=/opt/beerocks/scripts
# Where to store the temporary files
PKG_PRPL_TMP_PATH:=/tmp/beerocks

# Currently the build needs Python (this dependency will be got rid of)
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
  DEPENDS:=+libstdcpp +libuuid +libzmq-nc +libjson-c +libnl-tiny \
            +libopenssl +getopt +libncurses +libreadline +terminfo
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
	TARGET_BWL_TYPE=$(PRPLMESH_BWL_HAL)      \
	TARGET_TMP_PATH="$(PKG_PRPL_TMP_PATH)"   \
	PKG_PRPL_BIN_PATH="$(PKG_PRPL_BIN_PATH)" \
	PKG_PRPL_LIB_PATH="$(PKG_PRPL_LIB_PATH)" \
	PKG_PRPL_CFG_PATH="$(PKG_PRPL_CFG_PATH)" \
	PKG_PRPL_SHARE_PATH="$(PKG_PRPL_SHARE_PATH)" \
	CMAKE_TOOLCHAIN_FILE=./prplMesh/tools/cmake/toolchain/owrt.cmake \
	python$(PYTHON_VERSION) \
	./$(PKG_PRPL_SUBDIR)/tools/maptools.py build map -f MSGLIB=zmq CMAKE_BUILD_TYPE=$(CONFIG_PRPLMESH_BUILD_TYPE)
endef

define Build/Install
	# Install agent and controller files to $(PKG_INSTALL_DIR)
	# Always strip since the binaries are quite big otherwise.
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)"
	$(CP) "$(PKG_BUILD_DIR)/build/install/"* "$(PKG_INSTALL_DIR)/"
	# get rid of unnecessary stuff
	rm -Rf "$(PKG_INSTALL_DIR)/host" "$(PKG_INSTALL_DIR)/lib/cmake"
	rm -Rf "$(PKG_INSTALL_DIR)/include" "$(PKG_INSTALL_DIR)/bin/tests"
	rm -Rf "$(PKG_INSTALL_DIR)/scripts/beerocks*.sh" "$(PKG_INSTALL_DIR)/scripts/udhcpc*"
	# strip all executables and libs (they are pretty big for embedded sw)
	for _f in `find "$(PKG_INSTALL_DIR)" -type f`; do                   \
	  if file "$$$$_f" | grep -E 'ELF .*not stripped' > /dev/null; then \
	    $(TOOLCHAIN_DIR)/bin/${TARGET_CROSS}strip "$$$$_f";             \
	  fi;                                                               \
	done
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)/$(PKG_PRPL_BIN_PATH)"
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)/$(PKG_PRPL_SHARE_PATH)"
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)/$(PKG_PRPL_CFG_PATH)"
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)/$(PKG_PRPL_LIB_PATH)"
	$(INSTALL_DIR) "$(PKG_INSTALL_DIR)/$(PKG_PRPL_SCRIPTS_PATH)"
	mv -fT "$(PKG_INSTALL_DIR)/bin" "$(PKG_INSTALL_DIR)/$(PKG_PRPL_BIN_PATH)"
	mv -fT "$(PKG_INSTALL_DIR)/share" "$(PKG_INSTALL_DIR)/$(PKG_PRPL_SHARE_PATH)"
	mv -fT "$(PKG_INSTALL_DIR)/config" "$(PKG_INSTALL_DIR)/$(PKG_PRPL_CFG_PATH)"
	mv -fT "$(PKG_INSTALL_DIR)/lib" "$(PKG_INSTALL_DIR)/$(PKG_PRPL_LIB_PATH)"
	mv -fT "$(PKG_INSTALL_DIR)/scripts" "$(PKG_INSTALL_DIR)/$(PKG_PRPL_SCRIPTS_PATH)"
endef

define Package/prplmesh/install
	# Install files necessary for ipk
	$(CP) "$(PKG_INSTALL_DIR)/"* "$(1)/"
endef

$(eval $(call BuildPackage,prplmesh))
