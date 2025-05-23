#
# Copyright (C) 2007-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=util-linux
PKG_VERSION:=2.25.2
PKG_RELEASE:=4

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@KERNEL/linux/utils/$(PKG_NAME)/v2.25
PKG_MD5SUM:=cab3d7be354000f629bc601238b629b3

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING getopt/COPYING libblkid/COPYING libmount/COPYING Documentation/licenses/COPYING.GPLv2 Documentation/licenses/COPYING.LGPLv2.1 libuuid/COPYING Documentation/licenses/COPYING.BSD-3

PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_PACKAGE_cal \
	CONFIG_PACKAGE_cfdisk \
	CONFIG_PACKAGE_setterm

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/util-linux/Default
  SECTION:=utils
  CATEGORY:=Utilities
  URL:=http://www.kernel.org/pub/linux/utils/util-linux/
endef

CONFIGURE_ARGS += \
	--disable-rpath \
	--enable-new-mount	\
	--disable-tls		\
	--disable-sulogin	\
	--without-python	\
	--without-udev		\
	$(if $(CONFIG_PACKAGE_cal)$(CONFIG_PACKAGE_cfdisk)$(CONFIG_PACKAGE_setterm),--with-ncurses,--without-ncurses)

TARGET_CFLAGS += $(FPIC) -std=gnu99

define Build/InstallDev
	$(MAKE) -C $(PKG_BUILD_DIR) \
		BUILDCC="$(HOSTCC)" \
		DESTDIR="$(1)" \
		installdirs install-data

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/libblkid/blkid.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/libmount/mount.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/libsmartcols/smartcols.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/libuuid/uuid.pc $(1)/usr/lib/pkgconfig

	$(INSTALL_DIR) $(1)/usr/include/blkid
	$(CP) $(PKG_BUILD_DIR)/libblkid/src/blkid.h $(1)/usr/include/blkid
	$(INSTALL_DIR) $(1)/usr/include/libmount
	$(CP) $(PKG_BUILD_DIR)/libmount/src/libmount.h $(1)/usr/include/libmount
	$(INSTALL_DIR) $(1)/usr/include/uuid
	$(CP) $(PKG_BUILD_DIR)/libuuid/src/uuid.h $(1)/usr/include/uuid
	$(INSTALL_DIR) $(1)/usr/include/smartcols
	$(CP) $(PKG_BUILD_DIR)/libsmartcols/src/libsmartcols.h $(1)/usr/include/smartcols

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libblkid.{a,so*} $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libmount.{a,so*} $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libuuid.{a,so*} $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libsmartcols.{a,so*} $(1)/usr/lib
endef

define Package/libblkid
$(call Package/util-linux/Default)
  DEPENDS:=+libuuid
  TITLE:=block device id library
  SECTION:=libs
  CATEGORY:=Libraries
endef

define Package/libblkid/description
 The libblkid library is used to identify block devices (disks) as to their
 content (e.g. filesystem type, partitions) as well as extracting additional
 information such as filesystem labels/volume names, partitions, unique
 identifiers/serial numbers...
endef

define Package/libmount
$(call Package/util-linux/Default)
  DEPENDS:=+libblkid
  TITLE:=mount library
  SECTION:=libs
  CATEGORY:=Libraries
endef

define Package/libmount/description
 The libmount library is used to parse /etc/fstab, /etc/mtab and
 /proc/self/mountinfo files, manage the mtab file, evaluate mount options...
endef

define Package/libuuid
$(call Package/util-linux/Default)
  TITLE:=DCE compatible Universally Unique Identifier library
  SECTION:=libs
  CATEGORY:=Libraries
endef

define Package/libuuid/description
 The UUID library is used to generate unique identifiers for objects
 that may be accessible beyond the local system. This library
 generates UUIDs compatible with those created by the Open Software
 Foundation (OSF) Distributed Computing Environment (DCE) utility.
endef

define Package/libsmartcols
$(call Package/util-linux/Default)
  TITLE:=table or tree library
  SECTION:=libs
  CATEGORY:=Libraries
