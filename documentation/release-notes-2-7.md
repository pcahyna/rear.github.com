---
layout: default
title: Relax-and-Recover Release Notes
---

## This is currently work in progress for the upcoming ReaR 2.7 release
see https://github.com/rear/rear/issues/2751

## Release Notes for Relax-and-Recover version 2.7

This document contains the release notes for the open source project Relax-and-Recover.

[Relax-and-Recover website](http://relax-and-recover.org/)

[GitHub project](https://github.com/rear/)


This document is distributed with the following license: "Creative Commons
Attribution-NoDerivs 3.0 Unported (CC BY-ND 3.0)". To read the license deed,
go to [http://creativecommons.org/licenses/by-nd/3.0/](http://creativecommons.org/licenses/by-nd/3.0/)


## Overview

Relax-and-Recover (abbreviated ReaR) is a GNU/Linux system administrator tool and framework
used to create bootable disaster recovery images which makes bare metal
disaster recovery (including backup restore) easier.
System administrators use the Relax-and-Recover framework to set up a disaster recovery procedure
as part of their disaster recovery policy (which complements their existing backup policy).
Relax-and-Recover does not implement backup but complements it because backup (and restore)
happens via external backup software that is only called by Relax-and-Recover.


### Product Features

The following features are supported by the most recent releases of
Relax-and-Recover. Anything labeled as (*New*) was added as the
most recent release. New functionality for previous releases can be
seen in the next chapter that details each release.

The most recent release of Relax-and-Recover is supported on most GNU/Linux
based systems with kernel 2.6 or higher. It provides the following
functionality:

* Hot maintenance capability. A recovery/rescue image can be made online while
  the system is running

* Command line interface. Relax-and-Recover does not require a graphical
  interface to run, neither in creation mode, nor in recovery mode (console
  is enough)

* Support included for most common file systems, such as ext2, ext3, and ext4.
  Other filesystems like reiserfs, jfs, xfs, and btrfs are also implemented,
  but are less tested. _(Feedback is appreciated)_

* Selected Hardware RAID and (eg. HP SmartArray) and mirroring solutions (eg.
  DRBD) are supported

* NVME and mmcblk disks are supported

* LVM root volumes are supported

* Multipath support for SAN storage

* UEFI support (including UEFI USB booting)

* Integrates with _internal_ backup programs such as:

   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar)
   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_TYPE=incremental, FULLBACKUPDAY="Mon") for using incremental backups with a weekly full backup (old tar archives will not be removed automatically)
   - GNU tar (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_TYPE=differential, FULLBACKUPDAY="Mon") for using differential backups with a weekly full backup (old tar archives will not be removed automatically)
   - GNU tar with openssl encryption (BACKUP=NETFS, BACKUP_PROG=tar, BACKUP_PROG_CRYPT_ENABLED=1)
   - rsync on local devices (BACKUP=NETFS, BACKUP_PROG=rsync), such USB and local disks
   - rsync over the network (BACKUP=RSYNC, BACKUP_PROG=rsync)
   - Multiple backup methods. See the [documentation about Using Multiple Backups for Relax-and-Recover](https://github.com/rear/rear/blob/master/doc/user-guide/11-multiple-backups.adoc)
   - Any partition (e.g. a Windows partition) via BACKUP=BLOCKCLONE. See the [documention about BLOCKCLONE](https://github.com/rear/rear/blob/master/doc/user-guide/12-BLOCKCLONE.adoc)
   - BACKUP=ZYPPER is for SUSE systems only (*Experimental*)
   - BACKUP=YUM is for RedHat architectures ony (*Experimental*)

* Integrates with _external_ backup solutions such as:

  - Tivoli Storage Manager (BACKUP=TSM)
  - Data Protector (BACKUP=DP)
  - Symantec NetBackup (BACKUP=NBU)
  - Galaxy 5, 6, and 7 (BACKUP=GALAXY)
  - Galaxy 10 [Commvault Simpana] (BACKUP=GALAXY10)
  - Bacula (BACKUP=BACULA)
  - Bareos (BACKUP=BAREOS) (A fork of Bacula)
  - Rsync Backup Made Easy (BACKUP=RBME)
  - Duplicity/Duply (BACKUP=DUPLICITY)
  - EMC Networker, also known as Legato (BACKUP=NSR)
  - EMC Avamar (BACKUP=AVA)
  - SEP Sesam (BACKUP=SESAM)
  - FDR/Upstream (BACKUP=FDRUPSTREAM)
  - Novastor NovaBACKUP DC (BACKUP=NBKDC)
  - Borg Backup (BACKUP=BORG)
  - Rubrik Cloud Data Management (BACKUP=CDM)

* Integrates with [Disaster Recovery Linux Manager (DRLM)](http://drlm.org)

* Udev support (except for some really ancient udev versions) which is
  the base for many important features:

  - kernel drivers for network cards and storage adapters are loaded via udev
  - deal with network persistent names in udev rules
  - firmware loading
  - persistent storage device names (though Relax-and-Recover does nothing with this)

* Systemd support for the more recent Linux distributions

* System migration and reconfiguration ('MIGRATION_MODE')

  - facilitate recovery on hardware, that is not the same as the original system
  - network and storage drivers are adjusted
  - map hard disks if they do not match (e.g. sda -> sdb)
  - remap network MAC addresses
  - use another IP address, or using dhcp via templates or from kernel command line
  - rebuild the initial ramdisk if needed (for new storage drivers)
  - migration to SAN storage

* Support backup software: Bacula, both locally attached tapes (with
  bextract) and network-based backups. Also, in combination with OBDR tapes.

* Create OBDR tapes with method `mkbackup` and put the backup onto the tape
  to have a single-tape bootable recovery solution (label the OBDR tape
  with the method `format` to avoid accidental overwrites with OBDR)

* Create bootable disk (eSATA, USB ...) medium with the backup included:

    BACKUP_URL=usb:///dev/device

    Together with `OUTPUT=USB` we have a complete solution on hard disks
    (booting of it and restoring data).

* DHCP client support (IPv4 and IPv6). Dhcp client activation
  can be forced via the variable *USE_DHCLIENT=yes* (define in _/etc/rear/local.conf_).
  It is also possible to force DHCP at boot time with kernel option `dhcp`

* `USE_STATIC_NETWORKING=y`, will cause statically configured network settings to be applied even when `USE_DHCLIENT` is in effect

* Save layout and compare layouts for automation of making
  Relax-and-Recover snapshots (checklayout option)

* External USB booting uses extlinux (instead of syslinux), and
  therefore, the USB disk must first be formatted with an ext2, ext3, ext4
  or btrfs based file system

* VLAN tagging, teaming and bridge support

* Add timestamp of ReaR run with rc code to the syslog or messages file; sending mail report is also possible

* The possibility to backup any partition (in particular a Windows partition) via the BACKUP type BLOCKCLONE

* Unattended ReaR recovery has been improved

* Improved security model related to SSH keys

  - SSH_FILES='avoid_sensitive_files' (see details in _/usr/share/rear/conf/default.conf_)
  - SSH_UNPROTECTED_PRIVATE_KEYS='no' (see details in _/usr/share/rear/conf/default.conf_)

*NOTE*: Features marked *Experimental* are prone to change with future releases.

## Relax-and-Recover Releases

The first release of Relax-and-Recover, version 1.0, was posted to the web in July 2006.
For each release, this chapter lists the new features and defect fixes.
All releases are cumulative.
Unless otherwise noted all releases of Relax-and-Recover are intended to work backward compatible with previous versions.
In addition to the GPL disclaimer of warranty and liability there is no guarantee that things work backward compatible.
In general the older the system is the less likely it is that a newer Relax-and-Recover version works.
For each Relax-and-Recover version upgrade and for each change of a software that is used by Relax-and-Recover and
for each change of your basic system you must re-validate that your disaster recovery procedure still works for you.

The references pointing to *fix #nr* or *issue #nr* refer to our [GitHub issues tracker](https://github.com/rear/rear/issues).

### Version 2.7 (perhaps March 2022 or later cf. https://github.com/rear/rear/issues/2751)

#### Abstract

New features, bigger enhancements, and possibly backward incompatible changes:

* to be done



#### Details (mostly in chronological order - newest topmost):

* to be done




### Version 2.6 (June 2020)

#### Abstract

New features, bigger enhancements, and possibly backward incompatible changes:

* Initial preliminary first basic support for IBM Z architecture "s390x"
(a.k.a. "z Systems", formerly "System/390", simply called "s390") :
Currently primarily for Red Hat and SUSE enterprise systems
so that interested users can try out early how far things work
in their particular IBM Z environments and provide feedback
preferably plus needed fixes, adaptions, and enhancements
to ReaR upstream at https://github.com/rear/rear
(issues #2137 #2142).

* New BACKUP=CDM method to support Rubrik Cloud Data Management (CDM):
The Rubrik CDM backup and restore method for ReaR allows Rubrik CDM
to perform bare metal recovery of Linux systems.
It does this by including the Rubrik CDM RBS agent files in the
ReaR recovery system so that during `rear recover` the Rubrik CDM RBS agent
can be used to restore from Rubrik CDM (issue #2248).

* New use-case for BLOCKCLONE backup method with the "multiple backups" approach
for a more complex LUKS setup with different passphrases or keys:
The usual support for LUKS-encrypted filesystems means that during `reare recover`
a new encrypted filesystem will be created with new encryption keys.
To recover the exact LUKS setup including its keys from the original system
it is now possible to use the "multiple backups" approach by first ignoring
the encrypted filesystem during the "base system" phase, and then
using a second BLOCKCLONE phase with a `dd` image that contains
the encrypted filesystem. The new config variable BLOCKCLONE_TRY_UNMOUNT
is needed here: It will try to unmount the encrypted filesystem
before creating its image and before restoring it (issue #2200).

* Initial draft implementation of the new 'mountonly' workflow
to use ReaR as rescue system, therein mount the filesystems
of the target system so that one can manually repair it.
This is described in doc/user-guide/04-scenarios.adoc
(issue #2247).

* Enable creation of non consecutive partitions.
`parted` is not capable of creating non-consecutive partitions.
To still be able to do so, the trick consists in creating dummy partitions
to fill the gaps between partition numbers. Allocation of these dummy partitions
is done from the end of the target partition, because parted is not capable
of resizing a partition from the beginning (issues #2081 #1793 #1771 #1681).

* Improved handling of partition label type for multipath disks plus
including of multipath disks in backup (unless AUTOEXCLUDE_MULTIPATH is true).
Now the partition label type for multipath devices (same as is already done
for normal disks) is stored in 'multipath' entries in disklayout.conf
so that the syntax of those entries changed and that new syntax is now
documented in doc/user-guide/06-layout-configuration.adoc (issues #2234 #2236).

* The config variable WAIT_SECS default value was aligned
with the USER_INPUT_TIMEOUT default value so that now
the default timeout for user input is always 300 seconds.
The former WAIT_SECS default value of 30 seconds was too short
because the 'read -t' timeout interrupts ongoing user input.
The timeout must be sufficiently long for the user to read and
understand the possibly unexpected user input request message
and then some more time to make a decision what to enter and
finally even more time to enter the right value (issue #2257).

* Enhanced default AUTOEXCLUDE_PATH=( /media /run /mnt /tmp )
to automatically exclude temporarily mounted things (e.g. USB devices)
because mountpoints for temporarily mounted things are usually
sub-directories below /media /run (like /run/media) /mnt
and /tmp (issue #2239).

* Now we have CLONE_ALL_USERS_GROUPS="true" by default.
See the explanation in default.conf what that means.
(issue #2345)

* Replaced grub-mkimage with more sophisticated grub-mkstandalone when
building GRUB2 image for using GRUB2 as recovery system UEFI bootloader
(issue #2293).

#### Details (mostly in chronological order - newest topmost):

* Added DHCP auto-configuration for Network Manager (Ubuntu 20.04 Desktop):
While ReaR supports DHCP auto-detection during 'rear mkrescue' of some DHCP setups,
the Network Manager flavor on Ubuntu 20.04 Desktop was not supported
so DHCP for the recovery system was not auto-enabled there
(issue #2427).

* OPALPBA (OUTPUT=RAWDISK): Workaround for EFI firmware glitch
which prevents booting the real OS after unlocking SEDs.
Symptom: Boot hangs when Grub hands over control to the Linux kernel.
Seen on an HPE ML10Gen9 server. The workaround is to add and remove
a UEFI 'next boot' entry (essentially a no-op) which should convince
the EFI firmware to use the boot order to determine the effective
boot entry for the next boot
(issue #2425).

* Update default.conf:
Describe LUKS_CRYPTSETUP_OPTIONS usage with some examples
(issue #2389).

* Overhauled how SSH config files are parsed for 'IdentityFile' values
to find (and remove) unprotected SSH keys in the recovery system.
Now "find ./etc/ssh" ensures that SSH 'Include' config files
e.g. in /etc/ssh/ssh_config.d/ are also parsed
(issue #2421).

* Update default.conf:
Describe usage of non-quoted globbing patterns like VAR+=( /directory/* )
versus quoted globbing patterns like VAR+=( '/directory/*' )
and also describe how to prepend to an array in contrast to append to it
(issue #2417).

* When creating md5sums of the files in the recovery system
be safe against blanks or special characters in file names
by using appropriate options for all commands in the pipe
(issues #2407 #1372).

* Evaluate OUTPUT_LFTP_OPTIONS at the beginning of lftp operations.
Before OUTPUT_LFTP_OPTIONS had been evaluated after connection
to destination host was established which prohibited using
OUTPUT_LFTP_OPTIONS also for connection specific settings.
Now OUTPUT_LFTP_OPTIONS is moved to the beginning of lftp command
before any other lftp command is executed
(issue #2410).

* For BACKUP=BORG implemented pagination when selecting the Borg archive
to restore from: During "rear recover" all Borg archives are shown.
In case of a long list only the last items had been visble to the user.
Now pagination happens with a configurable number of Borg archives,
see BORGBACKUP_RESTORE_ARCHIVES_SHOW_MAX in default.conf
(issue #2408).

* For BACKUP=BORG updated generated locale filename
from "rear.UTF-8" to "en_US.UTF-8"
(issue #2402).

* Use plain ${COPY_AS_IS[*]} instead of quoted "${COPY_AS_IS[@]}"
in the tar command call that copies things into the recovery system
to ensure "things work as usually expected" for any methods
that are used to add elements to the COPY_AS_IS array and
better explain in default.conf how COPY_AS_IS works,
in particular that symlinks cannot be followed and
that files or directories that contain blanks or
other $IFS characters cannot be specified
(issue #2405).

* For RAWDISK output add distribution-specific GRUB2 module 'linuxefi.mod'
otherwise GRUB2 could not boot a UEFI rescue medium on Ubuntu 18.04 or 20.04
(issue #2419).

* ReaR was using hard-coded set of Grub2 modules for UEFI boot-loader.
New GRUB2_MODULES_UEFI and GRUB2_MODULES_UEFI_LOAD config variables
for installing GRUB2 as recovery system UEFI bootloader
so that user can add or remove GRUB2 modules as needed
(issues #2283 #2293 #2392).

* No longer load GRUB2 modules efi_gop and efi_uga in function create_grub2_cfg :
In the create_grub2_cfg function in lib/bootloader-functions.sh
do no longer enfore loading the GRUB2 modules efi_gop and efi_uga
because loading only the module all_video is sufficient and fail-safe
because moddep.lst contains "all_video: efi_gop efi_uga"
(issue #2388).

* No longer load video_bochs and video_cirrus by the create_grub2_cfg function.
In the create_grub2_cfg function in usr/share/rear/lib/bootloader-functions.sh
that is used in case of UEFI to set up GRUB2 as bootloader for the recovery system
do no longer let GRUB2 load the modules video_bochs and video_cirrus
because those are not available as GRUB2 modules in case of UEFI (x86_64-efi)
and the generic "insmod all_video" that is still there should be sufficient for GRUB2
(issue #2388).

* Allow to boot original system from Grub menu (UEFI):
This change adds following:
Possibility to boot original system for UEFI boot with OUTPUT=NETFS
and OUTPUT=USB (similarly to non UEFI ReaR rescue system).
Replaces grub-mkimage for building of Grub2 boot image for OUTPUT=USB
with build_bootx86_efi() (grub-mkstandalone).
Replaces separate Grub configuration for OUTPUT=USB with create_grub2_cfg().
Using build_bootx86_efi() and create_grub2_cfg() in OUTPUT=USB will
unify process of Grub boot image creation with OUTPUT=NETFS.
(issue #2326)

* Replace grub-mkimage with more sophisticated grub-mkstandalone when
building GRUB2 image for using GRUB2 as recovery system UEFI bootloader
(issue #2293).

* When transferring ReaR recovery system ISO,
create also destination directory structure (including parents).
This is useful because when destination directory structure does not exist
(or is incomplete), Lftp puts files into first available directory in OUTPUT_URL scheme
(issue  #2401).

* Have CLONE_ALL_USERS_GROUPS="true" by default and explain it in default.conf
(issue #2345).

* New OUTPUT_LFTP_OPTIONS config variable for lftp custom parameters
(issue #2384).

* Update 950_check_missing_programs.sh:
When checking for required programs also test for "basename program"
because when required programs are specified with absolute path
those programs appears in the ReaR recovery system in /bin/
so testing their original path would falsely fail during "rear recover"
(issue #2206).

* New prep/USB/<non-i386-arch>/350_safeguard_error_out.sh
safeguard scripts to let "rear mkrescue/mkbackup" error out
in case of false usage of OUTPUT=USB on non PC-compatible
(non-i386/x86/x86_64) architectures because with OUTPUT=USB
on those architectures the USB medium cannot be booted
(for those architectures there are no scripts that install a bootloader)
and documented that in the OUTPUT=USB section in default.conf
(issues #2348 #2396).

* Error out for unsupported workflows in any case:
Before init/default/050_check_rear_recover_mode.sh did only error out
when in the recovery system an unsupported workflow should be run.
Now it also errors out when on the normal/original system
an unsupported workflow that is likely destructive
(in particular recover layoutonly restoreonly finalizeonly and mountonly)
should be run
(issues #2387 #2395).

* Improve BorgBackup (short: Borg) integration into ReaR:
Several improvements e.g. error handling, output to logfile etc.
(issue #2382).

* In backup/NETFS/default/500_make_backup.sh add "bs=1M" to
SPLIT_COMMAND="dd of=$backuparchive bs=1M"
to let 'dd' read and write up to 1MiB at a time to speed up things
for example from only 500KiB/s (with the 'dd' default of 512 bytes)
via a 100MBit network connection to about its full capacity
(issue #2369).

*  Error out during "rear mkrescue/mkbackup" when LUKS version 2 is used
because LUKS version 2 is not suppported.
When LUKS version 2 is used it fails at least to determine the hash value
so we use an empty hash value as a simple test if gathering crypt information
was successful and error out if not.
(issues #2204 #2381)

* Auto-detect DHCP client with systemd-networkd
plus completely overhauled and simplified code in
prep/GNU/Linux/210_include_dhclient.sh
with resulting simplification of
skel/default/etc/scripts/system-setup.d/58-start-dhclient.sh
(issue #2375).

* Filter out duplicate entries in COPY_AS_IS
but keep the ordering of the elements
i.e. only the first occurrence of an element is kept.
Also remove duplicates in the copy_as_is_filelist_file
with 'sort -u' because here the ordering does not matter.
(issue #2377)

* Update 250_find_all_libs.sh:
Removed unreliably working code that intends to filter out
duplicates in the LIBS and COPS_AS_IS arrays via
echo "${ARRAY[@]}" | tr ' ' '\n' | sort -u | tr '\n' ' '
that fails when array elements contain spaces
so better code should be used e.g. like
printf '%s\n' "${ARRAY[@]}" | awk '!seen[$0]++'
but filtering out duplicates is not needed
and by the way cleaned up the whole script a bit.
(issue #2377)

* Skip Longhorn Engine replica devices (issue #2373).

* Update 400_prep_yum.sh:
For BACKUP=YUM error out if BACKUP_PROG_CRYPT_ENABLED is set
because BACKUP=YUM does not support backup encryption
(issue #2374).

* Fail-safe patching of etc/ssh/sshd_config in the recovery system
to ensure the needed ReaR settings/overrides actually apply.
For example if etc/ssh/sshd_config contains a Match section
at the end of the file e.g. appending "PermitRootLogin yes"
will not be effective as a global setting but only in the Match section.
Therefore now the needed ReaR settings are inserted at the beginning
and all subsequent entries of that kind are disabled (commented out)
to ensure that the ReaR global setting is the only one that is effective.
(issue #2362)

* Run what was build/default/985_fix_broken_links.sh
now earlier as build/default/490_fix_broken_links.sh
before files get modified in recovery system because
otherwise e.g. if etc/ssh/sshd_config is a symbolic link
build/default/500_ssh_setup.sh will fail to apply patches
(issue #2360).

 * Use simpler and more fail-safe syntax
 ARRAY+=( additional elements )
 everywhere (issue #2364).

* Update 58-start-dhclient.sh: Fixed issue #2354.

* Added usr/share/rear/restore/OPALPBA/ directory
to pass the test for $SHARE_DIR/restore/$BACKUP
in prep/default/035_valid_backup_methods.sh
because the mkopalpba workflow uses the nonexistent
BACKUP method OPALPBA to create a PBA image
(issue #2351).

* Fix OPALPBA: Could not change password on multiple SEDs at once.
"rear opaladmin changePW" (when used without any extra argument
or with multiple arguments) is meant to change the disk password
on multiple self-encrypting drives in a single invocation.
However, it stopped after successfully changing the password on the first drive
and the password of subsequent drives had not been changed. 
(issue #2349)

* Update 035_valid_backup_methods.sh:
Simplified prep/default/035_valid_backup_methods.sh
to make it work more predictable and fail-safe
(issue #2353).

* Have a hardcoded '-iso-level 3' option in
output/ISO/Linux-ppc64le/820_create_iso_image.sh
because it is also hardcoded in
output/ISO/Linux-i386/820_create_iso_image.sh
and it seems to also work in general on PPC64LE architecture
(issue #2344).

* Update default.conf: Adapt the explanation in default.conf how ISO_MAX_SIZE works
(issue #2347).

* Cleaned up and enhanced the ISO_MAX_SIZE implementation
in backup/NETFS/default/500_make_backup.sh to make it in particular more safe
against erroneous settings of the ISO_MAX_SIZE variable
(issue #2344).

* Added required NFS users for proper rpcbind startup: Usually 'rpcuser' is used
but 'rpc' is used in RHEL7.x and '_rpc' is used in Debian 10
(issues #2341 #2342 #2250)

* Cleaned up and enhanced 320_migrate_network_configuration_files.sh
plus some bugfixes where the old code presumably did not work.
(issues #2310 #2313 #2312)
   
* Update 890_finish_checks.sh: In the WARNING message at the end of "rear recover"
when no bootloader could be installed also mention
usr/share/rear/finalize/Linux-ppc64le/660_install_grub2.sh
as an example script how one could install a bootloader on POWER architecture.
Furthermore since finalize/default/110_bind_mount_proc_sys_dev_run.sh
it is no longer needed to manually mount /proc.
(issues #2339 #2045)

* Update 095_exclude_non_essential_files.sh: Added also /usr/lib/grub2
and /usr/share/grub2 to COPY_AS_IS_EXCLUDE because since openSUSE Leap 15.1
things were moved from /usr/lib/grub2/ to /usr/share/grub2/
(issue #2338).

* Update 270_create_grub2_efi_bootloader.sh: Test for Grub 2 EFI
components directories /usr/lib/grub/x86_64-efi and now also for
/usr/lib/grub2/x86_64-efi or /usr/share/grub2/x86_64-efi
because since openSUSE Leap 15.1 things were moved
from /usr/lib/grub2/ to /usr/share/grub2/
(issue #2338).

* Update uefi-functions.sh: Use /usr/*/grub*/x86_64-efi/partmap.lst
instead of /usr/lib/grub*/x86_64-efi/partmap.lst because since
openSUSE Leap 15.1 things were moved from /usr/lib/grub2/ to /usr/share/grub2/
(issue #2338).

* Update 630_install_grub.sh and 650_install_elilo.sh:
Show the actual missing directory in the Error message
(issue #2337).

* Moved backup/default/005_valid_backup_methods.sh
to prep/default/035_valid_backup_methods.sh to also
check for valid backup methods during "rear mkrescue"
because for most external backup methods only "rear mkrescue" is used,
cf. the section "BACKUP SOFTWARE INTEGRATION" in 'man rear'
(issue #2337).

* Changes to Data Protector branding, adjustment required to support
the new Data Protector 10.x agent with Secure Socket Communication.
Fixed some problems related to SessionID format and problems with
object names like host type vs. file system backup
(issue #2335).

* Hard-coded root home directory string ("/root") was replaced
with $ROOT_HOME_DIR that is the eeal home directory of root user
(issue #2334).

* Make USE_DHCLIENT and USE_STATIC_NETWORKING more fail-safe
in ReaR recovery system (issue #2325).

* For FDR backups, if the path for $FDRUPSTREAM_DATA_PATH does not exist,
then create it. This change is FDR specific for s390 and is only in effect
if ZVM_NAMING is "Y" (issue #2320).

* Overhauled rescue/default/010_merge_skeletons.sh:
Made 010_merge_skeletons.sh behave more reliably. 
Now it errors out when things really went wrong.
Now is is also supported that both a $skel_dir directory
(e.g. usr/share/rear/skel/default/) plus a $skel_dir.tar.gz
(e.g. usr/share/rear/skel/default.tar.gz) exist and
then both get copied into the recovery system,
first the directory and then the tar.gz so that
via the tar.gz files from the $skel_dir directory
could be overwritten if needed.
(issues #2307 #2317)

* Added $FDRUPSTREAM_DATA_PATH/rear to $COPY_AS_IS_EXCLUDE_FDRUPSTREAM
(issue #2318).

* Added documentation about possible (dirty) workarounds 
for needed bind9-export libraries for CentOS 7.7 and 8.0
which is somewhat related to Rubrik-CDM
(issues #2266 #2284).

* Enhanced recovery system BIOS boot default settings for USB and ISO:
For OUTPUT=ISO the user can now explicitly specify
what to boot by default when booting the ISO on BIOS systems via
ISO_DEFAULT="boothd0" to boot from the first disk and
ISO_DEFAULT="boothd1" to boot from the second disk.
For OUTPUT=USB the user can now explicitly specify
what to boot by default when booting the disk on BIOS systems via
USB_BIOS_BOOT_DEFAULT="boothd0" to boot from the first disk.
The default USB_BIOS_BOOT_DEFAULT="" boots the second disk.
(issues #2276 #2303)

* Remove outdated /root/rear-DATE-TIMESTAMP.log symlinks.
Before creating the current symlink to the current log file
test all files that match the file name pattern of such symlinks
and remove those already existing (old) symlinks where its
now outdated symlink target is the current log file
(issue #2301).

* Initial preliminary first basic support for IBM Z architecture "s390x"
(a.k.a. "z Systems", formerly "System/390", simply called "s390") :
Currently primarily for Red Hat and SUSE enterprise systems
so that interested users can try out early how far things work
in their particular IBM Z environments and provide feedback
preferably plus needed fixes, adaptions, and enhancements
to ReaR upstream at https://github.com/rear/rear
(issues #2137 #2142).

* Improved 'is_multipath_path' function to be more fail safe
that now tests (via "multipath -l" output) if multipath is used.
Additionally have 'lsblk' output as disklayout.conf header comments
to make it easier to understand the values in the subsequent entries.
Furthermore added 'xdd' to the PROGS array because sometimes
a tool to display binary files is required in the recovery system.
(issue #2298).

* Error out during "rear mkrescue" in case of insufficient LVM tools:
Overhauled layout/save/GNU/Linux/220_lvm_layout.sh
where now the exit code of the "lvm ..." calls are checked
and it errors out if one fails in particular to avoid that
entries in disklayout.conf are missing or broken
when too old LVM tools are used that do not support
the needed options of those "lvm ..." calls.
Also in layout/save/default/950_verify_disklayout_file.sh
a simple test was added to verify that the 'lvm...' entries
in disklayout.conf look syntactically correct.
Furthermore in 06-layout-configuration.adoc in the
"Disk layout file syntax" section the exact same syntax
for the "lvm..." entries is now listed as the headers
of the "lvm..." entries in disklayout.conf are.
(issues #2259 #2291)

* Fixed missing delete_dummy_partitions_and_resize_real_ones calls:
Use same code as for disks for multipath devices (these are just regular disks),
including MBR erasing and partition creation and cleanup : create_multipath()
cannot call create_disk() because create_disk() verifies that the device
is a block device, which it isn't with multipath because it's a symlink
to a device mapper instead. Make sure new code handling re-creation
of partitions and cleanup is used because Software Raid can have partitions
since these are virtual disks.
(issue #2281)

* Moved 'route' back from REQUIRED_PROGS to PROGS.
ReaR uses 'ip route' everywhere so 'route' is not actually required.
As 'route' is no longer always installed nowadays, having it
in REQUIRED_PROGS lets ReaR falsely error out on such systems
(issues #1961 #1652).

* Update 990_verify_rootfs.sh: Use a FDRUPSTREAM-specific
LD_LIBRARY_PATH to find FDR libraries (issue #2296).

* Do not run 'ldd' on untrusted files to mitigate
possible ldd security issues because some versions
of ldd may directly execute the file (see "man ldd")
which happens as user 'root' during "rear mkrescue".
The new TRUSTED_FILE_OWNERS user config array
contains user names that are trusted owners of files
where RequiredSharedObjects calls ldd (cf. COPY_AS_IS)
and where a ldd test is run inside the recovery system
that tests all binaries for 'not found' libraries.
Furthermore use '2>>/dev/$DISPENSABLE_OUTPUT_DEV'
at more places to avoid that the "rear -D mkrescue" log
file size would grow from about 5 MiB to about 17 MiB
so that now that log file size even shrinked to about 2 MiB.
(issue #2279)

* Initial draft implementation of the new 'mountonly' workflow
to use ReaR as rescue system, therein mount the filesystems
of the target system so that one can manually repair it.
This is described in doc/user-guide/04-scenarios.adoc
(issue #2247).

* Feature RAWDISK and OPALPBA improvements:
RAWDISK: include additional Grub modules from
/boot/grub (and /boot/grub2) which had formerly been missing.
OPALPBA: improve Plymouth boot animation on Ubuntu,
provide integration capabilities for other distros.
RAWDISK: add support for distros which use 'grub2' naming.
(issue #2275)

* Improved check for missing libraries in 990_verify_rootfs.sh
so that now also libraries are checked that are no executables
plus skipped the ldd test for firmware files
(issue #2279).

* Update default.conf: More explanatory comment in default.conf
how COPY_AS_IS versus LIBS, PROGS, and REQUIRED_PROGS are meant to be used
(issue #2278).

* Accommodate logs from multiple FDR/Upstream services.
Users may run multiple FDR/Upstream services on a single machine.
This code change allows ReaR to copy FDR/Upstream logs from the
recovery environment to the restored system for all running services.
Previously the code was expecting to find only one set of logs
and reports in the FDR/Upstream installation directory.
For several years now, logs and reports have been stored in a new
location ($FDRUPSTREAM_DATA_PATH), so we check here instead
(issue #2251).

* Update format-workflow.sh: Set EXIT_FAIL_MESSAGE=0 before exiting
in "rear format -- --help" to avoid the
"rear format failed, check ...rear...log for details"
message that is pointless in this case.

* Added alternative for 'poweroff.com' that is 'poweroff.c32' on RHEL8
(issue #2238).

* Removed unnecessary line that sets NSRSERVER
in layout/save/NSR/default/650_check_iso_recoverable.sh
which prevented setting NSRSERVER in local.conf 
(issue #2162).

* Enhanced default AUTOEXCLUDE_PATH=( /media /run /mnt /tmp )
plus explanatory comment in default.conf how AUTOEXCLUDE_PATH works
(issues #2239 #2261).

* Use is_true() for AUTOEXCLUDE_MULTIPATH and
cleanup of the get_partition_number function
that now errors out with BugError in cases that are
likely a bug in ReaR (issues #2245 #2260).

* Do not abort in case of invalid user input but retry if possible.
When input values can be (syntactically) validated there is now
a retry loop until the user provided valid input (or intentionally aborts)
except those cases where it was not possible to implement validation
when it is unknown what is valid for third-party tools (issue #2253).

* Removed '2>/dev/null' redirection where possible: 
Some general cleanup for ReaR 2.6. in all scripts 
by removing '2>/dev/null' where it makes sense 
and replacing '&>/dev/null' by '1>/dev/null' where it makes sense 
or also removing '&>/dev/null' where that seems to be better (issue #1395).
By the way verify/TSM/default/390_request_point_in_time_restore_parameters.sh
and verify/NBU/default/390_request_point_in_time_restore_parameters.sh 
have been somewhat overhauled but both scripts still abort 'rear recover'
when the user did an invalid date or time input (issue #2253).

* Fix including of multipath disks in backup: 
The AUTOEXCLUDE_DISKS logic traverses filesystems and tries 
to determine the associated underlying disks that should be kept. 
When having a filesystem fs:/A that is present on a multipath 
device MP and the root filesystem fs:/ that is present on a normal 
disk D, the code first established that fs:/A depends on fs:/ 
resulting in marking D as used and then short-circuited processing 
of multipath devices. This resulted in MP not being considered 
as a used device and removed from the backup. 
The problem is fxed by removing the short-circuit logic in 
find_disk_and_multipath() so multipath devices get processed 
too unless AUTOEXCLUDE_MULTIPATH is true (issue #2236).

* Improve handling of partition label type for multipath disks: 
When recording information about a multipath disk, 
ReaR did not store information about its partition label type 
in the 'multipath' entry in disklayout.conf. 
The recovery code tried to automatically detect the label type 
using a heuristic that depends on GPT partition names. 
The logic would incorrectly detect the device as having the 
MBR label type instead of GPT if one of the partition names 
was exactly "primary", "extended" or "logical". 
The problem is solved by explicitly storing the partition label type 
for multipath devices (same as is already done for normal disks) 
so that the syntax of the 'multipath' entries in disklayout.conf 
is changed and that new syntax is now documented in 
doc/user-guide/06-layout-configuration.adoc 
The recovery code is accordingly updated to utilize this
information (issue #2234).

* In doc/user-guide/16-Rubrik-CDM.adoc updated links to SUSE documentation: 
Replaced the outdated link 
https://www.suse.com/documentation/sle-ha-12/book_sleha/data/cha_ha_rear.html 
that gets redirected to a not really helpful standard fallback page 
by the currently valid URL for the most recent SLE-HA-15-SP1 
https://documentation.suse.com/sle-ha/15-SP1/html/SLE-HA-all/cha-ha-rear.html 
because meanwhile the SUSE documentation was moved to its own server. 
The more general and more comprehensive documentation URL 
https://en.opensuse.org/SDB:Disaster_Recovery 
is kept and listed topmost (issue #2255).

* Added explanatory comments to default.conf about backup restore
that may fail for things like BACKUP_PROG_COMPRESS_OPTIONS=("--zstd")
due to missing zstd binaries in the ReaR recovery system
probably also for --lzip --lzma --lzop (issue #2241).

* New BACKUP=CDM method for Rubrik Cloud Data Management (CDM)
see doc/user-guide/16-Rubrik-CDM.adoc (issues #2248 #2249).

* In verify/NETFS/default/050_start_required_nfs_daemons.sh 
all kind of '2>/dev/null' (i.e. also '&>/dev/null') are removed 
(so that '&>/dev/null' is replaced by '1>/dev/null') 
because in general '2>/dev/null' is unhelpful
because it needlessly suppresses error messages in the log 
that would be helpful to see when something fails (issues #2250 #1395).

* Update 06-layout-configuration.adoc: Changed section title
from "Including/Excluding components" to only "Excluding components".
The latter avoids possible misunderstanding that there would be
a config variable to explicitly include something but "including" here
describes only to disable autoecludes (issue #2229).

* Update default.conf: At AUTOEXCLUDE_DISKS removed the comment
`Explicitly excluding/including devices is generally a safer option`
because it is not acually helpful and even misleading because there is
currently no config variable to explicitly include devices (issue #2229).

* Do not keep the build dir when ReaR is run noninteractively
by additional special values for the KEEP_BUILD_DIR config variable,
see its desctiption in default.conf (issue #2218).

* Fix LVM2 thin pool recreation logic / use of vgcfgrestore is broken:
Removing forcibly (with '--force' passed twice) seems to work for now.
But our use of vgcfgrestore is probably not appropriate at all.
It works by chance. Typically, it works only for Linear volumes,
and won't probably for Caches and Raid hierarchies
or when there are existing Snapshots on the system.
The only proper solution is perhaps stop relying on vgcfgrestore at all,
but then we are not capable of restoring volume groups and logical volumes
with all properties from original system (issue #2222).

* Use `mountpoint` instead of `mount | grep` (issue #2225).

* Updated default.conf:
Replaced `ARRAY=( "${ARRAY[@]}" additional elements )`
with simpler and more fail safe `ARRAY+=( additional elements )`
(related to issues #2223 #2220).

* Append to the CLONE_USERS and CLONE_GROUPS arrays by using `+=`
instead of expanding the previous value to an empty element
which then causes problems later (issues #2223 #699).

* Update 900_clone_users_and_groups.sh:
Skip empty user and group values (issue #2220).

* New use-case for BLOCKCLONE backup method for complex LUKS-encrypted filesystems
by usinng the "multiple backups" approach with a second BLOCKCLONE phase with a `dd` image that contains the encrypted filesystem where unmounting the encrypted filesystem
before creating its image and before restoring it has to be done via the new
config variable BLOCKCLONE_TRY_UNMOUNT (issue #2200).

* Fixed when ReaR does not detect an uncompressed kernel
that is named 'vmlinux' (instead of 'vmlinuz').

* Added $BACKUP_OPTIONS to the line with mount_url in 800_copy_to_tftp.sh
and 810_create_pxelinux_cfg.sh otherwise in case of OUTPUT=PXE
mounting the NFS will fail.

* UEFI: Search for Grub2 modules in /usr/lib/grub*/x86_64-efi and not in /boot.
On Fedora and RHEL systems, Grub2 UEFI modules live in
/usr/lib/grub*/x86_64-efi, not /boot, unless grub2-install is executed,
but executing this tool is not needed with UEFI.
On SUSE systems, Grub2 UEFI modules also live in /usr/lib/grub*/x86_64-efi
but there is also a copy in /boot, so it's not needed searching in /boot at all.
Additionally, only UEFI modules should be looked for,
so /boot cannot be searched but only /boot/grub2/x86_64-efi
(similarly /usr/lib/grub*/x86_64-efi, not just /usr/lib/grub*),
otherwise we could get some false positives on dual boot systems
(UEFI + Legacy), since modules for Legacy will also match,
which is wrong (issues #2199 #2001). 

* RAWDISK and TCG Opal 2 Self-Encrypting Disks: Add Secure Boot support (issue #2166).

* Avoid that disklayout.conf contains duplicate 'lvmvol' lines:
Create the 'lvmvol' lines commented out when multiple segments exist for a given LV.
This is not an issue unless Migration Mode is used.
In such case, using 'lvcreate' commands already does best effort
and loses LV information (issues #2194 #2187).
Additionally do not overload the `kval` variable
(that is intended for passing options to lvcreate)
by adding extra keys to it, which are not supported by lvcreate.
Introduce another variable `infokval` for this purpose and print
those unsupported and purely informational keys only in comments (issue #2196).

* Apply layout mappings also to other relevant files 
via layout/prepare/default/320_apply_mappings.sh 
In migration mode apply the disk layout mappings 
not only to disklayout.conf but also to other files 
that are also used to migrate the disk layout, namely 
VAR_DIR/layout/config/df.txt and /etc/rear/rescue.conf 
Additionally re-read the BTRFS_SUBVOLUME_SLES_SETUP 
variable from /etc/rear/rescue.conf because its value could 
have been migrated to a new value (issue #2181).

* In the function apply_layout_mappings (therein in its "step 3") 
treat leftover temporary replacement words (like `_REAR1_`) 
as an error only if they are in a non-comment line (issue #2183).

* When 420_autoresize_last_partitions.sh resizes the last partition 
on a GPT disk it leaves 33 LBA blocks at the end of the disk 
so that 420_autoresize_last_partitions.sh produces a fully correct 
disklayout.conf file where no further automated adjustments by 
a subsequent script like 100_include_partition_code.sh are done (issue #2182).

* Error out when DHCLIENT_BIN and DHCLIENT6_BIN 
are empty and USE_DHCLIENT is set (issue #2184).

* In build/default/990_verify_rootfs.sh 
skip the ldd test for kernel modules because in general 
running ldd on kernel modules does not make sense 
and sometimes running ldd on kernel modules causes 
needless errors because sometimes that segfaults (issue #2177).

* Bacula: Fixed 'bconsole' prompt not displaying at all
when doing a recover (issue #2173).

* Fixed that the EFI Boot Manager cannot be installed
when the system has a NVMe SSD, because the device name
is resolved as /dev/nvme0n1p rather than /dev/nvme0n1
so for NVMe devices the trailing 'p' in the Disk value
as in /dev/nvme0n1p that is derived from /dev/nvme0n1p1
needs to be stripped to get /dev/nvme0n1 (issues #2160 #1564).

* Changed double quotes (") to single quotes(') in documentation
related to BORG_PASSPHRASE variable. Double quotes will not avoid
expanding of variable when '$' sign is used inside pass phrase string,
which can lead to unsuccessful opening of Borg archive (issue #2205).

* More fail safe BACKUP_PROG_CRYPT_KEY handling:
Use double quotes `"$BACKUP_PROG_CRYPT_KEY"` so that 
the BACKUP_PROG_CRYPT_KEY value can contain spaces. 
Escape special regexp characters in the BACKUP_PROG_CRYPT_KEY 
value when it is used as a regexp in grep or sed. 
Use single quotes `BACKUP_PROG_CRYPT_KEY='my_passphrase'`
in the documentation examples so that the BACKUP_PROG_CRYPT_KEY 
value can contain special characters (except single quote). 
Recommend in default.conf to not use special characters in the 
BACKUP_PROG_CRYPT_KEY value to be to be on the safe side 
against things breaking in unexpected weird ways when certain code 
in ReaR is not yet safe against arbitrary special characters in values
(issues #2157 #1372).

* Avoid that the BACKUP_PROG_CRYPT_KEY value appears in a log file
in particular when 'rear' is run in debugscript mode where 'set -x' is set
by redirecting STDERR to /dev/null for those confidential commands
that use the BACKUP_PROG_CRYPT_KEY value, cf. the comment of the
UserInput function how to keep things confidential when 'rear'
is run in debugscript mode (issues #2155 #2156).

* Fix when recovery fails on LUKS-encrypted filesystem using simple password
because only the first 2 parameters in /etc/crypttab are mandatory (issue #2151).

* Generic enhancements for IBM Z (s390) to support 'dasd' disks 
and SUSE specific enhancements to install during "rear recover" 
the special GRUB2 plus ZIPL bootloader on SLES12 and later via the 
new finalize/SUSE_LINUX/s390/660_install_grub2_and_zipl.sh script
(issues #2137 #2150).

* Cleanup of the OUTPUT=RAMDISK code.
Overhauled 900_copy_ramdisk.sh as architecture independent 
new script output/RAMDISK/default/900_copy_ramdisk.sh that 
replaces output/RAMDISK/Linux-i386/900_copy_ramdisk.sh 
and overhauled conf/templates/RESULT_usage_RAMDISK.txt 
plus better description of OUTPUT=RAMDISK 
and RESULT_FILES in default.conf (issue #2148).

* Enable creation of non consecutive partitions (issues #2081 #1793 #1771 #1681).

* The new SUSE-specific script finalize/SUSE_LINUX/i386/675_install_shim.sh 
calls 'shim-install' inside chroot on the recreated target system 
and uses boot/grub2/grub.cfg as input to recreate two needed 
(identical) EFI files /boot/grub2/x86_64-efi/core.efi
and /boot/efi/EFI/opensuse/grubx64.efi (issue #2116).

* Fixed that output/default/950_copy_result_files.sh falsely errors out
when an output method OUTPUT=FOO is used where there is no matching
usr/share/rear/conf/templates/RESULT_usage_FOO.txt because when it is missing
it is no sufficient reason to error out.
Furthermore now 950_copy_result_files.sh is skipped  when RESULT_FILES is empty
because that means there is no actual output and then it is not needed
to only copy VERSION README and LOGFILE to the output location (issue #2147).

### Version 2.5 (May 2019)

#### Abstract

New features, bigger enhancements, and possibly backward incompatible changes:

* Enhancements to better support mmcblk/eMMC disks:
An "eMMC" device could be not only one single disk but actually consist
of several 'disk' type block devices for example the actually usable
disk /dev/mmcblk0 (with its partitions like /dev/mmcblk0p1 and /dev/mmcblk0p2)
plus special additional disks on the same eMMC device like /dev/mmcblk0boot0
and /dev/mmcblk0boot1 and /dev/mmcblk0rpmb (issue #2087).

* Now there is in default.conf `MODULES=( 'all_modules' )`
which means that now by default all kernel modules
get included in the recovery system (issue #2041).
Usually this is required when migrating to different hardware. 
Additionaly it makes the recovery system better prepared when this or that
additional kernel module is needed, e.g. to ensure a USB keyboard is usable
in the recovery system (issue #1870) or to ensure data on external
medium (e.g. iso9660) can be read (issue #1202).
Furthermore this is helpful to be on the safe side against possibly missing
dependant kernel modules that are not automatically found (issue #1355).
The drawback of MODULES=( 'all_modules' ) is that it makes the recovery system
(and its ISO image) somewhat bigger (see issue #2041 for some numbers).
With `MODULES=()` the old behaviour can be still specified.
There is a minor backward incompatible change:
Before the user had to specify in etc/rear/local.conf
`MODULES=( "${MODULES[@]}" 'moduleX' 'moduleY' )`
to get some specific modules included in addition to the ones
via an empty MODULES=() but now the user must specify
`MODULES=( 'moduleX' 'moduleY' )`
for that because with "${MODULES[@]}" the new default value 'all_modules'
would be kept which would trigger that all modules get included
so that now `MODULES=( "${MODULES[@]}" 'moduleX' 'moduleY' )`
includes all kernel modules in the recovery system which includes
in particular 'moduleX' and 'moduleY' so that things still work
but with a bigger recovery system.
For details see the MODULES description in default.conf. 

* The new verify script layout/save/default/950_verify_disklayout_file.sh
verifies the disklayout.conf file that is created by "rear mkrescue/mkbackup".
Currently only some very basic verification is implemented: It verifies that
the 'disk' entries look syntactically correct (only basic value type testing),
the 'part' entries look syntactically correct (only basic value type testing),
the 'part' entries specify consecutive partitions.
The latter is needed to make ReaR more fail-safe in case of sparse partition
schemes (i.e. when there are non-consecutive partitions) because currently
"rear recover" fails when there are non-consecutive partitions (issue #1681).
In general verification of the created disklayout.conf should help
to avoid failures when it is too late (i.e. when "rear recover" fails).
It is better to error out early during "rear mkrescue/mkbackup".
It may happen that layout/save/default/950_verify_disklayout_file.sh
falsely lets "rear mkrescue/mkbackup" error out because of false alarm.
The immediate workaround for the user in such cases is to remove that script
or skip what it does by adding a 'return 0' command at its very beginning.

* Basic support for EFISTUB booting: Via the new config variable EFI_STUB
(see default.conf) the user can (and if needed must) specify that
the recreated system should boot via EFISTUB. If EFI_STUB is specified
but some boot loader like GRUB2 or ELILO is used on the original system,
the recreated system gets migrated to boot (only) via EFISTUB.

* The whole 'rear dump' output format need to be changed to improve it
to clearly distinguish array elements.

* Now during "rear mkrescue/mkbackup" md5sums are created for all regular files
in in the recovery system and stored as /md5sums.txt in the recovery system.
During recovery system startup it verifies those md5sums.
Via the new config variable EXCLUDE_MD5SUM_VERIFICATION (see default.conf)
the user can specify what files should be excluded from being verified
to avoid errors on "false positives".

* GRUB2 installation on x86 and ppc64le architecture was completely rewritten
and enhanced by the new config variable GRUB2_INSTALL_DEVICES (see default.conf)
so that now the user can specify what he wants if needed and in MIGRATION_MODE
disk mappings are applied when devices in GRUB2_INSTALL_DEVICES match.

#### Details (mostly in chronological order - newest topmost):

* In packaging/rpm/rear.spec reactivated 'BuildRoot' and 'defattr' because both are required for building 'rear' RPM packages on SLES 11 and RHEL 5 / CentOS 5 via the openSUSE Build Service and removed /etc/cron.d/rear and related things (issues #2135 #1855 #1856 #1908 #1892)

* In layout/prepare/default/420_autoresize_last_partitions.sh continue with the next disk if the current one has no partitions otherwise the "Find the last partition for the current disk" code fails (issue #2134)

* Improved handling of broken symlinks inside the recovery system: Relative symbolic links are now properly handled. Directories, which are link targets, are not copied into the recovery system but a meaningful hint is printed that COPY_AS_IS can be used for that (issues #2129 #2130 #2131)

* In layout/prepare/GNU/Linux/135_include_btrfs_subvolumes_generic_code.sh fixed 'btrfs subvolume set-default' command for older versions of 'btrfsprogs' where that command requires both arguments 'subvolid' and 'path' (issue #2119) 

* For backup NBU: Exclude the whole '/usr/openv/netbackup/logs' directory instead of only the files in that directory (issue #2132)

* Fixed layout mapping error when TCG Opal 2 self-encrypting disks were present but had to be excluded due to a non-existent disk during recovery. This exclusion was not handled properly (issue #2126)

* In build/default/995_md5sums_rootfs.sh also exclude all files with a trailing '~' in their name because those are also excluded when the recovery system initrd is made by pack/GNU/Linux/900_create_initramfs.sh (issue #2127)

* Suppressed unwanted "Welcome to Relax-and-Recover ..." etc/motd messages from 'chroot $ROOTFS_DIR /bin/bash --login ...' calls that appear in the log file or in the stdout of the chroot call when the output is further processes by 'grep' by redirecting stdin of the chroot call to /dev/null because no input is needed (issues #2120 #2125)

* Replace RULE_FILES with a global UDEV_NET_MAC_RULE_FILES in default.conf (issues #2074 #2123)

* Added comment to default.conf that tells when KEEP_BUILD_DIR is automatically set to true (issue #2121)

* Added /usr/openv/netbackup/sec/at/lib/ to NBU_LD_LIBRARY_PATH in default.conf (issues #2105 #2122)

* Simplified awk constructs in 320_include_uefi_env.sh into using plain grep and a bash array to avoid inexplicable wrong behaviour in some cases that is somehow related to the nullglob bash option together with different kind of awk (issues #2095 #2115)

* For older systems (e.g. like SLES11) where /dev is no mountpoint in the recovery system we first mount TARGET_FS_ROOT/dev as 'tmpfs' and then we copy all /dev contents from the recovery system into TARGET_FS_ROOT/dev to make all recovery system /dev contents available at TARGET_FS_ROOT/dev (which are needed therein for things like "chroot TARGET_FS_ROOT mkinitrd") but only as long as the recovery system runs. On the rebooted target system its pristine /dev will be there. This is basically what finalize/default/100_populate_dev.sh had done but now without dirty remainders on the user's target system disk (issue #2113).

* Do not copy symlink targets in /proc/ /sys/ /dev/ or /run/ into the ReaR recovery system. For example on SLES11 /lib/udev/devices/core is a symlink to /proc/kcore so that "rear mkrescue" basically hangs up while copying /proc/kcore because it is huge (issue #2112)

* Avoid needless things when there is more than one disk: Avoid tot go into MIGRATION_MODE in any case when there is more than one disk. Avoid that GRUB2 gets needlessly installed two times on the same device (issue #2108)

* Fixed disk device name in efibootmgr call for eMMC devices: For eMMC devices the trailing 'p' in the disk device name (as in /dev/mmcblk0p that is derived from /dev/mmcblk0p1) needs to be stripped (to get /dev/mmcblk0), otherwise the efibootmgr call fails because of a wrong disk device name (issue #2103)

* For Ubuntu 18.x use /run/systemd/resolve/resolv.conf as /etc/resolv.conf in the recovery system: Basically the /etc/resolv.conf symlink target and /lib/systemd/resolv.conf contain only the systemd-resolved stub resolver "nameserver 127.0.0.53" and only /run/systemd/resolve/resolv.conf contains a real nameserver (issues #2018 #2101)

* When mktemp needs to be called with a TEMPLATE call it with sufficent XXXXXXXXXX in the TEMPLATE, otherwise use the mktemp default (issue #2092)

* LPAR/PPC64 bootlist was incorrectly set when having multiple 'prep' partitions: Use the specific right syntax for array expansion of the boot_list array (issues #2096 #2097 #2098 #1068)

* Ensure that the Error function results a direct and complete exit of the whole running 'rear' program even if the Error function was called from a (possibly nested) subshell in a sourced script: Now the Error function terminates all descendant processes of MASTER_PID except MASTER_PID and the current (subshell) process that runs the Error function and when Error was called from a subshell it finally exits its own subshell so that when the Error function finished only MASTER_PID is still running and finally MASTER_PID exits as usual via the DoExitTasks function (issues #2088 #2089 #2099)

* Ignore special additional disks on eMMC devices named "rpmb" and "boot": Now the extract_partitions() function skips device nodes on eMMC devices like /dev/mmcblk0rpmb or /dev/mmcblk0boot0 and /dev/mmcblk0boot1 because ReaR wrongly recognized those 'disk' type block devices as if they were 'part' type block devices, i.e. those are no partitions, but special additional disks on the eMMC device (issue #2087)

* Updated the OPALPBA workflow: Set USE_RESOLV_CONF='no' as networking is not required/available in the PBA. Avoid copying in the entire /etc/alternatives directory as its links could pull in lots of unwanted stuff, which is not required in rescue systems. Clean up plymouth/unlock service startup (issue #2083)

* Network: Record permanent mac address when device is enslaved in a Team, or else /etc/mac-addresses will record broken information. Use "ethtool -P" as the preferred method to retrieve the MAC address. Otherwise fall back to other methods, which may lead to some invalid MAC address when using Teams (issues #1954 #2065)

* Added 'net-tools' to Debian dependencies as required for 'route' command which belongs to the REQUIRED_PROGS (issue #2082)

* Added an additional separated new btrfs_subvolumes_setup_generic() function to recreate all mounted Btrfs subvolumes in a more generic way. The old btrfs_subvolumes_setup function was renamed into btrfs_subvolumes_setup_SLES and this one is called as fallback to be backward compatible. For both btrfs_subvolumes_setup implementations it is individually configurable which one is used for which btrfs device via the new config variables BTRFS_SUBVOLUME_SLES_SETUP and BTRFS_SUBVOLUME_GENERIC_SETUP. Currently it is not documented because it is work in progress where arbitrary further changes will happen (e.g. the current btrfs_subvolumes_setup_generic function makes diskrestore.sh fail on older systems where 'btrfs subvolume set-default' needs two arguments) so one has to inspect the current code and comments in the layout/prepare/GNU/Linux/13X_include_... scripts to see how things currently work (issues #2067 #2079 #2080 #2084 #2085)

* Fixed SSH root login on the recovery system with some configurations: On Ubuntu 18.04 with OpenSSH 7.6, /etc/ssh/sshd_config contains commented-out lines for 'PermitRootLogin' and other options. This fix makes sure that settings changed for sshd in the ReaR recovery system will be real, not comments (issue #2070)

* RAWDISK output: Improved device partition detection (e.g. Ubuntu 18.04). On Ubuntu 18.04, it has been observed that after creating a loop device and creating a properly sized VFAT file system >250 MB on it, after mounting the file system size was actually just 30 MB. Reason: The partition detection did not pick up the correct partition sizes of the associated image file. This change uses losetup's --partscan option (supported by util-linux v2.21 and above) to offer one additional opportunity to detect partitions. If the option is not available, a traditional losetup call will be used as a fallback (issue #2071)

* OPALPBA output fix: Do not include any PBA into another PBA. A TCG Opal pre-boot authentication (PBA) system is a minimal operating system constructed by ReaR to unlock self-encrpyting disks before the regular OS takes over. Before this PR, it could happen that a previously created PBA was included in a subsequently created PBA, causing it to be unnecessarily large (issue ##2072)

* Now there is in default.conf MODULES=( 'all_modules' ) which means that now by default all kernel modules get included in the recovery system (issues #2041 #1870 #1202 #1355)

* New verify script layout/save/default/950_verify_disklayout_file.sh to verify disklayout.conf that was created by "rear mkrescue/mkbackup" (issues #2060 #1681)

* Refresh udev with trigger before activating multipath (issue #2064): Ensure that all information from multipath devices is updated by udev into /sys before activating multipath. This helps to mitigate certain kind of issues when something wrong in the SAN zoning configuration (issues #2002 #2016 #2019)

* For backup NSR: It is of additional use to not skip the retrieval of the filesystems even in NSR_CLIENT_MODE so that this is now also done in NSR_CLIENT_MODE: Due to saving the save sets filesystem information in $VAR_DIR/recovery/nsr_paths within the recovery image one is able to retrieve/read this information during a recovery process i.e. for advising the EMC networker server team to recover the appropriate filesystem(-structure) from the backups beeing made (issue #2058)

* Skip patching absolute symlinks during finalize stage (issue #2055). That does not actually fix issue #1338 but for now it should at least avoid patching wrong files. Furthermore do no longer create udev rules in the recreated system that have not been there. This way one can avoid that ReaR creates udev rules that are created and maintained by systemd/udev like /etc/udev/rules.d/70-persistent-net.rules when one excludes such udev rules from being restored from the backup or by moving them away via BACKUP_RESTORE_MOVE_AWAY_FILES (issue #770)

* Now /proc /sys /dev and /run are bind-mounted into TARGET_FS_ROOT at the beginning of the finalize stage via the new 110_bind_mount_proc_sys_dev_run.sh script and existing code in various finalize scripts for mounting /proc /sys /dev and things like that was removed and the finalize scripts were adapted and renumbered as needed (issues #2045 #2035)

* Added eno-fix.rules to RULE_FILES for LAN interface MAC address changes to the ens-style LAN interface names instead of the older eno-type LAN names and aligned RULE_FILES content in the involved scripts (issue #2046)

* Fixed 58-start-dhclient.sh script to make it work reliably with multiple network interfaces (issue #2038)

* Borg backup restore enhancements and fixes: Added checks whether we can read Borg archive, user can enable progress display via BORGBACKUP_SHOW_PROGRESS="yes", corrected mounting of USB device when using Borg (issues #2029 #2037)

* Fix for GRUB2 EFI modules search directory location: Instead of looking for GRUB2 modules only in /boot also find them in /usr/lib/grub*, where GRUB2 modules are normally installed by default (issue #2039)

* Basic support for EFISTUB booting plus documentation (issues #1942 #2030)

* Multipath optimizations: Optimized get_device_name() by calling "dmsetup info" only once, and for "dm" devices only. Removed collecting output of /sys/class/fc_transport since it can be very slow and is not used (issues #2020 #2034)

* Suppress dispensable 'set -x' debug output unless called with '--debugscripts x': A noticeable part (25% and more) of the 'set -x' debugscripts output is usually of no interest and therefore such output is suppressed by default (e.g. when rear is called with '-D') unless rear is called with '--debugscripts x' where the full debugscripts output is still there as it was before (issue #2024)

* Cleaned up the Docker specific exclude part in 230_filesystem_layout.sh: Determine docker_root_dir only once and try to be safer against possibly crippled Docker installations (e.g. timeout 'docker info') and be safe against empty docker_root_dir (otherwise all mountpoints would match the empty string and we would would skip all mountpoints) and show possible errors to the user in any case (issues #1989 #2021)

* Improved setup of /etc/resolv.conf in the recovery system: In case of static networking setup in the recovery system a plain traditional /etc/resolv.conf file with an entry of a remote 'nameserver DNS.server.IP.address' is needed. It cannot work when /etc/resolv.conf contains only loopback IP addresses (which happens when the stub resolver systemd-resolved is used) or when there is no nameserver entry so that "rear mkrescue/mkbackup" errors out in this case. In contrast when USE_DHCLIENT is ture (e.g. when DHCP is used on the original system) then during recovery system startup /etc/resolv.conf will be generated by /bin/dhclient-script so that it does not matter what its content was before. For special cases the user can specify what he wants via the new USE_RESOLV_CONF variable (issues #2015 #2018 #2076)

* Improved 'rear dump' output to clearly distinguish array elements. The whole 'rear dump' output format was changed. Now it shows normal string variables as `STRING="string of words"` and arrays as a beautified/simplified 'declare -p' output as `ARRAY=("first element" "second element" ... )` unless in debug mode where the plain 'declare -p' output is shown like `declare -- STRING="string of words"` and `declare -a ARRAY=([0]="first element" [1]="second element" ... )` that is needed for complicated array elements that contain brackets. Furthermore now the 'rear dump' output can be directly sourced (issue #2014)

* Error out when we cannot make a bootable EFI image of GRUB2 which is required when UEFI is used (issue 2013)

* Allow non-interactive authentication with rsync by using BACKUP_RSYNC_OPTIONS to specify the "--password-file=/full/path/to/file" rsync option (issue #2011)

* Add possibility for user to specify whether or not show Borg stats at the end of backup session (issue #2008)

* Adaption for Fedora 29: grub2-efi-x64-modules does not contain linuxefi module (issue #2001)

* Let the user optionally specify mkfs.xfs options if needed to recreate XFS filesystems with different options than before (e.g. in MIGRATION_MODE because of different replacement hardware) via the new MKFS_XFS_OPTIONS config variable (issue #2005)

* Network interface renaming: Automatically map device to its new name when device is found on the system with same MAC address but different name (issue #2004)

* It is no BugError when neither getty nor agetty are avaiable. Such a case it is an Error because the user must have the programs in REQUIRED_PROGS installed on his system

* Fix for 'error:unrecognized number' when booting ISO on PPC: Issue is caused by incorrect, according to PAPR specification, bootinfo entity parsing code in GRUB2 (issue #1978)

* Fixed that in recovery system DHCP client did not iterate through all network interfaces. It incorrectly took only the first one. If the interface that is intended for recovery was not the first one, it had failed (issue #1986)

* Automatically exclude BUILD_DIR from the backup. When TMPDIR was specified to something not in /tmp, BUILD_DIR was not automatically excluded from the backup (issue #1993)

* Support ISOs bigger than 4GiB with OUTPUT=ISO by including the udf kernel module into the recovery system when mkisofs or genisoimage is used (not needed for xorrisoifs) and enable creating an hybrid iso9660/udf DVD (issue #1836)

* Cleaned up how KERNEL_FILE is set: Now the KERNEL_FILE variable is set in the 'prep' stage only by the new prep/GNU/Linux/400_guess_kernel.sh that replaces the old pack/GNU/Linux/400_guess_kernel.sh  and all the various different 300_copy_kernel.sh scripts (except output/USB/Linux-i386/830_copy_kernel_initrd.sh) are removed because the 300_copy_kernel.sh scripts had also only set the KERNEL_FILE variable in various ways. Additionally it errors out in various output stage scripts when kernel or initrd are missing and it shows aligned error messages in those cases to the user. Finally it errors out when the file does not exist or is a broken symlink. (issues #1851 #1983 #1985)

* Enhanced and more robust exclude of vfat filesystem (ESP) from SELinux relabeling during recover process (issue #1977)

* Fixed NBU (NetBackup) not working since ReaR 2.4: With NetBackup, too many binaries were included, causing the recovery system verification to fail or print error messages. A new NBU_LD_LIBRARY_PATH variable is used during verification. The RequiredSharedOjects() function was fixed to not list the left part of the ldd mapping when there is a right part: Some NetBackup libraries have a mapping such as "/lib/ld64.so => /lib64/ld-linux-x86-64.so.2", causing the RequiredSharedOjects() function to print "/lib/ld64.so" which does not resolve (issue #1974)

* Add /etc/ca-certificates directory to recovery system (issue #1971)

* Skip 630_run_efibootmgr.sh when UEFI_BOOTLOADER is empty or not a regular file and determine the ESP mountpoint from UEFI_BOOTLOADER and use $TARGET_FS_ROOT/boot/efi as fallback plus corrected logic whether or not NOBOOTLOADER is set empty (issues #1942 #1945)

* Missing libraries in recovery system caused executables to fail because libraries were skipped from copying when their library path was a substring of another already copied library, for example /path/to/lib was skipped when /other/path/to/lib was already copied (issue #1975)

* Check for carriage return in local/site/rescue.conf files (issue #1965)

* Specific required programs are added to REQUIRED_PROGS depending on what component keywords are used in disklayout.conf (issue #1963)

* Cleanup and enhancement of REQUIRED_PROGS and PROGS checks (issue #1962)

* Moved PROGS from GNU/Linux.conf to default.conf REQUIRED_PROGS (issue #892)

* Record permanent mac address when device is enslaved in a bond, or else /etc/mac-addresses will record broken information (issue #1954)

* For Slackware Linux: Added automatic OS vendor and version detection. Added logic to search for kernel files when the standard ELILO kernel install paths come up empty. Added legacy LILO bootloader support.

* Enhanced and cleaned up 985_fix_broken_links.sh to find symlinks in the recovery system where the link target is outside of the recovery system and in such cases the missing link target gets copied into the recovery system plus more verbose error reporting to the user (issue #1638)

* Fixed that the FindStorageDrivers function failed on kernels with no modules.
When MODULES=( 'no_modules' ) is set FindStorageDrivers() is now skipped (issue #1941)

* Increased the default USB_UEFI_PART_SIZE from 200 to 400 MiB (issue #1205)

* Enhanced rescue/GNU/Linux/310_network_devices.sh for Bonding and Teaming
when it is configured with LACP. Bonding should not be simplified in this case
plus added code for non-simplified Teaming (issue #1926).
Furthermore simplified/hardened the tests/setup1 and tests/setup2 testbeds.

* In 500_clone_keyboard_mappings.sh avoid running find
in the whole tree of filesystems which takes ages (issue #1906)

* Let 100_include_partition_code.sh error out
when the parted command is not there (issue #1933)

* Suppress bash stderr messages like 'unary operator expected'
or 'integer expression expected' where a particular 'test'
intentionally also fails this way (issue #1931)

* Show errors to the user via LogPrintError instead of only LogPrint
because LogPrint outputs only in case of verbose mode (issue #1914)

* Avoid dangling symlinks when copying ReaR config files into the ReaR recovery system
by copying the content of the symlink target via 'cp -L' (issue #1923)

* Avoid 'nullglob' pitfalls in 250_populate_efibootimg.sh
and generally overhalued that script (issue #1921)

* Check that CONFIG_DIR is a directory (issue #1815)

* Avoid bash error messages like: "eq: unary operator expected"
when using older versions of xfsprogs (issue #1915)

* Added exclusion of zram devices from device mapping in default.conf.
By default zram devices are not to be mapped, exactly as it is done
for ramdisk and loop devices (issue #1916)

* Fixed a non-working awk command in the generate_layout_dependencies() function
that falsely also matched commented '#btrfsmountedsubvol' entries in disklayout.conf
with an egrep command that is more in line with how it had worked before (issue #1497)

* Fixed and enhanced NETFS+tar backup pipe exit code handling (issue #1913)

* Moved the functionality of the recovery system setup script 67-check-by-label-cdrom.sh
into the mount_url function 'iso' case plus additional enhancements there
with a user dialog if things are not o.k. and removed the no longer
needed 67-check-by-label-cdrom.sh (issues #1893 #1891 #326)

* Fixed that on LPAR/PPC64 PowerVM the boot devices order list was incorrectly
set (via 'bootlist') when having multiple 'prep' partitions. Now handling of
multiple 'prep' partitions was added plus enhanced handling of multiple prep
partitions and multipath (issue #1886)

* Fixed and enhanced the get_disk_size and get_block_size functions 
so that now by default blockdev is used (if exists) to retrieve the size
of the disk and its block size and compute partition start using 512 bytes
blocks (this is hardcoded in the Linux kernel) to fix wrong partition
information when a disk has 4K block size (issue #1884)

* Print multipath device name during "rear recover" when "firendly_name" option is off (issue #1889)

* Now the Error function shows some last messages of the last sourced script to the user (issues #1877 #1875)

* Duplicity: Misc improvements (issues #1876 #1879 #1882)

* Added code to recognize persistent LAN interface and manipulate KERNEL_CMDLINE (issue #1874)

* Initial tentative support for OBDR on ppc64le (issue #1868)

* Wait for systemd-udevd to avoid broken pipe error in 40-start-udev-or-load-modules.sh (issue #1832)

* Aviod duplicate UUID in boot menuentry when snapper is used (issue #1871)

* Added choice to confirm identical layout mapping only once plus disabling MIGRATION_MODE (issue #1857)

* Verify md5sums of files in recovery system (issues #1859 #1895)

* Fedora28: syslinux needs libcom32.c32 to boot from HD and
missing ldlinux.c32 and libutil.c32 prevents PXE booting (issues #1861, #1866)

* Add support for Slackware UEFI/USB (issues #1853, #1863)

* RAWDISK output portability improvements (issue #1846)

* Fixed, simplified, and enhanced GRUB2 installation on x86 and ppc64le architecture (issues #1828, #1845, #1847, #1437)

### Version 2.4 (June 2018)

#### Abstract

New features, bigger enhancements, and possibly backward incompatible changes:

* Major rework and changed default behaviour how ReaR behaves
in migration mode when partitions can or must be resized
to fit on replacement disks with different size.
The new default behaviour is that only the partition end value 
of the last partition on a disk (and therefore its partition size) 
may get changed if really needed but no partition start value 
gets changed to avoid changes of the partition alignment. 
The new 420_autoresize_last_partitions script implements 
the new behaviour and the old 400_autoresize_disks was 
renamed into 430_autoresize_all_partitions to still provide 
the old behaviour if that is explicitly requested by the user 
but the old behaviour may result unexpected changes 
of arbitrary partitions on a disk. 
The new config variables AUTORESIZE_PARTITIONS
AUTORESIZE_EXCLUDE_PARTITIONS
AUTOSHRINK_DISK_SIZE_LIMIT_PERCENTAGE
AUTOINCREASE_DISK_SIZE_THRESHOLD_PERCENTAGE
determine how ReaR behaves in migration mode
when partitions can or must be resized.
With AUTORESIZE_PARTITIONS='yes' the old behaviour is done. 
With AUTORESIZE_PARTITIONS='no' no partition is resized by ReaR. 
With the default AUTORESIZE_PARTITIONS='' at most the last active 
partition on each active disk gets resized but only if really needed 
which also depends on the settings of the other config variables above. 
For details see default.conf and the two 'autoresize' scripts.
For some examples see https://github.com/rear/rear/pull/1733 

* Network setup was completely reworked to support bonding, bridges, vlans and teaming.
There is a full rewrite of the 310_network_devices.sh script generating network interfaces
for use during ReaR rescue/recovery system networking setup via the 60-network-devices.sh script.
It also handles corner cases/odd setups that can be found from time to time,
typically when the administrator uses bonding plus bridges plus vlans as well as teaming. 

* Initial (limited) support for certain ARM based hardware.
It should work with Raspberry Pis, most TI and Allwinner devices.
There are two ARM specific BOOTLOADER variable values where
'ARM-ALLWINNER' is for Allwinner devices that will backup and restore the 2nd stage bootloader
versus plain 'ARM' which is a dummy that does nothing so that on Raspberry Pi and most TI devices
you need to include the first FAT partition (with the MLO or bootcode.bin) in your backup.

* Simplified and enhanced TSM restore plus first draft of TSM backup.

* EMC Avamar support has been added.

* Duplicity backup has been seriously enhanced.

* Support for TCG Opal 2-compliant self-encrypting disks and RAWDISK output.

* YUM+backup adds the ability to backup and restore files to the YUM method.

#### Details (mostly in chronological order):

* Copy backup restore log into recreated system (issue #1803)

* Sesam integration: add sesam bin directory to LD_LIBRARY_PATH (issue #1817)

* ReaR recovery fails when the OS contains a Thin Pool/Volume (issues #1380, #1806)

* Make SLES12-GA/SP0 btrfs recovery work again (issues #1796, #1813)

* Verify if dm-X is a partition before adding to sysfs_paths (issue #1805)

* Do not start multipathd when not needed (issue #1804)

* Better way to get multiapth partion name (issue #1802)

* Exclude multipath device that does not have mounted fs (issue #1801)

* Do not print each files restores by TSM in main output (issue #1797)

* man page BACKUP SOFTWARE INTEGRATION update (issues #1788, #1791)

* In the DRLM specific function drlm_import_runtime_config() solve problem with some variables loading config from DRLM (issue #1794)

* Better describe NON_FATAL_BINARIES_WITH_MISSING_LIBRARY in default.conf (issues #1792, #1693)

* Introduced KEYMAPS_DEFAULT_DIRECTORY and KEYMAP variables, see the default.conf file (issues #1781, #1787)

* Fix multipath partition replacement and multipath partition naming (issue #1765)

* Skip LUKS encrypted disks when guessing bootloader (issue #1779)

* First draft of TSM backup (issue #1348)

* HP RAID code was updated as the new executable is now called as "ssacli" (issue #1760)

* Exclude docker file systems from layout (issue #1749)

* Added migation mode confirmation at beginning of finalize stage (issue #1758)

* Show descendant processes PIDs with their commands in the log via pstree or ps as fallback (issues #1755, #1756)

* Check for 'Hah!IdontNeedEFI' GUID number for a GPT BIOS boot partition (issues #1752, #1754, #1780)

* Fixed invalid reported return code (always 0) upon NBU restoration failure (issue #1751)

* Run exit tasks code with default bash flags and options (issues #700, #1747, #1748)

* Major rework and changed default behaviour regarding AUTORESIZE_PARTITIONS (issues #102, #1731, #1733, #1746)

* YUM+backup adds the ability to backup and restore files to the YUM method (issues #1464, #1740)

* Add dbus user and group by default (issues #1710, #1743)

* Add gsk libs to TSM_LD_LIBRARY_PATH (issue #1744)

* Use 'grub2-install --no-nvram' on PowerNV system (issue #1742)

* We can use chronyd as time syncing mechanism now as well (issue #1739) 

* Trace and fix broken symbolic links in rootfs (issues #1638, #1734)

* Borg Backup can now use USB disk as well as backup storage area (issue #1730)

* Initial support for ARM (issue #1662)

* Improve the network parameters on the Linux Kernel command line (issue #1725)

* Clean termination of descendant processes (issues #1712, #1720)

* Simpler and more fail-safe SLE btrfs-example.conf files (issues #1714, #1716)

* Use a fallback to get interface state using the 'carrier' status (issues #1701, #1719)

* Fix duplicity backup (issue #1695)

* Include Bareos plugin directory to make bareos-fd start reliably (issues #1692, #1708)

* Again support GPT partition names with blanks (issues #212, #1563, #1706)

* Improvements around Borg Backup (issues #1698, #1700)

* Automatically add 'missing' devices to MD arrays with not enough physical devices upon restore (issue #1697)

* Network setup was completely reworked to support bonding, bridges, vlans and teaming (issue #1574)

* Fixed restore backup when BACKUP_INTEGRITY_CHECK=1 (issue #1685)

* Support TCG Opal 2-compliant self-encrypting disks and RAWDISK output (issue #1659)

* Add EMC Avamar backup (issues #1621, #1677, #1687)

* Avoid falsely detected changed layout for 'rear checklayout' (issues #1657, #1658, #1673)

* Simplified TSM dsmc restore and improved TSM connection test (issues #1534, #1643, #1645)

* Duplicity with duply waits forever (issues #1664, #1672)

* Duplicity: Add Support for NETFS URLs (issues #1554, #1665, #1668, #1669)

Many minor fixes (too many to list them all - use 'git log' to view them).
A big thank you to all contributors as without you it would be impossible to keep up with the development
in the Linux area. We love you all... :-)

### Version 2.3 (December 2017)

#### Abstract

New features and bigger enhancements:

* First steps towards running Relax-and-Recover unattended in general.
Several user dialogs that had been implemented in ReaR via the bash builtin 'read'
or the bash 'select' keyword are now implemented via the new UserInput function.
The UserInput function proceeds with a default input value after a timeout
so that it is now possible to let ReaR run unattended with its default behaviour.
Additionally one can predefine an automated input value for each particular
UserInput function call so that it is now also possible for the user
to predefine what ReaR should do when running unattended.
For details see the USER_INPUT_... config variables in default.conf.
Currently not all user dialogs use the UserInput function so that
this or that user dialog needs to be adapted when it is reported to us
via our [issue tracker](https://github.com/rear/rear/issues).
In contrast when programs that are called by ReaR work interactively
(e.g. third-party backup tools that show user dialogs or password prompts)
the program call itself must be adapted to run unattended (if possible),
see the section 'It should be possible to run ReaR unattended'
in our https://github.com/rear/rear/wiki/Coding-Style Wiki article.

* SSH support in the ReaR rescue/recovery system was overhauled.
By default it is now secure which means the recovery system is free of SSH secrets.
Individual settings can be specified via the SSH_FILES, SSH_UNPROTECTED_PRIVATE_KEYS,
and SSH_ROOT_PASSWORD config variables (for details see default.conf).

* Improved verification of the ReaR rescue/recovery system contents.
Now during 'rear mkrescue/mkbackup' there is a verification step where 'ldd' tests
for each program/binary and library in the recovery system whether or not
its required binaries/libraries can be found in the recovery system.

* Improved autodetection during 'rear recover' when disks on the replacement hardware
seem to not match compared to what there was on the original system so that
ReaR is now more fail-safe against recreating on a possibly wrong disk.

Possibly backward incompatible changes:

* In addition to STDERR now also STDOUT is redirected into the ReaR log file.
Accordingly all output of programs that are called by ReaR is now in the log file
so that the log file content is more complete and there is no longer unintended
verbose information from programs on the terminal where ReaR was lauched.
On the other hand this means when programs prompt via STDOUT to get some user input
(e.g. a program prompts for a user confirmation under this or that circumstances)
the program's STDOUT prompt is no longer visible to the user when the program was
not called properly in the particular ReaR script as described
in the section 'What to do with stdin, stdout, and stderr'
in our https://github.com/rear/rear/wiki/Coding-Style Wiki article.
We tried to fix as many program calls as possible but it is impossible
(with reasonable effort / with a reasonable amount of time)
to check all program calls in all ReaR scripts so that this or that
unnoticed program call will need to be fixed when it is reported to us
via our [issue tracker](https://github.com/rear/rear/issues).

* SSH support in the ReaR rescue/recovery system is now secure by default.
There are no longer private SSH keys in the recovery system by default and
a RSA key is generated from scratch when starting sshd during recovery system startup.
Accordingly it does no longer work by default to use SSH in the recovery system
via the SSH keys that exist on the original system. To get SSH keys included
in the recovery system use the SSH_FILES and SSH_UNPROTECTED_PRIVATE_KEYS
config variables (for details see default.conf).

* Verification of required binaries/libraries in the ReaR rescue/recovery system.
By default it is now fatal when 'ldd' reports a 'not found' library for
any file in a /bin/ or /sbin/ directory in the recovery system so that
now 'rear mkrescue/mkbackup' may fail where it had (blindly) worked before.
In particular third-party backup tools sometimes use their libraries
via unexpected ways which can cause 'false alarm' by the 'ldd' test.
With the new config variable NON_FATAL_BINARIES_WITH_MISSING_LIBRARY
one can specify for which files a 'not found' library should be
considered as 'false alarm' (for details see default.conf).

* Improved MIGRATION_MODE autodetection when the disk layout looks ambiguous.
Now 'rear recover' switches by default more often into MIGRATION_MODE
where manual disk layout configuration happens via several user dialogs
so that by default 'rear recover' shows more often user dialogs compared to before
but the intended behaviour can be enforced via the MIGRATION_MODE config variable
(for details see default.conf).

#### Details (mostly in chronological order):

* Use /etc/os-release and /etc/system-release before falling back to lsb_release check in function SetOSVendorAndVersion (issues #1611, #731)

* Make BACKUP_URL=iso for mkrescue and mkbackuponly no longer fatal (issue #1613)

* Add ntpdate support (issue #1608)

* Fix for XFS file system recreation code. In xfsprogs >= 4.7 log section sunit=0 is considered invalid (issue #1603)

* Changed the macro fedora_release into fedora in the rear.spec file (issue #1192 and bz1419512)

* Borg backup as back end now displays progress, when ReaR is launched in verbose mode (issue #1594)

* Better MIGRATION_MODE autodetection (pull request #1593 related to issue #1271)

* With the new config variable NON_FATAL_BINARIES_WITH_MISSING_LIBRARY the user can specify
what programs where the 'ldd' test reports 'not found' libraries are non-fatal
so that those programs in the recovery system do not lead to an Error abort of "rear mkrescue/mkbackup".
This is a generic method so that the user can avoid issues in particular with third-party backup tools that soemtimes
have unexpected ways to use their specific libraries like https://github.com/rear/rear/issues/1533 (for TSM) and
https://github.com/rear/rear/pull/1560 (for FDR/Upstream).

* Add a NSR_CLIENT_MODE to the backup method NSR (issue #1584)

* Let /bin/ldd detect *.so with relative paths (issue #1560)

* Add support for Bridge Interfaces(issue #1570). Usually, virtual interfaces are skipped,
but for Bridges to work, we consider Bridges as physical interfaces,
because the Bridge interface holds the IP address, not the physical interface attached to the Bridge.
This patch enables those configurations:
  - Bridge over simple Ethernet
  - Bridge over Bond
  - Bridge over Vlan interface

* Use UserInput in some more usual places to improve that 'rear recover' can run
unattended in migration mode (issues #1573, #1399)

* Error out for OUTPUT_URL=null together with OUTPUT=USB (issue #1571)

* Added/updated paths for FDR/Upstream 4.0 because FDR/Upstream 4.0 includes changes to some file paths (issue #1559)

* Fix copying kernel modules when module aliases are present (issue #1567)

* Netbackup agents not automatically started on RHEL 7 (issue #1523)

* Forbid mkrescue and mkbackuponly for iso backup scheme (issues #1547, #1548)

* Only support OpenSSH 3.1 and later for SSH setup (issue #1530)

* Implemented USB_DEVICE_FILESYSTEM_LABEL (issue #1535)

* Split network-functions.sh into DHCP setup and general ReaR functions (issue #1517)

* Avoid leaking unprotected SSH private key files onto rescue medium (issues #1512, #1513)

* Improve cryptographic security and user-friendliness for LUKS volumes (issue #1493)

* Improve ReaR network migration (issues #1605, #1510, #1399)

* Improved encrypted password detection in 500_ssh.sh (issue #1503)

* Several code improvements in the way libraries are detected and copied (issues #1521, #1502, #1494)

* Allow btrfsmountedsubvol to be excluded via EXCLUDE_RECREATE (issue #1497)

* Avoid recreation of non-existing btrfs subvolumes (issue #1496)

* Add automatically some important kernel parameters to KERNEL_CMDLINE (issue #1495)

* Cleanup and simplified default input for the UserInput function (issue #1498)

* Avoid systemd log messages about multiple disk partitions with identical name that happened because ReaR used
a static 'rear-noname' for originally unnamed partitions. Original gpt disk partitions may be unnamed but parted
requires a name for each gpt partition. Now ReaR uses the basename of the partition device path
(e.g. sda1, sda2) for originally unnamed partitions (issue #1483)

* Enhanced how the ssh user is copied into the recovery system and improved detection of the ssh user (issue #1489)

* Use meaningful variable for automated UserInput. Enforce calling UserInput with a UserInput ID so that
automated UserInput is always possible for the user. Use and enforce uppercase letters in UserInput IDs
because the resulting variable names are meant as user config variables (issue #1473)

* Fix UEFI tools integration (issues #1477, #1478)

* Added a new YUM backup method which will recreate the system by installing it from scratch via installing RPM packages.
The YUM backup method uses the yum package manager in the same manner as the ZYPPER backup method (issue #1464)

* BACKUP_PROG_OPTIONS used to be a string variable, turn it into an array (issue #1475)

* Now CLONE_ALL_USERS_GROUPS always extends the CLONE_USERS and CLONE_GROUPS arrays (issues #1471, #1464)

* PXE code improvements (issue #1466)

* Several improvements in the multipath code to allow full migration (issues #1449, #1450)

* Several improvements on the TSM code (issues #1539, #1461, #1452)

* Activate btrfs filesystem creation with uuid. Recent btrfs version finally added this option (issue #1463)

* By default a directories_permissions_owner_group file is created that saves permissions, owner, and group of basic directories
plus symbolic link names and link targets of basic directories. Those basic directories are the currently used mountpoints
(except some unwanted "noise" from all what there is mounted) plus the directories of the Filesystem Hierarchy Standard (FHS)
that actually exist on the system. Additionally with the new DIRECTORIES_TO_CREATE array the user can now explicitly
specify directories and symlinks that are still missing in his particular environment (issue #1459)

* Fix for cryptsetup hang on `cryptsetup luksOpen ...` when dmsetup is not present in recovery system (issue #1458)

* Enhance MOUNTPOINTS_TO_RESTORE into DIRECTORIES_TO_CREATE (issue #1455)

* Several improvements for ppc64le/ppc64 arch. Systemd automatic serial console detection, lilo and yaboot improvements (issue #1446, #1442)

* Introduction of UserInput in 300_map_disks.sh (issues 1399 and 1431)

* Adapt /etc/motd when 'rear recover' is running to avoid the additional 'Run rear recover to restore your system' message
that only makes sense as long as 'rear recover' was not ever started (issue 1433)

* SLES12 with btrfs but without snapshots failed to recreate/mount btrfs FS/subvolumes during recovery (issue #1036)

* Adapt chrp-boot option when xorrisofs is used. Xorrisofs use -chrp-boot-part option to generate PPC boot while mkisofs use -chrp (issue #1430)


### Version 2.2 (July 2017)

* Let the get_disk_size() function retry several times to be more fail-safe when udev needs some time until device files appear. This introduces the new generic helper function retry_command() plus the new config variables REAR_SLEEP_DELAY and REAR_MAX_RETRIES. For details see default.conf and lib/layout-functions.sh (issue #1370)

* ReaR failed to continue due incorrect check of presence of USB device (REAR-000) in /proc/mounts, despite foregoing script (060_mount_NETFS_path.sh) did mounting of this device (issue #1415)

* Add missing privilege separation dir to start sshd on Debian 9 (issue #1381)

* Redirect rsync verbose output to backup log (issue #1387)

* Load storage controllers in rescue system in same order as on host system (issue #1384):
  - Copy over modules from initrd on SUSE LINUX
  - Prevent sorting of MODULES_LOAD array to keep the order intended

* Add /yaboot to ISO_FILE when running SUSE ppc64 (issue #1414)

* Add SSL cert directories so we can interact with Google Cloud Storage (issue #1402)

* Modified the "unattended" into "automatic" with ISO_DEFAULT required for automated recovery tests (issue #1397)

* Use the original fds when ReaR was launched (which are now saved as fd6, fd7, and fd8 for stdin, stdout, and stderr respectively) for actually intended user input and user output. To keep backward compatible behaviour all old deprecated usage of '>&8' is converted into '>/dev/null' but /dev/null usage in general should be cleaned up later (issues #887, #1395)

* Added new generic UserInput and UserOutput plus LogUserOutput functions that are intended to replace current user input functionality that calls select or read directly. For the next ReaR version 2.3 it is planned to also redirect stdout into the log file in addition to stderr (issues #885, #1366, #1398, #1399)

* The 'make rpm' now relies on 'make srpm' which creates the src.rpm package first. This src.rpm package can then be easily copied to another computer to rebuild a rpm package from it without needed the sources itself (or git checkout) (issue #1389)

* Replaced some perl regexp with grep native extended regexp (issue #1376)

* Introducing SECURE_BOOT_BOOTLOADER variable in default.conf. This variable should enable users booting with Secure Boot, to use whatever custom signed boot loader they like, and removes hard coded entry 'shim.efi' from the ReaR code (issue #1374)

* Enhanced and cleaned up making ISO on POWER (ppc64/ppc64le). Now the backup can be stored in the ISO (via BACKUP_URL=iso...) and even multiple ISOs work on POWER now (issues #697, #1383)

* Create multipath.conf only during migration (from non-multipath to multipath), and always copy /etc/multipath/bindings to the TARGET_FS_ROOT (issues #1382, #1393)


### Version 2.1 (June 2017)

* Support for Grub2 installation with software RAID1 on Linux on POWER (ppc64/ppc64le) (issue #1369)

* REBUILD_INITRAMFS variable was introduced. The new default.conf setting REBUILD_INITRAMFS="yes" rebuilds the initramfs/initrd during "rear recover" to be more on the safe side. With REBUILD_INITRAMFS="" the old behaviour can still be specified (issue #1321)

* ISO_RECOVER_MODE=unattended mode (issue #1351) - required for automated ReaR testing with OUTPUT=ISO

* MODULES variable supports now special values like 'all_modules', 'loaded_modules', 'no_modules' (issues #1202, #1355)

* Include systemd/network to preserve "Predictable Network Interface Names" (issue #1349)

* Various improvements regarding multipath (issues #1190, #1309, #1310, #1311, #1314, #1315, #1324, #1325, #1328, #1329, #1344, #1346)

* Show OUTPUT variables in `rear dump` (issue #1337)

* Added support for "grub PXE style" via PXE_CONFIG_GRUB_STYLE and PXE_TFTP_IP on non x86 platform (issue #1339)

* Try 'wipefs --force' and use 'dd' as fallback to better clean up disk partitions (issue #1327)

* Reorganized "finalize" scripts ordering and cleanup of the PPC bootloader installation (issue #1323)

* Avoid long default wait in 'dig' when DNS servers are not set (issue #1319)

* Fail-safe calculations in partitioning code (issues #1269, #1307)

* Improved support on ppc/ppc64/ppc64le architectures (issues #1178, #1311, #1313, #1322)

* Define hostname in both /etc/HOSTNAME and /etc/hostname in rescue image (for Arch) (issue #1316)

* Rename network interface when MAC not present in udev (issue #1312)

* Added support for 'nano' editor (in addition to 'vi') (issues #1298, #1306)

* mmcblk disk types are now supported (issues #1301, #1302)

* NETFS_RESTORE_CAPABILITIES variable was introduced to restore file capabilities in a proper way (issue #1283)

* Added required libs and files for 'curl' with HTTPs by default (issues #1267, #1279)

* More precise XFS file system creation during `rear recover` (issues #1208, #1213, #1276)

* DRLM management and security improvements (issue #1252)

* Improved BOOTLOADER support (issue #1242)

* DRLM support for multiple backups via multiple config files (issue #1229)

* FIRMWARE_FILES support to exclude firmware files in rescue image to reduce the size of image (issue #1216)

* Enable SELinux in the rescue image for tar internal backup method if BACKUP_SELINUX_DISABLE=0 (issue #1215)

* BOOT_OVER_SAN is now fully supported (issues #1190, #1309, #1314, #1315, #1325, #1329, #1344)

* NVME disks are now fully supported (issue #1191)

* Some initial basic support for new backup type ZYPPER was added (issues #1085, #1209)

* Finding UEFI boot loaders on non standard places (issues #1204, #1225, #1293)

* The USB UEFI partition size USB_UEFI_PART_SIZE for kernel image has been increased from 100 to 200 MB (issue #1205)

* REAR_INITRD_COMPRESSION variable was introduced to specify initrd compression (e.g. 'lzma' for PPC64) (issues #1142, #1218, #1290)

* New backup type BLOCKCLONE was added to backup non-Linux partitions (e.g. Windows NTFS partitions) (issues #1078, #1162, #1172, #1180)

* Bareos 16.2 is now supported (issue #1169)

* New USB_PARTITION_ALIGN_BLOCK_SIZE and USB_DEVICE_FILESYSTEM_PARAMS variables were added (issue #1217)

* Improved the USB backup selection menu during the recovery via USB (issue #1166)

* USB_SUFFIX variable was introduced to align backup on USB with backup on NFS (issues #1164, #1160, #1145)

* Forbid incremental backup to work on BACKUP_URL=usb:// (issue #1146)

* The USB_DEVICE_PARTED_LABEL=gpt setting is now honered while formatting the USB disk (issue #1153)

### Version 2.00 (January 2017)

(*Important Note*) ReaR 2.00 introduced the 3-digits scripts instead of the 2-digits script. This means all scripts must begin with 3 digits, e.g. 010-my-script.sh instead of 10-my-script. Therefore, if you wrote your own scripts make sure to renumber these. You could also use the *make validate* to check this.

* Bareos support: add missing directory /var/run/bareos in recovery system (issue #1148)

* Forbid BACKUP_URL=usb for BACKUP_TYPE=incremental/differential (issues #1141 and #1145)

* Improved and added new example configurations (issue #1068, #1058)

* Modified/Improved the exit code messages of ReaR (issues #1089, #1133)

* Fix documentation regarding OUTPUT_URL=null (issues #734, #1130)

* Better and fail safe progress messages while tar backup restore (issue #1116)

* Implement simulation mode with simulation with the workflows validate and shell (issue #1098)

* Update 11-multiple-backups.adoc : Multiple backups are in general not supported for
BACKUP_TYPE=incremental or BACKUP_TYPE=differential (issues #1074 and #1123)

* Using RUNTIME_LOGFILE in all scripts as needed (issue #1119)

* New Backup method was added - BORG (issues #1030, #1037, #1046, #1048, #1118)

* Multiple backups are now possible (issues #1088, #1102, #1096) - see the [documentation page](https://github.com/rear/rear/blob/master/doc/user-guide/11-multiple-backups.adoc) (*New*)

* Support partitioning and formatting huge USB devices (issue #1105)

* Skip remount async when systemd is used (issue #1097)

* Fixed and enhanced code for multiple ISOs (issue #1081)

* BACKUP_TYPE=incremental and BACKUP_TYPE=differential were updated (issues #974, #1069)

* Added support for setting a UUID on XFS with enabled CRC (RHEL 7) (issue #1065)

* Fix for ISO not bootable for SLES11 ppc64 with root LVM (issue #1061)

* PXE booting enhancement with new style of uploading the boot files (issue #193)

* Renumbering the ReaR scripts from 2-digits to 3-digits (issue #1051)

* Improved boot loader detection (issue #1038)

### Version 1.19.0 (October  2016)

* Save bootloader info from POWER architecture and rebuild initrd after migration (issues #1029, #1031)

* Improved documentation and man page in general (issues #918, #930, #1004, #1007, #1008)

* New SLE12-SP2-btrfs-example.conf file because since SLES12-SP2 btrfs quota setup for snapper via "snapper setup-quota" is needed (issue #999)

* Simplified reboot halt poweroff and shutdown in the rescue/recovery system in case of systemd (issue #953)

* If TSM parameters contain a dot, the dot is replaced by an underscore in the TSM_SYS variable names (issues #985 and #986)

* Check if /dev/disk/by-label/RELAXRECOVER exist (issues #979 and #326)

* Added PRE_BACKUP_SCRIPT and POST_BACKUP_SCRIPT to be able to do custom tasks in the mkbackup/mkbackuponly workflows (issue #977)

* Make TMPDIR work in compliance with Unix standards (issue #969)

* USE_STATIC_NETWORKING now really overrides USE_DHCLIENT (issue #964)

* Make it safe against wrong btrfs subvolumes on SLES12 (issues #963, #966)

* Encrypted incremental backup cannot read the tar label (issue #952)

* Introduction of the NETWORKING_PREPARATION_COMMANDS variable to prepare network setup in the rescue/recovery system (issue #960)

* After migration fs_uuid for root partition was not changed in ELILO config file /etc/elilo.conf (issue #956)

* Clarified rear man page and default.conf file around BACKUP_URL=rsync: (issues #930 and #918)

* Make "rear recover" work with default btrfs on SLES12-SP2 (issue #944)

* Dropped GRUB_SUPERUSER and GRUB_RESCUE_PASSWORD to avoid that GRUB_RESCUE could change the behaviour of the GRUB2 bootloader in the currently running system in unexpected ways. With the new optional GRUB_RESCUE_USER setting GRUB_RESCUE works in compliance with the existing GRUB2 configuration (issues #938, #942)

* Bail out if not enough disk space for GRUB and GRUB2 rescue image (issue #913)

* Use BACKUP_PROG_COMPRESS_OPTIONS as an array so that one can use it to provide more complex values (issue #904)

* Add /usr/lib/syslinux/bios to the search path for mbr.bin (issue #908)

* Always load modules in /etc/modules (issue #905)

* Ask user for EFI partition size on USB disk (issue #849)

* Insure /etc/rear/mappings directory exists before doing a recovery (issue #861)

* First steps for rescue/recovery system update support via RECOVERY_UPDATE_URL (issue #841)

* NFS mount points are not recreated after a recover (issue #818)

* Correcting ReaR return code handling in auto recover mode (issue #893)

* Added NFSv4 support for security 'sys' only so far (issue #754)

* Changed the usage of 'rpcinfo -p' a bit to have the same outcome of different Linux flavours (issue #889)

* RSYNC: /boot/efi needs --relative rsync option (issue #871)

* New variables added for Bareos: BAREOS_RESTORE_JOB and BAREOS_FILESET

* Multipath partition not found in rhel7.2 (issue #875)

* Adding support for ppc64le PowerNV (non-virtualized aka Bare-Metal) (issue #863)

* First steps to support new ftpfs BACKUP_URL scheme (issue #845)

* Clean up 'url_host()' (issue #856)

* Fix that libaio (needed for multipath) could be missing in rescue/recovery system because libaio can be located in different directories (issue #852)

* Improved the Relax-and-Recover menu for GRUB2 (issues #844, #849, #850)

* Check for valid BACKUP_URL schemes (issue #842)

* USB UEFI boot support (issue #831)

* Mitigate the problem that btrfs subvolums are not restored by default via TSM (issue #833)

* Determine EFI virtual disk size automatically (issue #816)

* ebiso image size is too small if BACKUP=TSM (issue #811)

* Improving the logics around ebiso usage in UEFI mode (issue #801)

* Fix for wrong UUID in initrd for bootfs (issues #649 and #851)



### Version 1.18.0 (March 2016)

* Support was added for NVME SSD type of disk devices (issue #787)

* For LUKS added the password libraries (issue #679)

* Script 99_sysreqs.sh was added to save the minimal system requirements necessary for cloning
  a system in a remote DRP data center (issue #798)

* New 99_move_away_restored_files.sh to remove restored files after recover (issue #799)
  New array was introduced to make this - BACKUP_RESTORE_MOVE_AWAY_FILES=()

* Improved 40-start-udev-or-load-modules.sh script for better udevd handling (issue #766)

* Run ldconfig -X before dhclient gets started at boot time (issue #772)

* Remove the "-c3" option fron rsyslogd start-up (issue #773)

* Add example configuration for NetBackup Master/Media server

* Added backup capabilities; getcap and setcap are used to backup and restore (issue #771)

* Correct bash syntax so ReaR is compatible with bash v3 and v4 (issue #765, #767)

* Added support for new backup method Novastor NovaBACKUP DC (BACKUP=NBKDC) (issue #669)

* Code was improved to have network teaming support (issue #655)

* Example configuration to put backup and rescue image on same ISO image, eg. DVD (issue #430)

* Improved the ReaR documentation

* remove the noatime mount option for cifs mount (issue #752)

* Replace option 'grep -P' to 'grep -E' due to SELinux errors (issues #565, #737)

* Hidding the encryption key while doing the restore in the rear.log (issue #749)

* is_true function was to uniform the different ways of enable/disble variable settings (issue #625)

* Added and use sysctl.conf; rescue mode should honor these settings (issue #748)

* The BACKUP_PROG_COMPRESS variable was not used during incremental backup (issue #743)

* prevent any other workflow in ReaR rescue mode then recover (issue #719)

* Exclude Oracle ASM device directory from Rescue System (issue #721)

* SaveBashFlagsAndOptions and RestoreBashFlagsAndOptions in usr/share/rear/lib/framework-functions.sh were added (issue #700)

* /mnt/local became a global variable TARGET_FS_ROOT (issue #708)

* Copy rear.log from recovery into /var/log/rear/recovery/ directory after a 'rear recover' (issue #706)

* wipefs will be used when available (issue #649)

* SAN related improvements with btrfs (issue #695)

* Support for shim.efi (UEFI booting) added (issue #702)

* Added support for elilo (used by SLES 11/12) (issue #583, #691, #692, #693)

* Added the --debugscripts command line option (help-workflow) (issue #688)

* Removed dosfslabel as required program for vfat UEFI boot partition (issue #694)

* Bareos team added BAREOS_FILESET and ISO_DEFAULT to default.conf (for automated DR tests executed by Bareos team; issues #686, #719)

* Fix getty/agetty with upstart (issue #685)

* New SLE11-SLE12-SAP-HANA-UEFI-example.conf (issue #683)

* usr/share/rear/conf/examples/SLE12-SP1-btrfs-example.conf added as an example configuration file

* Added support for the SUSE specific gpt_sync_mbr partitioning scheme (issue #544)

* Improved btrfs snapshot support with SLES 12 (issue #556)

* Unload scsi_debug driver in recovery mode for RHEL 7.1 (issue #626)

* Saved the current mount points and permissions; in order to improve and avoid missing mount points after recovery (issue #619)

* NSR servername not defined causing ReaR to hang (issue #637)

* Removed mingetty as a required package (issue #661)

* Adding -scrollprompt=no to dsmc query  in script verify/TSM/default/40_verify_tsm.sh (issue #667)

* Fixed a bug around USB_DEVICE and OUTPUT_URL mis-match (issue #579)

* grub support for ppc64 (issue #673)

* grub2 supported was added for ppc64 (issue #672)

* ppc64le support was added into the rear.spec (issue #665)

* Network code partially rewritten to improve teaming (issue #662)

* Changed default value of USE_CFG2HTML from 1 to empty (means do not run cfg2html by default) (issue #643)

* Move the 50_selinux_autorelabel.sh script to the default location so it gets picked up by all backup methods. This was required for RHEL 7 (issue #650)

* Check via NSR if the ISO image is not obsolete (issue #653)

* Added ebiso support within ReaR (required for UEFI booting with SLES 11 & 12 (issue #657)

* FDR/Upstream (BACKUP=FDRUPSTREAM) (*New*) (issue #659)

### Version 1.17.2 (August 2015)

* Several fixed required to the Debian packaging rules needed so it builds correctly on OBS

* Fixed the NTP startup script (issue #641)

* Fixed the vfat label issue (issue #647)

* Improved the DUPLICITY method with finding all required libraries and python scripts

* Added the `/run` directory to the list of recreating missing directories (issue #619)

* Fix issue with USB disk and rsync as internal backup program (issue #645)

* Fix rsync restore: --anchored invalid rsync option (issue #642)

* A new variable was introduced `NSR_POOLNAME` (issue #640)

* Replaced almost all temporary files from `/tmp/` to `$TMP_DIR/` (issue #607)
  Related to security recommendations for Fedora and RHEL:
  - https://bugzilla.redhat.com/show_bug.cgi?id=1239009 (f22)
  - https://bugzilla.redhat.com/show_bug.cgi?id=1238843 (rhel 7.2)

* Move nfs-client from depends to recommends in Debian control file (issue #633)

* In `packaging/rpm/rear.spec` replaced "BuildArch: noarch" with "ExclusiveArch: %ix86 x86_64 ppc ppc64" that should tell the user that ReaR is known to work only on %ix86 x86_64 ppc ppc64 and removed "Requires: yaboot" for ppc ppc64 because that is the default installed bootloader on ppc ppc64 (issues #629 and #631)

* Support the Oracle Linux 6 ksplice module (issue #605)

* In script 27_hpraid_layout.sh added the missing `HPSSACLI_BIN_INSTALLATION_DIR` variable to the `COPY_AS_IS` array (issue #630)

* Modified the packaging Makefile and rules for debian to fix the failing OBS Debian builds (issue #604)

* Syslinux version > 5.00 is now supported (ISO and USB output) - works on Debian 8, Ubuntu 15.04 (issue #624)

* Bail out when syslinux/modules are not found in lib/bootloader-functions.sh (issues #467 and #596)
  You could also define a variable `SYSLINUX_MODULES_DIR` if ReaR cannot find it automatically (should not be necessary)

* Support was added for SLES11 on PPC64 hardware (issues #616 and #628)

* Support was added for new hardware - PPC64LE - RHEL and Ubuntu (issue #627)

* FIX the hashed password (`SSH_ROOT_PASSWORD` variable) and added a missing library libfreeblpriv3 (issue #560)

* Insert a 3 seconds sleep after a volume group restauration to give udevd or systemd-udevd time to create needed devices (issue #608 and #617)

* Variable `MANUAL_INCLUDE=YES` has been introduced to work with array `BACKUP_PROG_INCLUDE` (issue #597)

* Add new variable `NSR_DEFAULT_POOL_NAME` (defaulting to `Default`) to use a different default pool name. Renamed the `RETENTION_TIME` variable to `NSR_RETENTION_TIME` (issue #640)

* ReaR website shows the user guide which is part of the ReaR software (linked to GitHub)

* new document 10-integrating-external-backup.adoc which explains the steps to take for a new backup integration into ReaR

* All AsciiDoc documentation changed extention from .txt to .adoc

### Version 1.17.1 (June 2015)

* Removed the plain password in the logs (and output) coming from BACKUP_PROG_CRYPT_KEY to avoid crib (issue #568)

* Mount vfat file system without special mount options seems to work much better then with these options in recovery mode, therefore,
  we do not use these anymore (especially for /boot/efi) (issue #576)

* Elilo support has been added for SLES (not fully tested yet) - issue #583

* Grub2 rescue menu has been added (enable this feature with `GRUB_RESCUE=y`) - issue #589

* splitted script 31_include_uefi_tools.sh in two pieces: 31_include_uefi_tools.sh: to grab the UEFI tools (as long as /boot/efi is mounted), and 32_include_uefi_env.sh: to dig deeper into the configuration when UEFI is active (related to issue #214)

  This is necessary to have to UEFI tools on SLES 11/12 where we cannot create an UEFI bootable ISO image. We must boot in BIOS mode, and need the UEFI tools to make the system bootable over UEFI.

* It is now possible to format an USB disk with a vfat and ext3 partition (for UEFI booting) - issue #593

    `rear -v format -- --efi /dev/<usbdevice>`

* Simplified the code for ext* fs and added StopIfError calls to prevent divide by zero during recovery (issue #598)

* Syslinux version >6 requires some special handling due to splitting up the package (Ubuntu 15.04) (issue #584)

* Debian 8 support added with ISO booting with latest syslinux release as well (issues #596, #599 and #600)

* Changed the behavior of SSH_ROOT_PASSWORD - now saved as MD5 hash password, but backwards compatibility is still respected (issue #560)

* For EMC NetWorker server/client we added some exclude items to COPY_AS_IS_EXCLUDE_NSR (issue #571)

* Removed the Warning message from main usr/sbin/rear script as it was misleading (issues #563 and #564)

* output/ISO/Linux-i386/80_create_isofs.sh: make sure ISO_FILES[@] are copied to isofs directory (issue #569)


## System and Software Requirements

Relax-and-Recover works on GNU/Linux kernel with version 2.6 and higher.
For lower kernel versions Relax-and-Recover cannot be used, and for these
systems, [mkcdrec](http://mkcdrec.sourceforge.net/) is still a good
alternative.

As Relax-and-Recover has been solely written in the *bash* language we need
the bash shell which is standard available on all GNU/Linux based systems.
The default backup program Relax-and-Recover uses is GNU/tar which is also
standard available.

Relax-and-Recover is known to work well on x86, x86_64 and ppc64(le) based architectures.
Relax-and-Recover has also been ported to ia64 and arm architectures, but
these are less tested. Use the '`rear validate`' command after every
successful DR test please and mail us the results.


### Choosing the best compression algorithm

The default backup program with Relax-and-Recover is (`BACKUP_PROG=tar`)
GNU tar and the default compression used with tar is `gzip`. However, is
using `gzip` the best choice? We have done some tests and published the
results. See
[Relax-and-Recover compression tests](http://www.it3.be/2013/09/16/NETFS-compression-tests/)

## Support

Relax-and-Recover (ReaR) is an Open Source project under GPL v3 license which means
it is free to use and modify. However, the creators of ReaR have spend many, many hours in
development and support. We will only give *free of charge* support in our free time (and when work/home balance
allows it).

That does not mean we let our user basis in the cold as we do deliver support as a service (not free of charge).

## Supported and Unsupported Operating Systems

We try to keep our wiki page [Test Matrix rear 2.6](https://github.com/rear/rear/wiki/Test-Matrix-rear-2.6) up-to-date with feedback we receive from the community.

ReaR-2.6 is supported on the following Linux based operating systems:

* Fedora 29, 30, 31, and 32
* RHEL 6, 7, and 8
* CentOS 6, 7, and 8
* Scientific Linux 6 and 7
* SLES 12 and 15
* openSUSE Leap 15.x
* Debian 8, and 9
* Ubuntu 16, 17, and 18

ReaR-2.6 dropped official support for the following Linux based operating systems:

* Fedora < 29
* RHEL < 6
* CentOS < 6
* Scientific Linux < 6
* SLES < 12
* openSUSE Leap 42.x and before (i.e. openSUSE <= 13)
* openSUSE Tumbleweed
* Debian < 8
* Ubuntu < 16

Usually ReaR-2.6 should also work on newer versions of the above listed supported Linux based operating systems but sometimes arbitrary failures can happen when software that is used by ReaR (like partitioning tools, filesystem tools, bootloader tools, ISO image creating tools, networking tools, and so on) changes in not fully backward compatible ways or when there are innovations of the basic system (like kernel, storage, bootloader, init, networking, and so on) that are not yet supported by ReaR.

In theory ReaR-2.6 should work on openSUSE Tumbleweed but in practice arbitrary failures could happen at any time because the Tumbleweed distribution is a pure rolling release version of openSUSE containing the latest stable versions of all software (cf. https://en.opensuse.org/Portal:Tumbleweed) so arbitrary changes of any software are possible at any time that could arbitrarily break how ReaR works.

ReaR-2.6 may still work for SLES 11 and openSUSE Leap 42.x or even earlier openSUSE versions but it is no longer sufficiently well tested there so arbitrary regressions could appear.

ReaR 2.6, ReaR-2.5, and ReaR-2.4 (and probably also some earlier versions) are known to no longer work reasonably well for the following Linux based operating systems:

* RHEL 5 (and probably also CentOS 5): See issue #1766
* SLES 9 and 10: See issue #1842

If you require support for *unsupported* Linux operating systems you must acquire a *ReaR support contract*.

Requests to port ReaR to another operating system (not Linux) can only be achieved with **serious** sponsoring.

## Supported and Unsupported Architectures

ReaR-2.6 is supported on:

* Intel x86 type of processors
* AMD x86 type of processors
* PPC64 processors
* PPC64LE processors

ReaR-2.6 may or may not work on:

* Intel Itanium processors
* ARM type of processors
* IBM Z "s390x" type of processors

ReaR-2.6 does not support:

* old PPC (32bit) processors

If you feel the need to get a fully functional ReaR working on one of the above mentioned type of processors please buy
consultancy from one of our official developers.

## Supported ReaR versions

ReaR has a long history (since 2006) and we cannot support all released versions. If you have a problem we urge you to install the latest stable ReaR version or the development version (available on GitHub) before submitting an issue.

However, we do understand that it is not always possible to install on hundreds of systems the latest version so we are willing to support previous versions of ReaR if you buy a support contract. Why do we change our policy? We cannot handle the big support requests anymore and we must give paid projects priority, therefore, we urge our customers to buy a support contract for one or more systems. You buy time with our core developers.


## Known Problems and Workarounds

*Issue Description*: 'rear' package on Ubuntu 14.04 depends on isolinux package (which does not exist)

* Workaround:

Read the comments in [issue #1403](https://github.com/rear/rear/issues/1403)


*Issue Description*: tar --test-label is not supported on Centos 5 who have tar version 1.15

* Workaround:

Read the comments in [issue #1014](https://github.com/rear/rear/issues/1014)


*Issue Description*: BACKUP=NSR on RHEL 6 could break yum

[Issue #387](https://github.com/rear/rear/issues/387) describes a problem seen on RHEL 6 where when `rear` uses NSR and afterwards the link `/lib64/libexpat.so.1` has been changed.

* Workaround:

So far there is *no* workaround for this issue.

*Issue Description*: usage of an alternative configuration directory is different in mkbackup or recover mode

Using `rear -v -c /etc/rear/mydir mkbackup` works fine in production, but when you try (once booted from rescue image) `rear -v -c /etc/rear/mydir recover` it will fail.

* Workaround:

The configuration files are copied to `/etc/rear/` into the rescue image, so you need to type: `rear -v recover`
See issue #512

*Issue Description*: Is there a possibility to add btrfs subvolume to a rsync backup

* Workaround:

At present (release 1.18) there is no workaround in place. If you happen to know how this could be fixed then add your ideas to [issue #417](https://github.com/rear/rear/issues/417)

*Issue Description*: UEFI ISO booting does not work on openSUSE 12.x, or SLES 11/12

* Workaround:

At present (release 1.18.x and higher) `genisoimage` cannot produce ISO images that can boot via UEFI on an openSUSE distribution (and also SLES). However, use the [`ebiso`](http://download.opensuse.org/repositories/Archiving:/Backup:/Rear/SLE_11_SP3/x86_64/ebiso-0.2.3-1.x86_64.rpm) package instead to create UEFI ISO images on SLES.


*Issue Description*: System reconfiguration still has some weaknesses.

* this has to be tested before relying on it, there are too many unknowns
involved so that we cannot guarantee anything in this area. It has been
developed mostly as a P2V tool to migrate HP servers to VMware Vms

* hard disks need to be at least of the same size and amount as in the
original system, ATM this is a simple 1:1 mapping of old to new disks,
there is no removal of RAID groups or merging of smaller disks onto a
bigger one or making stuff smaller.

* any use of _/dev/disk/by-path_ or _/dev/disk/by-id_ is untested and will
most likely not work. In some cases Relax-and-Recover will print a warning,
but we are not able to detect all cases. Typically this leads to unbootable
systems or bad _/etc/fstab_ files


*Issue Description*: If SELinux is not disabled during backup (variable
`BACKUP_SELINUX_DISABLE=` in _/etc/rear/local.conf_) then we might see
errors in the `rear-$(hostname).log` file such as:

    tar: var/cache/yum/i386/15/updates/packages: Cannot setfilecon: No such file or directory

* Workaround:

Make sure the `BACKUP_URL` destination understands extended attributes
(CIFS is out of the question and NFS is problematic). When using local
disks (or external USB devices) make sure the proper mount options are
given in the `BACKUP_OPTIONS` variable, e.g.:


    BACKUP_OPTIONS="rw,relatime,seclabel,user_xattr,acl,barrier=1,data=ordered"


(TIP) `BACKUP_SELINUX_DISABLE=1` variable has been introduced in the
_/usr/share/rear/conf/default.conf_ file to disable SELinux
while the backup is running (default setting).

*Issue Description*: ERROR: FindStorageDrivers called but STORAGE_DRIVERS is empty

Above error message might be seen after a fresh installation of the GNU/Linux kernel. ReaR got confused between the running kernel version number and the actual fresh kernel available.

* Workaround:

Reboot your server before using ReaR again, which is a good practice anyway after upgrading the GNU/Linux kernel.

