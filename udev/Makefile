
include $(TOPDIR)/rules.mk

PKG_NAME:=udev
PKG_VERSION:=182
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/udev.git
PKG_HASH:=a0638858c5d3447e6b61409e488df65239343a81ccb258a89046c83841dd7890
PKG_MIRROR_HASH:=cb6e22158843486ed80588347f8cbc76ed6e4bd711bcff46124b66835916ec55


PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/udev
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=udev
	MENU:=1
	DEPENDS:=+libblkid +libkmod +usbutils
	PROVIDES:=udev
	CONFLICTS:=eudev
endef

define Package/udev/description
udev
endef

define Package/udev/conffiles
/etc/udev/udev.conf
endef

define Package/udev/config
	source "$(SOURCE)/Config.in"
endef

TARGET_CFLAGS += 
TARGET_LDFLAGS += 

CONFIGURE_ARGS += --disable-keymap --disable-gudev --disable-introspection --disable-gtk-doc --disable-gtk-doc-html
CONFIGURE_ARGS += --prefix=/usr --exec-prefix= --sysconfdir=/etc --sbindir=/sbin
CONFIGURE_ARGS += USBUTILS_LIBS=$(STAGING_DIR)/usr/lib
CONFIGURE_ARGS +=  $(udev-args-y)

udev-args-$(CONFIG_UDEV_DISABLE_LOGGING) += --disable-logging
udev-args-$(CONFIG_UDEV_ENABLE_DEBUG) += --disable-debug
udev-args-$(CONFIG_UDEV_EXTRA_edd_id) += --disable-edd
udev-args-$(CONFIG_UDEV_EXTRA_floppy) += --disable-floppy

udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_accelerometer) += accelerometer
udev-extra-rules-$(CONFIG_UDEV_EXTRA_accelerometer) += 61-accelerometer.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_ata_id) += ata_id
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_cdrom_id) += cdrom_id
udev-extra-rules-$(CONFIG_UDEV_EXTRA_cdrom_id) += 60-cdrom_id.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_collect) += collect
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_edd_id) += edd_id
udev-extra-rules-$(CONFIG_UDEV_EXTRA_edd_id) += 61-persistent-storage-edd.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_firmware) += firmware
udev-extra-rules-$(CONFIG_UDEV_EXTRA_firmware) += 50-firmware.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_floppy) += create_floppy_devices
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_input_id) += input_id
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_mtd_probe) += mtd_probe
udev-extra-rules-$(CONFIG_UDEV_EXTRA_mtd_probe) += 75-probe_mtd.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_path_id) += path_id
udev-extra-rules-$(CONFIG_UDEV_EXTRA_qemu) += 42-qemu-usb.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_rule_generator) += write_cd_rules write_net_rules
udev-extra-lib-data-$(CONFIG_UDEV_EXTRA_rule_generator) += rule_generator.functions
udev-extra-rules-$(CONFIG_UDEV_EXTRA_rule_generator) += 75-cd-aliases-generator.rules
udev-extra-rules-$(CONFIG_UDEV_EXTRA_rule_generator) += 75-persistent-net-generator.rules
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_scsi_id) += scsi_id
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_usb_id) += usb_id
udev-extra-lib-bin-$(CONFIG_UDEV_EXTRA_v4l_id) += v4l_id
udev-extra-rules-$(CONFIG_UDEV_EXTRA_v4l_id) += 60-persistent-v4l.rules

define Build/Compile
	$(call Build/Compile/Default)
endef

define Build/InstallDev

	$(INSTALL_DIR)		$(1)/lib
	$(INSTALL_DIR)		$(1)/usr/include
	$(INSTALL_DIR)		$(1)/usr/lib/pkgconfig
	$(INSTALL_DIR)		$(1)/usr/share/pkgconfig
	$(CP)				$(PKG_INSTALL_DIR)/usr/include/libudev.h			$(1)/usr/include
	$(CP)				$(PKG_INSTALL_DIR)/usr/share/pkgconfig/udev.pc		$(1)/usr/share/pkgconfig
	$(CP)				$(PKG_INSTALL_DIR)/lib/libudev.so*					$(1)/lib
	$(CP)				$(PKG_INSTALL_DIR)/lib/pkgconfig/libudev.pc			$(1)/usr/lib/pkgconfig

endef

define Package/udev/install

	$(INSTALL_DIR)		$(1)/etc/udev/rules.d
	$(INSTALL_DIR)		$(1)/lib/udev/rules.d
	$(INSTALL_DIR)		$(1)/sbin
	$(INSTALL_DIR)		$(1)/lib
	$(INSTALL_DIR)		$(1)/lib/udev

	$(INSTALL_BIN)		$(PKG_INSTALL_DIR)/sbin/udevadm
	$(INSTALL_BIN)		$(PKG_INSTALL_DIR)/sbin/udevd
	$(INSTALL_BIN)		$(1)/sbin

	$(INSTALL_DATA)		$(PKG_INSTALL_DIR)/etc/udev/udev.conf
	$(INSTALL_DATA)		$(1)/etc/udev
	$(INSTALL_DATA)		$(1)/lib/udev/rules.d
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,$(udev-extra-rules-y))
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,50-udev-default.rules)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,60-persistent-input.rules)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,60-persistent-serial.rules)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,60-persistent-storage.rules)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,80-drivers.rules)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev/rules.d/,95-udev-late.rules)

	$(CP)				$(PKG_INSTALL_DIR)/lib/libudev.so*					$(1)/lib

ifneq ($(udev-extra-lib-bin-y),)
	$(INSTALL_BIN)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev,$(udev-extra-lib-bin-y))
	$(INSTALL_BIN)		$(1)/lib/udev
endif

ifneq ($(udev-extra-lib-data-y),)
	$(INSTALL_DATA)		$(addprefix $(PKG_INSTALL_DIR)/lib/udev,$(udev-extra-lib-data-y))
	$(INSTALL_DATA)		$(1)/lib/udev
endif

endef

$(eval $(call BuildPackage,udev))
