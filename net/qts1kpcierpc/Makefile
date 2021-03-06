include $(TOPDIR)/rules.mk

PKG_NAME:=qts1kpcierpc
ifdef LEDE_RELEASE
WRT_FORK:=LEDE
else
WRT_FORK:=OPENWRT
endif
PKG_VERSION:=37.3.1.25-$(WRT_FORK)-1
PKG_RELEASE:=1

PKG_SOURCE:=quantenna_sources.tar.gz
PKG_SOURCE_VERSION:=b0e7e4f587e9a0b6655b13e2504ed0b6880354fc
PKG_SOURCE_URL:=https://github.com/ILOVEPIE/qts1kpcie_wifi_driver/blob/$(PKG_SOURCE_VERSION)/quantenna_sources.tar.gz?raw=true
PKG_MD5SUM:=5dac6da69b7c8453c8cfb3d79468e360

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/qts1kpcierpc
  SUBMENU:=Wireless Drivers
  TITLE:=Quantenna QTS1000 Driver Utilities
  DEPENDS:=+librpc +QUANTENNA_IWINFO_PLUGIN:qts1kpcie_status_plugin
  MAINTAINER:="Patrick Martin thehairyrock@gmail.com"
  URL:="http://www.downloads.netgear.com/files/GPL/R7500-and_qtn_gpl_src_V1.0.0.94.zip"
  MENU:=1
endef

define Package/qts1kpcierpc/description
 Utilitys for the Quantenna wifi driver
endef

define Package/qts1kpcierpc/config
	source "$(SOURCE)/Config.in"
endef

define Build/Configure
	$(call Build/Configure/Default,--with-linux-headers=$(LINUX_DIR))
endef

define Build/Compile
	+$(MAKE_VARS) \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(MAKE_PATH)/../quantenna/client/libqcsapi_client_src \
		$(MAKE_FLAGS) \
		qcsapi_pcie qcsapi_sockrpc qcsapi_sockraw;
	
	+$(MAKE_VARS) \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(MAKE_PATH)/../quantenna/client/qevt-client \
		$(MAKE_FLAGS) \
		qevt_client;
	
endef

define Package/qts1kpcierpc/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/libqcsapi_client.so $(1)/usr/lib/libqcsapi_client.so
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/libqcsapi_client.so.1 $(1)/usr/lib/libqcsapi_client.so.1
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/libqcsapi_client.so.1.0.1 $(1)/usr/lib/libqcsapi_client.so.1.0.1
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/qcsapi_pcie $(1)/usr/sbin/qcsapi_pcie
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/qcsapi_sockrpc $(1)/usr/sbin/qcsapi_sockrpc
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/../quantenna/client/libqcsapi_client_src/qcsapi_sockraw $(1)/usr/sbin/qcsapi_sockraw
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/../quantenna/client/qevt-client/qevt_client $(1)/usr/sbin/qevt_client
endef

$(eval $(call BuildPackage,qts1kpcierpc))