endef

define Package/libsmartcols/description
 The smartcols library is used to print tables and trees in a pretty way.
endef

define Package/agetty
$(call Package/util-linux/Default)
  TITLE:=alternative Linux getty
  SUBMENU=Terminal
endef

define Package/agetty/description
 agetty opens a tty port, prompts for a login name and invokes the
 /bin/login command
endef

define Package/blkid
$(call Package/util-linux/Default)
  TITLE:=locate/print block device attributes
  DEPENDS:= +libblkid +libuuid
  SUBMENU=disc
endef

define Package/blkid/description
 The blkid program is the command-line interface to working with the libblkid
 library.
endef

define Package/cal
$(call Package/util-linux/Default)
  TITLE:=display a calendar
  DEPENDS:= +libncurses
endef

define Package/cal/description
 cal displays a simple calendar
endef

define Package/cfdisk
$(call Package/util-linux/Default)
  TITLE:=display or manipulate disk partition table
  DEPENDS:= +libblkid +libncurses +libsmartcols
  SUBMENU:=disc
endef

define Package/cfdisk/description
 cfdisk is a curses-based program for partitioning any hard disk drive
endef

define Package/dmesg
$(call Package/util-linux/Default)
  TITLE:=print or control the kernel ring buffer
  DEPENDS:= +librt
endef

define Package/dmesg/description
 dmesg  is used to examine or control the kernel ring buffer
endef

define Package/fdisk
$(call Package/util-linux/Default)
  TITLE:=manipulate disk partition table
  DEPENDS:= +libblkid +libsmartcols
  SUBMENU=disc
endef

define Package/fdisk/description
 a menu-driven program for creation and manipulation of partition tables
endef

define Package/findfs
$(call Package/util-linux/Default)
  TITLE:=find a filesystem by label or UUID
  DEPENDS:= +libblkid
  SUBMENU=disc
endef

define Package/findfs/description
 findfs will search the disks in the system looking for a filesystem which has
 a label matching label or a UUID equal to uuid
endef

define Package/flock
$(call Package/util-linux/Default)
  TITLE:=manage locks from shell scripts
endef

define Package/flock/description
  manages flock locks from within shell scripts or the command line
endef

define Package/getopt
$(call Package/util-linux/Default)
  TITLE:=parse command options (enhanced)
endef

define Package/getopt/description
 getopt is used to break up (parse) options in command lines for easy parsing
 by shell procedures, and to check for legal options
endef

define Package/hwclock
$(call Package/util-linux/Default)
  TITLE:=query or set the hardware clock
endef

define Package/hwclock/description
 hwclock is a tool for accessing the Hardware Clock
endef

define Package/logger
$(call Package/util-linux/Default)
  TITLE:=a shell command interface to the syslog system log module
endef

define Package/logger/description
 logger makes entries in the system log, it provides a shell command interface
 to the syslog system log module
endef

define Package/look
$(call Package/util-linux/Default)
  TITLE:=display lines beginning with a given string
endef

define Package/look/description
 look utility displays any lines in file which contain string
endef

define Package/losetup
$(call Package/util-linux/Default)
  TITLE:=set up and control loop devices
  DEPENDS:= +libsmartcols
endef

define Package/losetup/description
 losetup is used to associate loop devices with regular files or block devices,
 to detach loop devices and to query the status of a loop device
endef

define Package/lsblk
$(call Package/util-linux/Default)
  TITLE:=list block devices
  DEPENDS:= +libblkid +libmount +libsmartcols
  SUBMENU=disc
endef

define Package/lsblk/description
 lsblk lists information about all or the specified block devices
endef

define Package/lscpu
$(call Package/util-linux/Default)
  TITLE:=display information about the CPU architecture
  DEPENDS:= +libsmartcols
endef

define Package/lscpu/description
 lscpu displays information about the CPU architecture
endef

define Package/mcookie
$(call Package/util-linux/Default)
  TITLE:=generate magic cookies for xauth
