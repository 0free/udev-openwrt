
include $(TOPDIR)/rules.mk

PKG_NAME:=udev
PKG_VERSION:=182
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/udev.git
PKG_HASH:=12116f22cbc5258b4cc79ee6a0574730e71a8abe31badf076709d5ac33a7e2d9
PKG_MIRROR_HASH:=b450009b6c6224e72418b14071e623250997ddc0ace339cd3fbab18ffc45a6ab

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

PKG_CONFIG_PATH=$(STAGING_DIR)/usr/lib/pkgconfig:$(STAGING_DIR)/usr/share/pkgconfig

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
DISABLE_NLS:=--disable-nls

include $(INCLUDE_DIR)/package.mk

define Package/udev
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=udev
	DEPENDS:=+libblkid +libkmod +libusb-1.0 +usbutils
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

TARGET_CFLAGS += -L$(STAGING_DIR)/usr/lib
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib

CONFIGURE_ARGS += --disable-manpages --disable-gtk-doc
CONFIGURE_ARGS += --prefix=/usr --exec-prefix= --sysconfdir=/etc --sbindir=/sbin
CONFIGURE_ARGS += USBUTILS_LIBS=$(STAGING_DIR)/usr/lib
CONFIGURE_ARGS += \
$(if $(CONFIG_UDEV_DEBUG),--enable-debug,--disable-debug) \
$(if $(CONFIG_UDEV_LOGGING),--enable-logging,--disable-logging) \
$(if $(CONFIG_UDEV_GUDEV),--enable-gudev,--disable-gudev) \
$(if $(CONFIG_UDEV_INTROSPECTION),--enable-introspection,--disable-introspection) \
$(if $(CONFIG_UDEV_KEYMAP),--enable-keymap,--disable-keymap) \
$(if $(CONFIG_UDEV_EMTD_PROBE),--enable-mtd_probe,--disable-mtd_probe) \
$(if $(CONFIG_UDEV_RULE_GENERATOR),--enable-rule_generator,--disable-rule_generator) \
$(if $(CONFIG_UDEV_FLOPPY),--enable-floppy,--disable-floppy) \
$(if $(CONFIG_UDEV_PACKAGE),--with-PACKAGE,--without-PACKAGE) \
$(if $(CONFIG_UDEV_SELINUX),--with-selinux,--without-selinux)

udev-extra-lib-data-$(CONFIG_UDEV_RULE_GENERATOR) += rule_generator.functions

udev-extra-lib-bin-$(CONFIG_UDEV_ACCELEROMETER) += accelerometer
udev-extra-lib-bin-$(CONFIG_UDEV_ATA_ID) += ata_id
udev-extra-lib-bin-$(CONFIG_UDEV_CDROM_ID) += cdrom_id
udev-extra-lib-bin-$(CONFIG_UDEV_COLLECT) += collect
udev-extra-lib-bin-$(CONFIG_UDEV_EDD_ID) += edd_id
udev-extra-lib-bin-$(CONFIG_UDEV_FIRMWARE) += firmware
udev-extra-lib-bin-$(CONFIG_UDEV_FLOPPY) += create_floppy_devices
udev-extra-lib-bin-$(CONFIG_UDEV_INPUT_ID) += input_id
udev-extra-lib-bin-$(CONFIG_UDEV_MTD_PROBE) += mtd_probe
udev-extra-lib-bin-$(CONFIG_UDEV_PATH_ID) += path_id
udev-extra-lib-bin-$(CONFIG_UDEV_RULE_GENERATOR) += write_cd_rules write_net_rules
udev-extra-lib-bin-$(CONFIG_UDEV_SCSI_ID) += scsi_id
udev-extra-lib-bin-$(CONFIG_UDEV_USB_ID) += usb_id
udev-extra-lib-bin-$(CONFIG_UDEV_V4L_ID) += v4l_id

udev-extra-rules-$(CONFIG_UDEV_ACCELEROMETER) += 61-accelerometer.rules
udev-extra-rules-$(CONFIG_UDEV_CDROM_ID) += 60-cdrom_id.rules
udev-extra-rules-$(CONFIG_UDEV_EDD_ID) += 61-persistent-storage-edd.rules
udev-extra-rules-$(CONFIG_UDEV_FIRMWARE) += 50-firmware.rules
udev-extra-rules-$(CONFIG_UDEV_MTD_PROBE) += 75-probe_mtd.rules
udev-extra-rules-$(CONFIG_UDEV_QEMU) += 42-qemu-usb.rules
udev-extra-rules-$(CONFIG_UDEV_RULE_GENERATOR) += 75-cd-aliases-generator.rules
udev-extra-rules-$(CONFIG_UDEV_RULE_GENERATOR) += 75-persistent-net-generator.rules
udev-extra-rules-$(CONFIG_UDEV_V4L_ID) += 60-persistent-v4l.rules

define Build/Configure
	$(call Build/Configure/udev)
	(cd $(PKG_BUILD_DIR) && chmod +x configure && ./configure $(CONFIGURE_ARGS);)
endef

define Build/Compile
	$(call Build/Compile/udev)
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

	$(INSTALL_DIR)		$(1)/sbin
	$(INSTALL_DIR)		$(1)/lib
	$(INSTALL_DIR)		$(1)/lib/udev
	$(INSTALL_DIR)		$(1)/lib/udev/rules.d
	$(INSTALL_DIR)		$(1)/etc/udev/rules.d

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
