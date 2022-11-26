# Copyright (C) 2022, Applied Informatics Software Engineering GmbH

include $(TOPDIR)/rules.mk

PKG_NAME:=rmagent
PKG_VERSION:=1.14.1
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=macchina-remote-sdk-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/my-devices/sdk/archive/refs/tags/
PKG_HASH:=fa5c2d39f0b05f6098cec9c0ee93aa96f5907ff0781536a3baba2fe705d56d25

PKG_LICENSE:=BSL-1.0

TARGET_CXXFLAGS += -DWEBTUNNEL_ENABLE_TLS

include $(INCLUDE_DIR)/package.mk

define Package/rmagent
  SECTION:=net
  CATEGORY:=Network
  TITLE:=macchina.io REMOTE Device Agent (WebTunnelAgent)
  URL:=https://github.com/my-devices/sdk
  DEPENDS:=+libstdcpp +libpthread +librt +libopenssl +zlib
endef

define Package/rmagent/description
	macchina.io REMOTE Device Agent (WebTunnelAgent) connects a device to a macchina.io REMOTE server.
	For more information, please visit <https://macchina.io/remote>.
endef

define Build/Prepare
	(cd $(PKG_BUILD_DIR) && tar xfz $(DL_DIR)/$(PKG_SOURCE) --strip 1)
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./configure --cflags=-DPOCO_UTIL_NO_XMLCONFIGURATION --cflags=-DPOCO_UTIL_NO_JSONCONFIGURATION --no-tests --no-samples --omit=PageCompiler,PageCompiler/File2Page --static)
endef

define Build/Compile
	(cd $(PKG_BUILD_DIR); $(MAKE) DEFAULT_TARGET=static_release $(MAKE_VARS) $(MAKE_FLAGS) CROSS_COMPILE=$(TARGET_CROSS) POCO_TARGET_OSARCH=$(ARCH))
	(cd $(PKG_BUILD_DIR)/WebTunnel/WebTunnelAgent; $(MAKE) POCO_BASE=$(PKG_BUILD_DIR) $(MAKE_VARS) $(MAKE_FLAGS) CROSS_COMPILE=$(TARGET_CROSS) POCO_TARGET_OSARCH=$(ARCH) WEBTUNNEL_ENABLE_TLS=1 shared_release)
endef

define Package/rmagent/install
	$(INSTALL_DIR) $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/WebTunnel/WebTunnelAgent/bin/Linux/$(ARCH)/WebTunnelAgent $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/WebTunnelAgent $(1)/etc/init.d/
	$(INSTALL_DATA) ./files/WebTunnelAgent.properties $(1)/etc/WebTunnelAgent.properties
endef

$(eval $(call BuildPackage,rmagent))