endef

define Package/mcookie/description
 mcookie generates a 128-bit random hexadecimal number for use with the X
 authority system
endef

define Package/mount-utils
$(call Package/util-linux/Default)
  TITLE:=related (u)mount utilities
  DEPENDS+= +libmount +libsmartcols
endef

define Package/mount-utils/description
 contains: mount, umount, findmnt
endef

define Package/namei
$(call Package/util-linux/Default)
  TITLE:=follow a pathname until a terminal point is found
endef

define Package/namei/description
 namei uses its arguments as pathnames to any type of Unix file (symlinks,
 files, directories, and so forth)
endef

define Package/rename
$(call Package/util-linux/Default)
  TITLE:=rename files
endef

define Package/rename/description
 rename will rename the specified files by replacing the first occurrence of
 expression in their name by replacement
endef

define Package/partx-utils
$(call Package/util-linux/Default)
  TITLE:=inform kernel about the presence and numbering of on-disk partitions
  DEPENDS:= +libblkid +libsmartcols
  SUBMENU=disc
endef

define Package/partx-utils/description
 contains partx, addpart, delpart
endef

define Package/script-utils
$(call Package/util-linux/Default)
  TITLE:=make and replay typescript of terminal session
  SUBMENU=Terminal
endef

define Package/script-utils/description
 contains: script, scriptreplay
endef

define Package/setterm
$(call Package/util-linux/Default)
  TITLE:=set terminal attributes
  DEPENDS:= +libncurses
  SUBMENU:=Terminal
endef

define Package/setterm/description
 setterm writes to standard output a character string that will invoke the
 specified terminal capabilities
endef

define Package/sfdisk
$(call Package/util-linux/Default)
  TITLE:=partition table manipulator for Linux
  SUBMENU=disc
endef

define Package/sfdisk/description
 list the size of a partition, list the partitions on a device, check the
 partitions on a device and repartition a device
endef

define Package/swap-utils
$(call Package/util-linux/Default)
  TITLE:=swap space management utilities
  DEPENDS+= +libblkid
  SUBMENU:=disc
endef

define Package/swap-utils/description
 contains: mkswap, swaplabel
endef

define Package/uuidd
$(call Package/util-linux/Default)
  TITLE:=UUID generation daemon
  DEPENDS:= +libuuid
endef

define Package/uuidd/description
 The uuidd daemon is used by the UUID library to generate universally unique
 identifiers (UUIDs), especially time-based UUIDs, in a secure and
 guaranteed-unique fashion, even in the face of large numbers of threads
 running on different CPUs trying to grab UUIDs.
endef

define Package/uuidgen
$(call Package/util-linux/Default)
  TITLE:=create a new UUID value
  DEPENDS:= +libuuid
endef

define Package/uuidgen/description
 The uuidgen program creates (and prints) a new universally unique identifier
 (UUID) using the libuuid library. The new UUID can reasonably be considered
 unique among all UUIDs created on the local system, and among UUIDs created on
 other systems in the past and in the future.
endef

define Package/wall
$(call Package/util-linux/Default)
  TITLE:=send a message to everybody's terminal
  SUBMENU=Terminal
endef

define Package/wall/description
 wall sends a message to everybody logged in with their mesg permission
 set to yes
endef

define Package/whereis
$(call Package/util-linux/Default)
  TITLE:=locate the binary, source, and manual page files for a command
endef

define Package/whereis/description
 whereis locates source/binary and manuals sections for specified files
endef

define Package/wipefs
$(call Package/util-linux/Default)
  TITLE:=wipe a signature from a device
  DEPENDS:= +libblkid
  SUBMENU:=disc
endef

define Package/wipefs/description
 wipefs can erase filesystem, raid or partition table signatures (magic
 strings) from the specified device to make the signature invisible for
 libblkid.
endef

define Package/libblkid/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libblkid.so* $(1)/usr/lib/
endef

define Package/libmount/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libmount.so* $(1)/usr/lib/
endef

