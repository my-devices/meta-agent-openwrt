# Copyright (C) 2022, Applied Informatics Software Engineering GmbH

include $(TOPDIR)/rules.mk

PKG_NAME:=rmgateway
PKG_VERSION:=2.4.0
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=macchina-remote-gateway-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/my-devices/gateway/archive/refs/tags/
PKG_HASH:=c240890d31f5c0fcb94d78060ef6ce4e57f000e154d7d490dd564e7c95f0af0e

PKG_LICENSE:=BSL-1.0

MACCHINA_REMOTE_SDK_VERSION:=1.13.0
MACCHINA_REMOTE_SDK_SOURCE:=macchina-remote-sdk-$(MACCHINA_REMOTE_SDK_VERSION).tar.gz
MACCHINA_REMOTE_SDK_SOURCE_URL:=https://github.com/my-devices/sdk/archive/refs/tags/
MACCHINA_REMOTE_SDK_HASH:=db17ab1532f600573dc18ba80c9c931ab49ec19020ac9902a117ba255c4091c3

TARGET_CXXFLAGS += -DWEBTUNNEL_ENABLE_TLS

include $(INCLUDE_DIR)/package.mk

define Package/rmgateway
  SECTION:=net
  CATEGORY:=Network
  TITLE:=macchina.io REMOTE Gateway
  URL:=https://github.com/my-devices/gateway
  DEPENDS:=+libstdcpp +libpthread +librt +libopenssl +zlib
endef

define Package/rmgateway/description
	macchina.io REMOTE Gateway connects one or more devices to a macchina.io REMOTE server.
	For more information, please visit <https://macchina.io/remote>.
endef

define Download/macchina_remote_sdk
  FILE:=$(MACCHINA_REMOTE_SDK_SOURCE)
  URL_FILE:=$(MACCHINA_REMOTE_SDK_SOURCE)
  URL:=$(MACCHINA_REMOTE_SDK_SOURCE_URL)
  HASH:=$(MACCHINA_REMOTE_SDK_HASH)
endef

define Build/Prepare
	(mkdir $(PKG_BUILD_DIR)/sdk $(PKG_BUILD_DIR)/gateway)
	(cd $(PKG_BUILD_DIR)/sdk && tar xfz $(DL_DIR)/$(MACCHINA_REMOTE_SDK_SOURCE) --strip 1)
	(cd $(PKG_BUILD_DIR)/gateway && tar xfz $(DL_DIR)/$(PKG_SOURCE) --strip 1)
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR)/sdk; ./configure --cflags=-DPOCO_UTIL_NO_XMLCONFIGURATION --cflags=-DPOCO_UTIL_NO_JSONCONFIGURATION --no-tests --no-samples --omit=PageCompiler,PageCompiler/File2Page --static)
endef

define Build/Compile
	(cd $(PKG_BUILD_DIR)/sdk; $(MAKE) DEFAULT_TARGET=static_release $(MAKE_VARS) $(MAKE_FLAGS) CROSS_COMPILE=$(TARGET_CROSS) POCO_TARGET_OSARCH=$(ARCH))
	(touch $(PKG_BUILD_DIR)/gateway/src/*.h $(PKG_BUILD_DIR)/gateway/src/*.cpp)
	(cd $(PKG_BUILD_DIR)/gateway; $(MAKE) POCO_BASE=$(PKG_BUILD_DIR)/sdk $(MAKE_VARS) $(MAKE_FLAGS) CROSS_COMPILE=$(TARGET_CROSS) POCO_TARGET_OSARCH=$(ARCH) WEBTUNNEL_ENABLE_TLS=1 shared_release)
endef

define Package/rmgateway/install
	$(INSTALL_DIR) $(1)/etc/rmgateway/
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/gateway/bin/Linux/$(ARCH)/rmgateway $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/rmgateway $(1)/etc/init.d/
	$(INSTALL_DATA) ./files/rmgateway.properties $(1)/etc/rmgateway/rmgateway.properties
endef

$(eval $(call Download,macchina_remote_sdk))
$(eval $(call BuildPackage,rmgateway))