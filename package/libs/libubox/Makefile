include $(TOPDIR)/rules.mk

PKG_NAME:=libubox
PKG_RELEASE=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=$(PROJECT_GIT)/project/libubox.git
PKG_SOURCE_DATE:=2018-02-08
PKG_SOURCE_VERSION:=bb0c830b2a259b7c5ffa7c309ce9d3004f3c41a1
PKG_MIRROR_HASH:=25d6a194b0dfdcf85119af90d354e00b8d1c20df94aa0f5ec43cf0399f407298
CMAKE_INSTALL:=1

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=

PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>

PKG_BUILD_DEPENDS:=lua

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libubox
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Basic utility library
  ABI_VERSION:=$(PKG_VERSION)
  DEPENDS:=
endef

define Package/libblobmsg-json
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=blobmsg <-> json conversion library
  DEPENDS:=+libjson-c +libubox
endef

define Package/jshn
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libjson-c +libubox +libblobmsg-json
  TITLE:=JSON SHell Notation
endef

define Package/jshn/description
  Library for parsing and generating JSON from shell scripts
endef

define Package/libjson-script
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+libubox
  TITLE:=Minimalistic JSON based scripting engine
endef

define Package/libubox-lua
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libubox +liblua
  TITLE:=Lua binding for the OpenWrt Basic utility library
endef

TARGET_CFLAGS += -I$(STAGING_DIR)/opt/include
CMAKE_OPTIONS = \
	-DLUAPATH=/opt/lib/lua

define Package/libubox/install
	$(INSTALL_DIR) $(1)/opt/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/lib/libubox.so $(1)/opt/lib/
endef

define Package/libblobmsg-json/install
	$(INSTALL_DIR) $(1)/opt/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/lib/libblobmsg_json.so $(1)/opt/lib/
endef

define Package/jshn/install
	$(INSTALL_DIR) $(1)/opt/bin $(1)/opt/share/libubox
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/jshn $(1)/opt/bin
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/share/libubox/jshn.sh $(1)/opt/share/libubox
endef

define Package/libjson-script/install
	$(INSTALL_DIR) $(1)/opt/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/opt/lib/libjson_script.so $(1)/opt/lib/
endef

define Package/libubox-lua/install
	$(INSTALL_DIR) $(1)/opt/lib/lua
	$(CP) $(PKG_BUILD_DIR)/lua/uloop.so $(1)/opt/lib/lua/
endef


CMAKE_HOST_OPTIONS += \
	-DBUILD_LUA=OFF \
	-DBUILD_EXAMPLES=OFF \

$(eval $(call BuildPackage,libubox))
$(eval $(call BuildPackage,libblobmsg-json))
$(eval $(call BuildPackage,jshn))
$(eval $(call BuildPackage,libjson-script))
$(eval $(call BuildPackage,libubox-lua))
$(eval $(call HostBuild))
