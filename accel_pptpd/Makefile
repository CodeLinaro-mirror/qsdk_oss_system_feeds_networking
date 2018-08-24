
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=accel-pptp
PKG_VERSION:=0.8.5
PKG_RELEASE:=2

PPPD_VERSION:=2.4.7

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://downloads.sourceforge.net/project/accel-pptp/accel-pptp/
PKG_MD5SUM:=061ca3991fb69e0e79cb872b0aaf1d89

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1


PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
include $(INCLUDE_DIR)/package.mk

define Package/accel_pptpd
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Accel pptp server
  URL:=$(PKG_SOURCE_URL)
  SUBMENU:=VPN
endef

CONFIGURE_ARGS += \
	--enable-bcrelay \

TARGET_CFLAGS += \
	-DBUILD_PPTP_DRIVER=FALSE

define Package/accel_pptpd/conffiles
/etc/pptpd.conf
/etc/ppp/options.pptpd
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)			\
		CC="$(TARGET_CC)"			\
		host_alias="$(GNU_TARGET_NAME)"		\
		target_alias="$(GNU_TARGET_NAME)"	\
		build_alias="$(GNU_HOST_NAME)"		\
		ac_cv_build="$(GNU_HOST_NAME)"		\
		ac_cv_host="$(GNU_TARGET_NAME)"		\
		kernel_dir="$(LINUX_DIR)"		\
		default
endef
define Package/accel_pptpd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/sbin/bcrelay \
		$(PKG_INSTALL_DIR)/usr/sbin/pptpctrl \
		$(PKG_INSTALL_DIR)/usr/sbin/pptpd \
		$(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/lib/pptpd
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pptpd/* $(1)/usr/lib/pptpd/
	$(INSTALL_DIR) $(1)/usr/lib/pppd/$(PPPD_VERSION)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pppd/$(PPPD_VERSION)/* $(1)/usr/lib/pppd/$(PPPD_VERSION)/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) ./files/pptpd.conf $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/ppp
	$(INSTALL_DATA) ./files/options.pptpd $(1)/etc/ppp/
endef

$(eval $(call BuildPackage,accel_pptpd))
