#
# Copyright (C) 2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=odp
PKG_VERSION:=1.15.0.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/Linaro/$(PKG_NAME)/archive/v$(PKG_VERSION)
PKG_MD5SUM:=a2d4dbc3dddeaa7f4b20fbf35611f6f2

PKG_LICENSE:=BSD
PKG_LICENSE_FILES:=LICENSE

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/Default
  TITLE:=OpenDataPlane
  URL:=https://www.opendataplane.org
endef

define Package/$(PKG_NAME)-libs
$(call Package/$(PKG_NAME)/Default)
  SUBMENU:=Networking
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE+= library
  DEPENDS:=+libpcap +libopenssl +libatomic
endef

define Package/$(PKG_NAME)-examples
$(call Package/$(PKG_NAME)/Default)
  SECTION:=net
  CATEGORY:=Network
  TITLE+= example apps
  DEPENDS:=+odp-libs
endef

TARGET_CFLAGS += -Wno-error

CONFIGURE_ARGS+= \
	--enable-helper-linux

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./bootstrap)
	$(call Build/Configure/Default)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libodp*.{a,so*} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libodp*.pc $(1)/usr/lib/pkgconfig/
endef

define Package/$(PKG_NAME)-libs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib
endef

define Package/$(PKG_NAME)-examples/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* \
		$(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/common_plat/performance/*.sh \
		$(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/test/common_plat/performance/udp64.pcap \
		$(1)/usr/bin
endef

$(eval $(call BuildPackage,odp-libs))
$(eval $(call BuildPackage,odp-examples))