define Package/libsmartcols/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libsmartcols.so* $(1)/usr/lib/
endef

define Package/libuuid/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libuuid.so* $(1)/usr/lib/
endef

define Package/agetty/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/agetty $(1)/usr/sbin/
endef

define Package/blkid/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/blkid $(1)/usr/sbin/
endef

define Package/cal/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cal $(1)/usr/bin/
endef

define Package/cfdisk/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/cfdisk $(1)/usr/sbin/
endef

define Package/dmesg/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/dmesg $(1)/usr/sbin/
endef

define Package/fdisk/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/fdisk $(1)/usr/sbin/
endef

define Package/findfs/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/findfs $(1)/usr/sbin/
endef

define Package/flock/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/flock $(1)/usr/bin/
endef

define Package/getopt/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/getopt $(1)/usr/bin/
endef

define Package/hwclock/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hwclock $(1)/usr/sbin/
endef

define Package/logger/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/logger $(1)/usr/bin/
endef

define Package/look/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/look $(1)/usr/bin/
endef

define Package/losetup/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/losetup $(1)/usr/sbin/
endef

define Package/lsblk/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/lsblk $(1)/usr/bin/
endef

define Package/lscpu/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/lscpu $(1)/usr/bin/
endef

define Package/mcookie/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/mcookie $(1)/usr/bin/
endef

define Package/mount-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/{u,}mount $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/mountpoint $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/findmnt $(1)/usr/bin/
endef

define Package/namei/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/namei $(1)/usr/bin/
endef

define Package/rename/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rename $(1)/usr/bin/
endef

define Package/partx-utils/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/partx $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/addpart $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/delpart $(1)/usr/sbin/
endef

define Package/script-utils/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/script $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scriptreplay $(1)/usr/bin/
endef

define Package/setterm/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/setterm $(1)/usr/bin/
endef

define Package/sfdisk/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/sfdisk $(1)/usr/sbin/
endef

define Package/swap-utils/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/mkswap $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/swaplabel $(1)/usr/sbin/
endef

define Package/uuidd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/uuidd $(1)/usr/sbin/
endef

define Package/uuidgen/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/uuidgen $(1)/usr/bin/
endef

define Package/wall/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wall $(1)/usr/bin/
endef

define Package/whereis/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/whereis $(1)/usr/bin/
endef

define Package/wipefs/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/wipefs $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,libblkid))
$(eval $(call BuildPackage,libmount))
$(eval $(call BuildPackage,libsmartcols))
$(eval $(call BuildPackage,libuuid))
$(eval $(call BuildPackage,agetty))
$(eval $(call BuildPackage,blkid))
$(eval $(call BuildPackage,cal))
$(eval $(call BuildPackage,cfdisk))
$(eval $(call BuildPackage,dmesg))
$(eval $(call BuildPackage,fdisk))
$(eval $(call BuildPackage,findfs))
$(eval $(call BuildPackage,flock))
$(eval $(call BuildPackage,getopt))
$(eval $(call BuildPackage,hwclock))
$(eval $(call BuildPackage,logger))
$(eval $(call BuildPackage,look))
$(eval $(call BuildPackage,losetup))
$(eval $(call BuildPackage,lsblk))
$(eval $(call BuildPackage,lscpu))
$(eval $(call BuildPackage,mcookie))
$(eval $(call BuildPackage,mount-utils))
$(eval $(call BuildPackage,namei))
$(eval $(call BuildPackage,rename))
$(eval $(call BuildPackage,partx-utils))
$(eval $(call BuildPackage,script-utils))
$(eval $(call BuildPackage,setterm))
$(eval $(call BuildPackage,sfdisk))
$(eval $(call BuildPackage,swap-utils))
$(eval $(call BuildPackage,uuidd))
$(eval $(call BuildPackage,uuidgen))
$(eval $(call BuildPackage,wall))
$(eval $(call BuildPackage,whereis))
$(eval $(call BuildPackage,wipefs))
