#
# Copyright (C) 2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ofp
PKG_VERSION:=2.0.1
PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/OpenFastPath/$(PKG_NAME)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=5b446401c0c09f233b761e1fc0c187656841b51c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

#PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=https://github.com/OpenFastPath/$(PKG_NAME)/archive/$(PKG_VERSION)

PKG_LICENSE:=BSD 3-CLause
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/Default
  TITLE:=OpenFastPath
  URL:=https://github.com/OpenFastPath/ofp
endef

define Package/$(PKG_NAME)-libs
$(call Package/$(PKG_NAME)/Default)
  SUBMENU:=Networking
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE+= library
  DEPENDS:=+libstdcpp +libopenssl +odp-libs +libconfig
endef

define Package/$(PKG_NAME)-examples
$(call Package/$(PKG_NAME)/Default)
  SECTION:=net
  CATEGORY:=Network
  TITLE+= example apps
  DEPENDS:=+libstdcpp +libopenssl +odp-libs +ofp-libs +libconfig
endef

TARGET_CFLAGS += -Wno-error

CONFIGURE_ARGS+= \
	--with-odp=$(STAGING_DIR)/usr/ \
	--enable-debug \
	--enable-ipv6 \
	--enable-sp

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./bootstrap)
	$(call Build/Configure/Default)
endef

define Package/$(PKG_NAME)-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib
endef

define Package/$(PKG_NAME)-examples/install
	$(FIND) $(PKG_BUILD_DIR)/example/ -executable -type f -exec $(INSTALL_BIN) {} $(PKG_INSTALL_DIR)/usr/bin/ \;
	$(FIND) $(PKG_BUILD_DIR)/scripts/ -executable -type f -exec $(INSTALL_BIN) {} $(PKG_INSTALL_DIR)/usr/bin/ \;
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* \
		$(1)/usr/bin
endef

$(eval $(call BuildPackage,$(PKG_NAME)-libs))
$(eval $(call BuildPackage,$(PKG_NAME)-examples))
