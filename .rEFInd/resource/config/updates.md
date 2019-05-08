0.11.2 (10/22/2017):
--------------------

- Fixed a bug that caused the hidden-tags feature to not work unless
  it was explicitly set in refind.conf. (It should have been enabled by
  default, and now it is.)

- Fixed a bug (introduced in 0.11.1) that caused setting volumes in
  manual boot stanzas to fail.

0.11.1 (10/9/2017):
-------------------

- Modified refind-install to be smarter about modifying NVRAM entries under
  Linux. It now re-creates existing rEFInd entries only if the existing
  entries don't point to the same location that rEFInd will use.

- Modified the way rEFInd tracks boot loader files. This was made necessary
  by macOS 10.13/APFS, which was confusing rEFInd's old tracking code when
  it was compiled with GNU-EFI. Ideally, this will have no noticeable effect
  to end users; however, it's possible that some loaders will appear or
  disappear from the menu, or "file not found" errors when launching loaders
  will go away, after upgrading to this version of rEFInd.

- Added support for new macOS boot loader locations used in macOS 10.13
  ("High Sierra").

- Fixed bug that could cause hidden-tag maintenance tool to hang.

- Fixed bug, introduced in 0.11.0, that caused the Apple and Microsoft
  recovery tools to not be detected.

0.11.0 (8/13/2017):
-------------------

- Fixed lack of scaling of disk badges when other icons were properly scaled
  on HiDPI/retina displays.

- Added new --encryptkeys option to refind-install, to improve the security
  of locally-generated Secure Boot keys stored on your computer's hard disk.

- Fixed spoof_osx_version to work with newer Apple EFIs.

- Added new mouse support: Uncomment enable_mouse in refind.conf to activate
  this support. You can select an OS to launch with this feature active, but
  you cannot use submenus via the mouse. The mouse_size and mouse_speed
  tokens control the mouse size in pixels and tracking speed, respectively.
  Note that not all computers support mice in their EFIs, which is why this
  feature is disabled by default.

- Fixed bug that caused specifying a full path to the fallback boot
  loader (EFI/BOOT/bootx64.efi) in dont_scan_files to fail to work.

- Added dont_scan_tools refind.conf token to enable hiding EFI tools.
  This feature overrides scantools -- if the latter includes, say, "shell",
  but the former includes "shell.efi", any shell called "shell.efi" will be
  ignored. As with dont_scan_files, you can specify a "bare" filename or a
  complete path. The point is to enable hiding redundant tools -- say, if
  multiple MokManager tools show up in the menu and you want just one.

- A new feature enables you to hide OSes and external tool tags from the
  rEFInd main menu: Highlight the OS tag and hit "-" or the Delete key. The
  OS tag should disappear, and remain gone between reboots. A new tool tag
  (on the second row) with a recycling icon enables recovering OS tags
  hidden in this way.

- You can now specify volumes by GUID number in dont_scan_dirs and
  dont_scan_files specifications -- e.g., "dont_scan_files
  2C17D5ED-850D-4F76-BA31-47A561740082:\EFI\badloader\badloader.efi" to keep
  EFI\badloader\badloader.efi on the volume with a partition GUID value of
  2C17D5ED-850D-4F76-BA31-47A561740082 off the menu.

- I've removed support for the previously-deprecated volume-number style
  volume specifications (fs0:, fs1:, etc.).

- Fixed bug in mvrefind that caused it to not move the EFI/BOOT-rEFIndBackup
  directory back to EFI/BOOT when renaming rEFInd in EFI/BOOT to something
  else.

- The refind-install and mvrefind scripts now create a BOOT.CSV file in the
  rEFInd installation directory. The fallback.efi (aka fbx64.efi) program
  delivered with many distributions can use this file to re-create rEFInd's
  NVRAM boot entry should it be lost. Some distributions (such as Fedora)
  set up fallback.efi in the EFI/BOOT directory on the ESP, so it will run
  automatically if NVRAM variables are lost. All this said, there's no
  guarantee that rEFInd will be FIRST in the boot order should the NVRAM
  entries be lost.

- Improved error messaging on Macs; most serious system errors should now
  be reported on-screen in graphics mode.

- Fixed bug that caused Fedora/CentOS/RHEL "vmlinuz-0-rescue*" kernels to
  not be sorted down in the kernel list if they happened to be the first
  ones passed to rEFInd by the EFI.

0.10.9 (7/30/2017):
-------------------

- The PauseForKey() function now works on Macs in graphics mode, which can
  help when some errors are encountered. (Most of the errors associated with
  this function still rely on Print(), which still doesn't work on Macs; but
  at least there will be a prompt to press a key.)

- rEFInd now displays its background and banner before scanning for boot
  loaders, and displays a message that it's performing this scan.

- Added support for HiDPI/retina displays: rEFInd now automatically scales
  icons and text to twice the normal size when the screen's horizontal
  resolution is above 1920. This does not occur in text mode (you must still
  select an appropriate text-mode resolution via the "textmode" option in
  refind.conf), and it can be overridden by explicitly setting the
  "small_icon_size", "big_icon_size", and "font" options in refind.conf.
  Note that icons are resized from their originals and so may look a bit
  chunky, by HiDPI standards; but rEFInd now embeds both 14- and 28-point
  fonts, so the fonts will look better. If the screen is HiDPI but 1920
  pixels wide or less, the default icon and font sizes are used, so
  adjustment via refind.conf options may still be desirable.

- Added os_trusty.png, os_xenial.png and os_zesty.png icons, for those who
  have multiple Ubuntu versions and want to distinguish them visually.

- Updated LodePNG, which is used to load PNG images, to the latest version
  (20161127).

- A new "edk2" target to "make" is available, which builds in the standard
  TianoCore way. This has the advantage over the "tiano" target of working
  with newer versions of EDK2, including the recently-released UDK2017
  . (The
  "tiano" target maxes out at UDK2014.)

- Improved compatibility with standard TianoCore-style build process; can
  now build rEFInd binary, gptsync, and drivers in the "TianoCore" way, in
  addition to by using Linux Makefiles.

0.10.8 (5/21/2017):
-------------------

- Added shimx64.efi.signed as a valid Shim source filename, and
  mm{arch}.efi.signed as a valid MokManager source filename, to
  refind-install script. This enables users on Ubuntu to point to Ubuntu's
  signed Shim and MokManager files, which are stored in /usr/lib/shim under
  these odd filenames.

- Fixed compile problems with GNU-EFI 3.0.5.

- Added icon for Devuan GNU+Linux, based on the icon in the Devuan press kit
  (https://devuan.org/os/press/).

- Minor code efficiency improvements.

- Added ability to specify volumes by partition unique GUIDs in
  dont_scan_volumes.

- Renamed "Mac OS X" to "macOS" in assorted messages, per Apple's
  re-branding with version 10.12 (Sierra). Unfortunately, I don't know of an
  easy way to tell which macOS version a given macOS boot loader will load,
  so it's one name or the other for everything.

0.10.7 (4/17/2017):
-------------------

- Update refind-install to recognize and copy mmx64.efi as alternative
  name for MokManager.

- Hide volume name "Recovery HD," if that's what it is, from loader
  description. This is done because some OS X users are getting confused and
  even upset over this detail, even though it's accurate, at least from an
  EFI/rEFInd point of view.

- Fixed memory management bug (introduced in 0.10.6) that caused error
  messages to be displayed on some systems.

0.10.6 (4/16/2017):
-------------------

- Fixed bug in drivers that could cause filesystems to not be registered,
  and perhaps other unknown problems. This manifested on 32-bit GNU-EFI
  compiles, but in principle it could cause problems on 64-bit or TianoCore
  builds, too.

- Fixed bug in mvrefind that could cause it to fail to move the rEFInd
  installation because it incorrectly converted the target directory to an
  empty string if the target directory did not (yet) exist.

- Added mm{arch}.efi as MokManager filename, and fb{arch}.efi to list
  of boot loaders to be ignored.

- New refind.conf token: extra_kernel_version_strings, which sets strings
  that are treated something like digits for purposes of matching Linux
  kernel and initrd filenames.

- Don't set the video mode if the computer is already running in the
  requested mode. This is a shot-in-the-dark attempt to fix problems with
  Mac "retina" displays, which tend to get bumped down to a lower resolution
  by rEFInd.

0.10.5 (3/4/2017):
------------------

- Two improvements to initrd detection for Linux kernels:
  - If multiple initrd files match the kernel's version number, the file
    with more matching characters after the version number is used, rather
    than the first initrd file found.
  - The "%v" string, if present in the refind_linux.conf file's second
    field, will be replaced by the kernel version number. Thus, you can
    specify options like:
    "Boot with standard initrd" "ro root=/dev/sda2 initrd=initrd-%v-std"
    "Boot with debug initrd"    "ro root=/dev/sda2 initrd=initrd-%v-debug"
    This enables using multiple initrd files per kernel, to be used for
    different purposes.

- Minor code optimization.

- Add new key mappings: Backspace (Delete on Mac keyboards) works the same
  as Esc, and Tab works the same as F2/Insert/+. This is done for the
  benefit of new Apple laptops that lack physical Esc and function keys.

- Fix to refind-install to work better with disks other than /dev/sd? and
  /dev/hd? devices.

- Fixes to touch/tablet support to improve reliability.

0.10.4 (10/9/2016):
-------------------

- Fixed compile problem for drivers with recent versions of GNU-EFI
  (3.0.4, maybe 3.0.3).

- Fixed bug that could cause program crash on startup. (In practice, it
  manifested with GNU-EFI starting with version 3.0.3 or 3.0.4.)

- An anonymous contributor has provided support for touch screens. This
  support requires that the "enable_touch" token be used in refind.conf.
  Note, however, that not all tablet computers have EFIs that provide the
  necessary support in the firmware.

- Martin Whitaker contributed 64-bit support to the ext4fs driver, which
  makes it compatible with ext4fs as written by some recent Linux
  distributions.

- Tweaked refind-install to do a better job of detecting disks other
  than /dev/sd? and /dev/hd? devices.

0.10.3 (4/24/2016):
-------------------

- Altered RPM & Debian installation scripts so as to NOT call sbsign if
  Secure Boot is disabled. This is a response to Ubuntu bug #1574372
  (https://bugs.launchpad.net/ubuntu/+source/sbsigntool/+bug/1574372): In
  Ubuntu 16.04, the sbsign program is segfaulting randomly, which prevents
  proper installation of the program. This change at least permits proper
  installation IF Secure Boot is disabled.

- Changed description of BIOS/CSM/legacy OS loaders on Macs to include the
  string "(Legacy)", so as to more easily identify BIOS/CSM/legacy-mode OSes
  in the rEFInd main menu.

- Added recognition of the fwupx64.efi file as a firmware update tool.
  This filename is excluded from the first-row launchers, and is instead
  presented on the second row, controlled by the "fwupdate" item on the
  "showtools" option line. It's enabled by default. Note that it's still a
  bit unclear to me how this tool is supposed to be used. rEFInd launches it
  with no options, but if it should take options, this will have to be
  changed in the future.

- Tightened exclusion of shell binary filenames from boot loader scan.
  Previously, any filename containing the substring "shell" was excluded
  from scans. Now it's tighter; only files matching one of the filenames in
  the constant SHELL_NAMES in main.c are excluded. This change will enable
  programs with names that include "shell", but that aren't in rEFInd's
  SHELL_NAMES list, such as "shelly.efi", to be shown in the rEFInd main
  menu.

- Fixed bug in NTFS driver that caused it to hang (and thus hang the
  computer) in some situations, particularly when a file on an NTFS volume
  had many fragments and when the computer's CSM was activated. (Fix
  courtesy of "S L.")

- Modified SIP/CSR rotation code: If the csr-active-config EFI variable is
  missing AND the firmware is Apple (as identified by the string "Apple"
  being present in the ST->FirmwareVendor string), rEFInd treats the
  computer as one on which SIP is available and set to the "enabled" state
  (0x10). The upshot is that the SIP/CSR tool will appear if the showtools
  and csr_values options are set appropriately in refind.conf, even if the
  csr-active-config variable is missing from the NVRAM. The point of this
  change is that I've received reports of some Macs that run OS X 10.11 but
  that lack this variable. OS X acts as if SIP were enabled, but rEFInd is
  then unable to disable SIP. This change gives rEFInd the ability to
  disable SIP on such systems. The drawback is that the variable might be
  set on some systems that don't run OS X 10.11. This should be harmless
  from a technical point of view, but the presence of SIP indicators in
  rEFInd could be confusing.

- Added refind-mkdefault script to simplify resetting rEFInd as the default
  boot program in Linux. The intent is to run this after GRUB, Windows, OS
  X, or some other tool takes over as the primary boot manager. It can be
  called from a startup script to handle this task automatically.

0.10.2 (1/26/2016):
-------------------

- Fixed bug in refind-install that caused mountesp to be installed as a FILE
  called /usr/local/bin on OS X if the /usr/local/bin directory did not
  already exist.

- Fixed bug in mvrefind that caused it to fail to move bootmgfw.efi in
  some situations, and another that caused it to give the resulting NVRAM
  entry the default rEFInd name of "rEFInd Boot Manager," rather than the
  intended "Windows Boot Manager" (to work around bugs in some EFIs).

- Worked around bug/quirk in some EFIs (in HP ProBook 6470b laptop, at
  least) that prevented EFI filesystem drivers from working. (Drivers would
  load but not provide access to filesystems.)

- Fixed refind-install bug that caused --usedefault option to not work in OS
  X. (This bug did not affect Linux.)

- Improved Secure Boot detection in refind-install in Linux.

- Fixed bug that caused custom volume badges (vol_*.png) to be read only
  from default location ("icons" subdirectory), effectively eliminating the
  ability to adjust them.

- Added centos.crt and centos.cer public key files.

0.10.1 (12/12/2015):
--------------------

- Change to PPA version: Installing the PPA now queries the user about
  whether to install to the ESP. Upgrades will remember the initial
  selection.

- Modified time-based sorting of loaders in a single directory to push
  anything starting with "vmlinuz-0-rescue" to the end of the list. Fedora
  gives its rescue kernels filenames that begin with that string, and if
  such a kernel happens to be the most recent, treating it normally will
  cause it to become the default when kernel folding is in use. This is
  almost certainly undesirable, so this change keeps the rescue kernel at
  the end of the list instead, which is saner.

- Significantly reworked the project's Makefiles. This should have no
  impact on ordinary users, and even most developers should barely notice
  it; but it should make future extensions to additional platforms or
  building in different environments easier.

- Added workaround to gptsync for issue with some Macs' EFIs that caused
  the program to skip through all prompts, thus accepting the default
  option. This would normally cause gptsync to do nothing.

- Added type code 53746F72-6167-11AA-AA11-00306543ECAC (Apple Core Storage,
  gdisk type AF05) to list of partition types recognized by gptsync.

- Removed Luxi Sans Mono font, since I discovered it was not open source;
  and changed the default font from Nimbus Mono to Liberation Mono.

- Added support for compiling rEFInd for ARM64 (aka AARCH64 or aa64). This
  works with both GNU-EFI and Tianocore UDK2014.SP1.P1. This support is
  currently poorly tested. In particular, I used QEMU on an x86-64 computer
  to create a virtualized ARM64 environment; I've not yet tested on a real
  computer. I couldn't get QEMU to create a video card, so I used a serial
  terminal, which means that the graphics features are untested -- I ran
  rEFInd with "textonly" uncommented in refind.conf. I've tested the ext4fs
  driver but no other drivers, although they all compile. (So does gptsync,
  although it's unlikely to be useful on ARM64.) Some rEFInd features are
  meaningless on ARM64, such as BIOS-mode boot support, anything geared
  toward Macs (csr_values/csr_rotate, spoof_osx_version, etc.), and
  enable_and_lock_vmx.

- Fixed bug that caused rEFInd to fail to scan EFI boot loaders on
  removable media when rEFInd itself was launched from the fallback
  filename.

- Moved detailed descriptions of refind-install from installing.html to
  a refind-install man page. To keep this information Web-accessible, I've
  also created HTML versions of the three man pages and linked them into
  the HTML documentation.

- Updated LodePNG to latest version (20151024).

- Fixed bugs in mkrlconf and in refind-install that could cause some kernel
  options to be excluded from refind_linux.conf. There were two trouble
  conditions:
  - Previously, these scripts assumed that the first option in
    /proc/cmdline was the kernel's filename, but this isn't always the
    case. (In particular, when gummiboot launches the kernel, this is not
    true. It might be an incorrect assumption in some other cases, too.)
    The fix involves checking for likely signs of a kernel filename before
    discarding this first option.
  - These scripts cut the "initrd=*" option from /proc/cmdline, but the
    call to "sed" was overzealous and cut until the end of input. This
    usually worked, since the initrd= option was usually last on the line;
    but if it wasn't, any options following initrd= would be lost.

- Added "kernel*" as a matching pattern for Linux kernels, since this is
  what Gentoo uses by default.

- The refind-install script can now be run as a symbolic link in Linux.
  This enables creating a /usr/sbin/refind-install link in Linux packages,
  with the binaries stashed wherever the package system likes them. This
  feature does NOT work in OS X, but there's relatively little need for it
  there.

0.10.0 (11/8/2015):
-------------------

- Fixed bug that caused refind-install to not unmount the ESP when it
  should under OS X.

- Modified refind-install and mkrlconf scripts to use /proc/cmdline as
  source for default boot options EXCEPT when refind-install receives the
  --root option. In that case, refind-install continues to use
  /etc/default/grub as the source of default options. The idea behind this
  change is that it's more reliable to get boot options from /proc/cmdline
  when the targeted system is the one that's booted; but --root would be
  used from emergency disks or live CDs, in which case the current boot
  options would be completely wrong, so extracting boot options from GRUB
  files is the best bet for getting close to the right options.

- Added "@/boot" to default also_scan_dirs setting. This makes kernels
  show up on Btrfs volumes under Ubuntu (and perhaps others), at least when
  the Btrfs driver is loaded.

- Added new System Integrity Protection (SIP) rotation feature for Macs
  running OS X 10.11 or later. This feature is disabled by default, except
  on CD-R and USB flash drive images, on which it's enabled. To enable it,
  you must make TWO changes to refind.conf: Uncomment the new "csr_values"
  item and add "csr_rotate" to the "showtools" line (uncommenting it, too,
  if it's commented out). If desired, you can set more values on
  "csr_values"; these are comma-delimited one-byte hexadecimal values that
  define various SIP states.  When SIP/CSR rotation is activated, a new
  shield icon appears among the tools. Selecting it causes the next defined
  value to be set and a confirmation message to appear for three seconds.

- Added display of current System Integrity Protection (SIP) mode to
  "About" display.

- Added mountesp script for OS X to (you guessed it!) mount the ESP.

- Renamed support scripts: install.sh to refind-install, mvrefind.sh to
  mvrefind, and mkrlconf.sh to mkrlconf.

- New icons! The old ones were getting to be a jumbled mess of styles,
  particularly for OS tags. I used the AwOken icon set
  (http://alecive.deviantart.com/art/AwOken-163570862) for the core icons,
  then expanded from there by creating my own icons and modifying icons for
  Debian and Elementary OS. I'm also trying to keep better track of
  copyrights and licenses on icons. Between that and some icons being for
  OSes that probably see very little use (FreeDOS and eComstation, for
  instance), a few OS icons have been lost. If you prefer the old icons,
  you can continue to use them by upgrading rEFInd, renaming icons-backup
  to something else (say, icons-classic), and then adding an "icons" line
  in refind.conf to point to the old icons directory.

- Changed from .zip to .tar.gz as source code archive format. I did this
  because Linux is the only officially-supported build platform, and
  tarballs are a more natural fit to a Linux environment. I'm leaving .zip,
  .deb, and .rpm files as the formats for binary packages.

- Added detection of System Integrity Protection (SIP; aka "rootless") mode
  to OS X portion of install.sh script. When detected, and if no existing
  rEFInd installation is found, the script now prints a warning and brief
  instructions of how to enter the Recovery mode to install rEFInd and
  suggests aborting the installation. (The user can override and attempt
  installation anyhow.) If SIP is detected along with an existing rEFInd
  installation, the script moderates the warning and explains that an
  update of a working rEFInd will probably succeed, but that re-installing
  to fix a broken rEFInd will probably fail.

- Added new "spoof_osx_version" token, which takes an OS X version number
  (such as "10.9") as an option. This feature, when enabled, causes rEFInd
  to tell a Mac's firmware that the specified version of OS X is being
  launched. This option is usually unnecessary, but it can help properly
  initialize some hardware -- particularly secondary video devices. OTOH,
  on some Macs it can cause hardware (notably keyboards and mice) to become
  unresponsive, so you should not use this option unnecessarily.

- Worked around an EFI bug that affected my 32-bit Mac Mini: That system
  seems to have a broken EFI, or possibly a buggy CPU, that causes some
  (but not all) conversions from floating-point to integer numbers to hang
  the computer. Such operations were performed only in rEFInd's
  graphics-resizing code, and so would manifest only when icons or
  background images were resized. My fix eliminates the use of
  floating-point operations in the affected function, which eliminates the
  crashes. There may be some degradation in the quality of resized images,
  though, particularly on 32-bit systems. (64-bit systems use larger
  integers, which enable greater precision in my floating-point
  workaround.)

- Under OS X, install.sh can now be run from the recovery system. This may
  help work around OS X 10.11's problems with System Integrity Protection,
  since it should be possible to reboot into the recovery system to install
  rEFInd without disabling SIP for the main installation, even for just one
  boot.

0.9.2 (9/19/2015):
------------------

- Added "--keepname" option to install.sh. This option causes install.sh
  to keep refind_x64.efi named as such rather than rename it as grubx64.efi
  when using Shim. This option is meaningful only if the --shim option is
  also used. This option passes the refind_x64.efi filename as an option to
  Shim, which overrides the default filename of grubx64.efi. A big caveat:
  Only Shim 0.7 and later supports this feature. (Shim 0.4 also works if a
  refind_x64.efi is referred to as "\refind_x64.efi" on the command line,
  but the need for a leading backslash to refer to a file in the same
  directory as Shim is so confusing and wrong that I cannot in good
  conscience support it.) I've not seen signed Shim binaries between 0.4
  and 0.7, so I don't know if any of them might work.

- Implemented a workaround for a bug in Shim 0.8 that prevented
  authentication of more than one binary. If any filesystem drivers were
  installed, the first one would be verified, leaving rEFInd unable to
  launch anything else unless it was signed by a key in the computer's main
  Secure Boot db list.

0.9.1 (9/13/2015):
------------------

- When rEFInd identifies the root (/) partition via the Freedesktop.org
  Discoverable Partitions Specification, it now checks two of the
  partition's attributes, as per the DPS (see
  http://www.freedesktop.org/wiki/Specifications/DiscoverablePartitionsSpec/):
  - The partition's read-only attribute determines whether to pass a "rw"
    or "ro" option to the kernel.
  - If the partition's do-not-automount flag is set, rEFInd will not pass
    it as a "root=" option to the kernel. This flag can be used to remove
    all but one partition from consideration as a root (/) partition if a
    system has more than one with the correct type code.

- Improved Freedesktop.org Discoverable Partitions Specification support:
  Previously, if no refind_linux.conf file was present but an /etc/fstab
  file was found, rEFInd ignored the Discoverable Partitions Specification
  filesystem-type codes. This was fine if /etc/fstab contained a valid "/"
  filesystem specification, but if that was absent, the result was no
  "root=" specification being present. Under these circumstances
  (refind_linux.conf absent, /etc/fstab present but lacking a "/" entry),
  rEFInd now tries to identify a device to specify as "root=" via the
  Discoverable Partitions Specification.

- Fixed bug that caused "Found match!" and a prompt to press a key to
  continue to be printed if any partition used the Freedesktop.org
  Discoverable Partitions Specification root-partition GUID. (This
  was leftover debugging/testing code that I somehow missed deleting.)

- Added icon for Elementary OS.

- Added /etc/lsb-release to files scanned for clues about the Linux
  distribution. This file differentiates Mint and Elementary OS from Ubuntu
  better than does /etc/os-release, and may also help with other
  closely-related distributions.

- Improvements to handling of case-insensitive string comparisons. These
  are buggy on some EFIs, and such bugs affect things like dont_scan_*
  blacklists, removal of rEFInd's own directory from scanning, matching of
  keyword names in refind.conf, and even loading of icons. I've replaced
  many calls to problematic functions with safer calls, which should help a
  lot. There may still be problems on some systems with some computers,
  though; as far as I can tell, the bugs are buried deep in some EFI
  firmware, so I can only replace some of the most direct calls to
  potentially buggy system calls.

0.9.0 (7/26/2015):
------------------

- New icon for Kali Linux, submitted by Francesco D'Eugenio.

- Minor code changes to ensure that rEFInd compiles with GCC 5.1. (Tested
  with GNU-EFI on a Fedora 22 system; not yet tested with the TianoCore
  EDK2.)

- Added new "fold_linux_kernels" token to refind.conf. This option, when
  active (the default) "folds" all Linux kernels in a directory into a
  single entry on the rEFInd menu. The kernel with the most recent time
  stamp is launched by default. To launch another kernel, you must press F2
  or Insert; additional kernels appear as options on the first kernel's
  submenu. To see the pre-0.9.0 behavior, you must set "fold_linux_kernels
  false" (or one of its synonyms, "off" or "0"). The point of this option
  is to help de-clutter the rEFInd main menu.

- Added new Linux root (/) partition auto-discovery feature, based on
  Freedesktop.org's Discoverable Partitions Spec (DPS)
  (http://www.freedesktop.org/wiki/Specifications/DiscoverablePartitionsSpec/):
  If no refind_linux.conf file or /etc/fstab file is found, and if a
  partition with the correct DPS type code for the system architecture is
  found, rEFInd adds "ro root=/dev/disk/by-partuuid/{GUID}" to the kernel
  options. This will not help on LVM setups, and will get it right for only
  one installation on systems with multiple Linux installations, but it may
  help some users, if/when the DPS type codes become more common.

- Fixed bug that caused a rEFInd crash if an empty refind_linux.conf
  file was encountered.

- The mkrlconf.sh script now checks the OS on which it's running, which
  should help avoid confusion or problems by users who mistakenly run it
  under OS X.

- rEFInd now skips checking for various BIOS-mode boot sector signatures
  when running on a UEFI-based PC; these checks are run only on Macs. This
  may reduce startup time on systems with many partitions.

- Fixed Debian debinstall script to work correctly on IA32 systems. It had
  a bug that caused filesystem drivers and gptsync to not be packaged for
  IA32.

- Modified Debian postinst file to call install.sh with --localkeys option
  if sbsign and openssl are available, even when NOT in Secure Boot mode or
  if shim is not detected. This helps with my Ubuntu PPA when using custom
  Secure Boot keys, since the PPA is delivered unsigned. (Users will have
  to have added their own local keys to their firmware's db.) For
  consistency, I've made the same change to the RPM .spec file.

0.8.7 (3/1/2015):
-----------------

- Fixed install.sh bug that caused inappropriate installation under the
  name bootx64.efi (or bootia32.efi) under Linux, with a failure to update
  the boot entries in NVRAM, has been fixed.

- Added identification of XFS as filesystem type in volume descriptions.

- More fixes to filesystem type detection code. Previous version sometimes
  identified FAT or NTFS (or anything with a boot loader) as a whole-disk
  device rather than the correct filesystem type.

- Added protections to the code to reduce the risk of crashes that might
  occur when dereferencing NULL pointers in various situations.

- I'm deprecating the use of filesystem numbers (as in "fs0:") because
  they're unreliable -- filesystem numbers can change between boots and
  might not be the same as those used in an EFI shell or other program.
  Sooner or later I'll remove code supporting this feature. In the
  meantime, if it doesn't work for you, please switch to using filesystem
  labels, partition labels, or partition GUIDs.

- Added detection of FreeBSD's BIOS-mode GPT boot loader. Previously,
  rEFInd could detect FreeBSD's BIOS-mode MBR boot loader, which gave
  FreeBSD an appropriate icon on Macs; but the BIOS-mode GPT boot loader
  code is different, so some recent FreeBSD installations showed up with
  generic grey diamond icons. This change creates FreeBSD icons instead.

- Added "Secure Boot [active|inactive]" notice to "about" menu for x86
  (32-bit) systems, since there are now a few 32-bit UEFI systems that
  support Secure Boot. (AFAIK, these are mostly tablets and convertibles
  such as the ASUS T100.)

- Added KeyTool.efi and KeyTool-signed.efi to list of MOK managers. KeyTool
  is the "super-deluxe" Secure Boot key and hash manager provided as part
  of the efitools package.

- Fixed more instances of "invalid parameter" errors on some EFIs.

- Improved Secure Boot detection in install.sh.

- install.sh should no longer complain when copying Shim or MokManager over
  itself.

0.8.6 (2/8/2015):
-----------------

- Removed special case of ignoring an HFS+ name of "HFS+ volume", since the
  old rEFInd HFS+ driver that produced this name for all HFS+ volumes has
  long since been updated to deliver a real name.

- Addition of new Windows 8 OS icon. On Macs and for BIOS/legacy boots, the
  new icon is now used for Windows Vista, 7, and 8, while the old one is
  used for earlier versions of Windows. For EFI-mode boots, the new icon is
  used universally.

- If the NTFS driver is loaded, rEFInd now scans NTFS volumes on Macs for
  the presence of Windows boot files, and removes any NTFS volume that
  lacks such files from the BIOS/legacy boot list. This should help
  unclutter the display on Macs that contain NTFS data partitions.

- Fixed bug that caused misidentification of both whole disks and NTFS
  volumes as being FAT. (This bug affected the identification of devices
  and locations in the rEFInd menu, not actual access to devices.)

- Code refactoring to clear out legacy-boot functions from the
  ever-expanding refind/main.c file.

- Added new "badges" option to the "hideui" token in refind.conf. This
  option hides the device-type badges associated with the OS boot options.

- Reverted rEFIt commit r472, introduced in rEFInd 0.8.5 to support more
  BMP images because I've received bug reports that it's causing existing
  selection images to fail to load.

- Fixed install.sh bug that caused misidentification of installation
  directory under OS X if an already-mounted ESP has spaces in its path.

- Fixed Mac-specific install.sh bug that could cause misidentification of
  the ESP on disks with partition numbers of 10 or above.


0.8.5 (2/1/2015):
-----------------

- Added NTFS EFI filesystem driver.

- Minor improvements to filesystem driver framework code.

- Changes to 

- Fixed bug in Btrfs driver's address reference.

- Improved install.sh to make it smarter about figuring out where to
  install on Macs. Specifically, this version now upgrades existing
  installations, if found (as it always has under Linux), rather than
  blindly install to EFI/BOOT; it installs to EFI/refind if not existing
  installation is found; it installs using the --shortform option to bless,
  which seems to eliminate the 30-second delay problem; and it can handle
  an HFS+ ESP, which it treats as a separate HFS+ volume (as if the user
  had used --ownhfs). These changes do not affect behavior under Linux.

- Added missing check of architecture type for several tools.

- Applied rEFIt commit r472, which adds support for BMP images with negative
  height fields, indicating that the image is NOT vertically flipped. This
  commit and r467 were not incorporated in the original rEFInd because I
  forked it from a Debian rEFIt package that had been patched to build
  under GNU-EFI, and was apparently based on a slightly earlier version.

- Applied rEFIt commit r467, which improves Mac handling of legacy boots
  from other than the first hard disk.


0.8.4 (12/8/2014):
------------------

- Tweaked default for dont_scan_volumes: Removed "Recovery HD". This change
  better suits the needs of OS X 10.10 ("Yosemite") installations, but may
  result in some stray Recovery HD entries on some Macs.

- Updated icons for Fedora and Ubuntu and added an icon for Xubuntu.

- Added new configuration option, "enable_and_lock_vmx", which sets an
  Intel CPU feature that's required for some types of virtualization to
  work. Most EFIs enable setting this feature in their own setup utilities,
  but some (such as most Macs) don't.

- If rEFInd can't locate an icons directory (either the default or one
  specified by the icons_dir token), the program switches to text-only
  mode.

- If a loader contains the string "grub" and no other clue to the loader's
  OS association exists, search for os_grub.{png|icns} (which is not
  provided with rEFInd) or os_linux.{png|icns}. (Previous versions provided
  a generic loader icon for GRUB.)

- Fixed bug that caused dont_scan_files to not work with special-case
  boot loaders (for OS X and Windows) when specifying the complete path to
  the loader (e.g., EFI/Microsoft/Boot/bootmgfw.efi).

- Added support for the iPXE network boot tool (see BUILDING.txt for
  building and basic use instructions).

0.8.3 (7/6/2014):
-----------------

- Added new feature: Setting "timeout = -1" in refind.conf causes rEFInd to
  immediately boot the default option UNLESS a keypress is in the buffer
  when rEFInd launches. In that case, if the keypress corresponds to a
  shortcut key, the associated boot loader is launched; or if not, the menu
  is displayed.

- Added new icons for Clover boot loader and for Mythbuntu Linux
  distribution.

- rEFInd now displays the partition's label, when one is available, when
  offering a BIOS-mode boot option for a partition with no filesystem
  driver. This works only on Macs doing BIOS-mode booting.

- Removed GPLv2 code from the FSW core files. This was done because the
  Btrfs driver is derived from the GRUB Btrfs driver, which is licensed
  under the GPLv3. Ironically, the GPLv2 and GPLv3 are incompatible
  licenses, so ensuring that the Btrfs driver doesn't rely on GPLv2 code
  was legally necessary. In most cases, I reverted to the original rEFIt
  code, although I kept my own cache code; since I wrote it, I can
  change its license to a BSD license.

- Fixed bug that caused rEFInd to unload drivers immediately after loading
  them. This didn't affect rEFInd's own drivers because they didn't include
  the unload hooks, but it did affect some other drivers.

- Changed default scan_all_linux_kernels setting from "false" to "true",
  and commented the option out in refind.conf-sample. This should not
  affect most people, since refind.conf-sample had this option commented
  out, and most rEFInd users either use it that way or don't have Linux
  kernels installed at all. I've made this change because I want rEFInd to
  "do the right thing" by default in as many cases as possible. For a while
  now, rEFInd has been excluding non-bootable files from its menu, and most
  kernels "in the wild" now include the EFI stub. Thus, enabling this
  support by default seems worthwhile. If you prefer to not scan Linux
  kernels by default, simply uncomment the "scan_all_linux_kernels" line
  and ensure it's set to "false".

0.8.2.1 (6/8/2014):
-------------------

- Removed stray bit of debugging code that caused a prompt to press a
  key to appear at rEFInd startup.

0.8.2 (6/8/2014):
-----------------

- Changed behavior when default_selection is not set: It now boots the
  previously-booted loader, assuming it's still available; if not, rEFInd
  boots the first loader (as it does now). Behavior is unchanged if
  default_selection is set. Note that this behavior depends on the ability
  of rEFInd to store an EFI variable in NVRAM. It therefore fails on
  systems with flaky NVRAM storage. You can view the previously-booted
  loader in the
  /sys/firmware/efi/efivars/PreviousBoot-36d08fa7-cf0b-42f5-8f14-68df73ed3740
  variable under Linux.

- Added icon for Mageia Linux (os_mageia.png).

- Fixed bug that could misidentify a not-quite-GUID as a GUID in a
  manual boot stanza's "volume" line.

- I've updated my personal build system, and therefore the rEFInd Makefiles
  and related files, to use TianoCore UDK2014 rather than UDK2010.

- Added "deep_uefi_legacy_scan" token. When not set (the default), rEFInd
  does not modify EFI NVRAM settings when scanning for BIOS-mode boot
  loaders on UEFI-based (non-Mac) computers. Some computers require
  uncommenting this setting for rEFInd to reliably detect some BIOS-mode
  boot devices. Passing "0", "off", or "false" as an option resets it to
  the default value (useful in a loaded secondary configuration file to
  override a setting in the main file).

0.8.1 (5/15/2014):
------------------

- Fixed bug that could cause rEFInd to fail to detect boot loaders stored
  on the root directory of a partition.

- Added two new bitmap fonts to those distributed with rEFInd: Ubuntu Mono
  and Nimbus Mono. Both come in 12-, 14-, 16-, and 24-point sizes.

- Messages about pauses for scanning and re-scanning of boot loaders are
  now suppressed when doing an initial delayed scan when scan_delay is 1
  second.

- Improved centering of legacy boot option descriptions on some systems'
  screens.

- Fixed bug that could cause a BIOS-mode boot to boot from an inappropriate
  device if that device had an innately high boot priority (as set by the
  firmware).

- Changed icons from ICNS to PNG form. There are several reasons to do
  this, all of them minor; but together they're enough to warrant a change.
  PNG is more common, and therefore more accessible to most users --
  particularly those who don't use OS X. The PNG files are smaller than
  their ICNS equivalents. PNG supports a wider range of sizes (although I'm
  not now using anything that ICNS doesn't support, I might in the future).
  The icon-scaling support added a few versions ago makes ICNS's support
  for multiple icon sizes relatively unimportant.

- Reversed order of search for icons by extension: rEFInd now searches
  for PNG files before ICNS files, rather than the other way around. This
  makes it possible to override a volume icon for rEFInd by giving it the
  name .VolumeIcon.png, even when a .VolumeIcon.icns file exists on the
  volume and is used by OS X.

- Fixed bug that caused .VolumeIcon.icns to take higher-than-intended
  precedence in icon setting for OS X.

- Chainloading to BIOS-mode boot loaders now works on UEFI-based PCs when
  rEFInd is built with GNU-EFI, not just when built with Tianocore.

0.8.0 (5/4/2014):
-----------------

- The "dont_scan_volumes" parameter now also works with legacy-boot
  volumes. Unlike with EFI volumes, where the option you pass must exactly
  match an entire volume name, when applied to legacy-boot volumes, it
  matches any part of the description that appears beneath the item when
  you select it in the rEFInd main menu.

- Can now boot in legacy mode from second (and probably later) hard disks!

- rEFInd now limits the length of the firmware name string shown in the
  system information screen to 65 characters. This is done because at least
  one EFI presents a longer string by default, and this causes the entire
  information display to come up empty on 800x600 displays.

- rEFInd now uses the partition's name (as stored in the GPT data
  structures) as a fallback for the filesystem's name if the latter can't
  be found. Exceptions are if the partition name is one of three generic
  names used by GPT fdisk: "Microsoft basic data", "Linux filesystem", or
  "Apple HFS/HFS+". These are ignored in favor of the descriptive fallback
  (e.g., "20 GiB Btrfs volume")

- It's now possible to specify a volume by partition GUID number in a
  manual boot stanza. This should be more reliable (albeit also more
  awkward) than using a filesystem number (such as fs0: or fs1:).

- Fixed memory-allocation bug that could cause error message displays,
  and possibly hangs, when re-scanning boot loaders.

0.7.9 (4/20/2014):
------------------

- Attempt to fix rEFInd perpetually re-scanning after ejecting a disc on
  some Macs.

- Added check to remove redundant (or non-functional if Secure Boot is
  active) kernel entries for Ubuntu, which is now including two versions of
  kernels, one signed and the other unsigned.

- Fixed bug in install.sh that could cause it to display error messages
  if the dmraid utility was not installed.

- The HFS+ driver now reports a correct volume name.

- Fixed some EFI filesystem driver bugs that could cause lockups under
  some circumstances. These bugs could affect any of the filesystem
  drivers.

- Added "gdisk" option to the "showtools" configuration file token. When
  active, this adds gdisk.efi or gdisk_{arch}.efi, if present in the
  EFI\tools directory, to the tools row.

- Fixed mistaken identification of the MOK utility as the "MOK utility
  utility."


0.7.8 (3/9/2014):
-----------------

- Added "debian" directory to source, which facilitates creation of Debian
  packages. Packages built in this way are built with GNU-EFI and don't run
  any post-installation script, so although the rEFInd binaries are on the
  hard disk, they aren't installed to be bootable; you must manually run
  install.sh. Also, at least on Ubuntu, the Make.common file's /usr/lib64
  references must be changed to /usr/lib. This is more of a proof of
  concept and a "leg up" for distribution maintainers than anything else.

- Two new options, big_icon_size and small_icon_size, set the size of
  the first-row OS icons and of the second-row tool icons, respectively.
  The big_icon_size option also indirectly sets the size of disk-type
  badges; they're 1/4 the size of the big icons. Default values are 128 and
  48, respectively, to match the actual icon files provided with rEFInd. If
  the icon you're using is of a different size than you've specified,
  rEFInd scales it. For best quality, you should both provide icons drawn
  to the right size and set the icon sizes in refind.conf.

- rEFInd now automatically scales icons to fit the standard icon sizes.
  This won't have any effect with the icons that come with rEFInd, but it
  can help if you want to use another icon, since you needn't scale it in a
  graphics program before using it. Note that rEFInd uses bitmap icons, so
  scaling by a huge amount (say, a 16x16 icon to fit the standard 128x128
  OS icon) is not likely to look good.

- Added new option, banner_scale, that tells rEFInd how to handle banners:
  Set to "noscale" (the default), banners are not scaled, although they'll
  be cropped if they're too big for the display. This is the same as the
  behavior in previous versions. Set to "fillscreen", rEFInd now scales the
  banner image (larger or smaller) to fill the display.

- Adjusted the post-installation script in refind.spec (used to generate
  RPMs, and therefore also indirectly Debian packages) to search for
  existing shim program files under the filesnames shim.efi and shimx64.efi
  rather than just shim.efi. Ubuntu uses shimx64.efi, so Debian packages
  were failing to detect Ubuntu's shim in previous versions. (Note,
  however, that Ubuntu's early shim 0.1 is unsuitable for use with rEFInd
  The newer 0.4 version that's in the repositories now should work fine;
  it's only when installing on an older system that's NOT been updated that
  problems might arise.

0.7.7 (1/3/2014):
-----------------

- Can now specify complete paths, optionally including volumes, in
  dont_scan_files.

- Added shimx64.efi to the default dont_scan_files list.

- Added windows_recovery_files token, to specify what program(s) launch a
  Windows recovery utility; and the "windows_recovery" option to
  "showtools," to control whether or not to display the Windows recovery
  utility on the second row of icons.

- The use_graphics_for, also_scan_dirs, dont_scan_dirs, dont_scan_files,
  and scan_driver_dirs tokens in refind.conf now support "+" as the first
  option, which causes the remaining options to be added to the default
  value rather than replacing that value. (This has no practical effect for
  scan_driver_dirs, though, since it has a null default value.)

- Added support for specifying the configuration file at program launch,
  via the "-c" parameter, as in "refind_x64.efi -c foo.conf" to use the
  foo.conf file as the main configuration file.

- Scans of ext2/3/4fs and ReiserFS partitions now omit partitions with
  duplicate filesystem UUIDs. These are likely parts of RAID arrays and so
  would have the same boot loaders or kernels as the first one with a given
  UUID.

- Added feature in install.sh: Script now tries to locate and mount an ESP
  in Linux, if it's currently unmounted.

- Fixed bug in mkrlconf.sh and install.sh that caused a stray line break
  and PARTUUID= specification to appear in generated refind_linux.conf file
  under some circumstances.

0.7.6 (12/15/2013):
-------------------

- Added support for multiple "default_selection" targets. These MUST be
  comma-separated AND enclosed in quotes, as in:
  default_selection "fred,ginger"
  This example will launch "fred" by default if it's available; and if
  it's not, rEFInd will attempt to launch "ginger" as the default.

- Added support for time-sensitive "default_selection" setting. This token
  may now have either one or three options. If one, it's interpreted as it
  has been in the past, as setting a default that's independent of times.
  If you follow this default by two times, however, those are interpreted
  as the start and end times (in 24-hour format) for a default setting. For
  instance, "default_selection foo 8:00 17:00" causes foo to be the default
  from 8:00 (AM) to 17:00 (aka 5:00 PM). You can include multiple
  "default_selection" lines to set different defaults for a variety of
  times. If they're in conflict, the last one takes precedence. Note that
  times are hardware clock's native value, which may be local time or UTC,
  depending on your computer.

- Added support for a blank-screen startup: Set "screensaver -1" and the
  screen saver will be initialized when rEFInd starts. If you set a low
  "timeout" value, the result will be a boot straight to the default OS
  unless you hit a key soon after rEFInd starts. Once you hit a key, the
  screensaver will be disabled.

- Added --ownhfs {target} option to install.sh. This option causes rEFInd
  to install to an HFS+ partition in a way that's more consistent with the
  way the Mac's native boot loader is installed. Note that you should NOT
  install to an already-bootable partition with this option, since it will
  overwrite the existing boot loader, which would render OS X unbootable.

0.7.5 (11/10/2013):
-------------------

- Fixed bug that caused unbootable exFAT partitions to show up as
  bootable on Macs with BIOS/CSM/legacy boot options enabled.

- Fixed bug in install.sh that caused installs to the ESP on recent
  versions of OS X to fail.

- Fixed bug that caused rEFInd to hang on some Macs when multiple EFI
  drivers were present.

- Fixed bug that caused clear to default gray screen when launching OSes
  with 'use_graphics_for' enabled, even when the rEFInd background is not
  gray. Now rEFInd clears to the same background color used in its menu.
  When launching OS X, though, the OS X boot loader will itself clear to
  gray a second or so later; and when launching Linux, it will clear to
  black a second or so later.

0.7.4.1 (8/25/2013):
--------------------

- My initial 0.7.4 release broke legacy-boot ability on Macs, so I quickly
  released this version using the original 0.7.4 filenames to fix the
  problem.

0.7.4 (8/25/2013):
------------------

- Fixed options passing to loader to include loader's filename as the first
  option. This omission had no effect on most boot loaders, but caused
  VMware's mboot64.efi to fail.

- Added support for memtest86 as second-row option. Program must be
  stored in EFI/tools, EFI/tools/memtest, EFI/tools/memtest86, EFI/memtest,
  or EFI/memtest86; and must use the name memtest86.efi, memtest86_x64.efi,
  memtest86x64.efi, or bootx64.efi (changing "x64" to "ia32" on IA-32
  systems). The memtest86 program is scanned for when the "showtools"
  option includes the "memtest" or "memtest86" token, which it does by
  default.

- Added space to end of "Boot %s from %s" string; enables adding a space
  to the end of the "default_selection" item (in quotes) to set a default
  that matches a volume name that's identical to another one except for
  extra characters at the end of the non-wanted volume's name.

- Fixed bug that could cause rEFInd to hang when launching boot loaders
  under some conditions. (Launching from Firewire drives on Macs is the
  known case, but there may be others.)

0.7.3 (8/7/2013):
-----------------

- Fixed bug that caused missing media-type badges on BIOS-mode boot
  loaders on Macs.

- Fixed bug that caused failure when launching BIOS-mode OSes on Macs.

0.7.2 (8/6/2013):
-----------------

- Fixed bug that caused display glitches in the final entry on the first
  row of icons if the second row of icons was empty.

- Fixed bug that could cause incorrect scanning or even a rEFInd crash when
  using volume specification in also_scan_dirs token.

- Added protection against loading invalid drivers and other EFI programs.
  (Some EFIs crash when attempting to load such drivers and programs.)

- Added PreLoader.efi and shim-fedora.efi to default dont_scan_files list;
  it's now "shim.efi, shim-fedora.efi, PreLoader.efi, TextMode.efi,
  ebounce.efi, GraphicsConsole.efi, MokManager.efi, HashTool.efi,
  HashTool-signed.efi".

- Added icon for Funtoo Linux.

- Fixed reading of volume badges from user-specified icons directory, which
  was broken.

- Fixed handling of /.VolumeBadge.icns (or /.VolumeBadge.png) files, which
  was broken.

0.7.1 (7/8/2013):
-----------------

- Fixed build problem with recent development versions of EDK2.

- Added scan for Boot Repair's backup of the Windows boot loader
  (bkpbootmgfw.efi). If found, give separate entries for it and for
  bootmgfw.efi, each with its own descriptive text label.

- Fixed also_scan_dirs; used to have bug that caused it to ignore
  volume specification, if present.

- Fixed bug in driver cache that caused Btrfs driver to hang sometimes.

0.7.0 (6/27/2013):
------------------

- Added Btrfs signature to rEFInd, so that it can identify the filesystem
  type for volumes that lack labels.

- Changed some critical filesystem driver pointers from 32-bit to 64-bit.
  This *SHOULD* enable use of over-2TiB filesystems (for those filesystems
  that support such large volumes). This capability is largely untested,
  though.

- Added a cache to the filesystem driver core, and therefore to all the
  filesystem drivers. This cache greatly improves performance in
  VirtualBox, and offers modest performance improvements on a few "real"
  computers. The most dramatic improvement is on ext2/3fs under VirtualBox:
  Loading a kernel and initrd used to take ~200 seconds on my system, but
  now takes ~3 seconds! On most "real" hardware, the improvement is much
  less dramatic -- an improvement of a second or less, presumably because
  of cacheing within the EFI or on the hard disk itself.

- Filter boot loaders based on a test of their validity; keeps out Linux
  kernels without EFI stub loader code, loaders for the wrong architecture,
  non-EFI loaders, etc.

- New Btrfs driver, contributed by Samuel Liao based on GRUB 2.00 Btrfs
  code.

0.6.12 (6/18/2013):
-------------------

- Changed the 64-bit EFI shell included in the CD-R and USB flash drive
  images to a version 2 shell that should support the "bcfg" command.

- Added support for PreBootloader to refind.spec's built-in installation
  script.

- Added support for the Linux Foundation's PreLoader to install.sh. It's
  treated just like shim, including using the --shim option (or, now,
  --preloader); but it searches for and copies HashTool.efi rather than
  MokManager.efi, and filenames are adjusted appropriately.

- Added code to determine Linux root filesystem from /etc/fstab file, if
  it's on the same partition as the kernel and if the refind_linux.conf
  file is not available. This enables rEFInd to boot Linux without any
  rEFInd-specific configuration files on some (but not all) systems.

0.6.11 (5/13/2013):
-------------------

- New feature: rEFInd now ignores symbolic links to files on filesystems
  that support them. This prevents the "vmlinuz" symbolic link that some
  distributions create in the root directory from appearing in the loader
  list. Note that this does NOT affect symbolic links to directories.

- Added icons for Lubuntu and Kubuntu.

- Improved the install.sh script so that it does a better job dealing with
  directory names that contain spaces.

- rEFInd now tries to guess the Linux distribution type based on the kernel
  filename (Fedora and RHEL only) or the "ID" or "NAME" variables in
  /etc/os-release on the kernel's partition. None of these is guaranteed to
  work. A fallback of the Tux penguin icon remains in place in case rEFInd
  can't find anything substantive enough for a guess.

- Added "EFI\opensuse" to the locations searched for MOK utilities, since
  OpenSUSE now uses that name.

- Renamed "Reboot to Firmware User Interface" to "Reboot to Computer Setup
  Utility" in menu.

- Fixed bug in gptsync that caused it to hang if the disk had too few GPT
  partitions to fill the MBR.

0.6.10 (5/5/2013):
------------------

- Added support for "screensaver" token. If set to a positive integer, this
  causes the screen to blank after the specified number of seconds of
  inactivity. Pressing most keys (unfortunately NOT including Shift, Alt,
  or Ctrl) will restore the display and restart the screen saver timeout.

- Added icon for ChromeOS (os_chrome.icns in the icons subdirectory).
  ChromeBooks reportedly boots using the fallback filename, but if a user
  wants to install rEFInd on a ChromeBook, renaming the original EFI/BOOT
  directory to EFI/chrome and then installing rEFInd in the fallback
  filename will bring up this new icon for ChromeOS.

- Added new option to reboot the computer into the firmware's user
  interface. This option is active by default, or can be set via the
  "firmware" option to the "showtools" token in refind.conf. It works
  on only some computers, though; older computers lack this feature, and
  when rEFInd is told to use this feature on such computers, the directive
  is quietly ignored.

- Upgraded LodePNG library from version 20121216 to 20130415 and
  restructured rEFInd-specific modifications to simplify future upgrades.

- Replaced hexadecimal error code with description if an error is
  encountered when saving a screen shot.

- Enable multiple screen shots: Rather than naming all screen shots
  "screenshot.bmp", the name is now "screenshot_###.bmp", where "###" is a
  sequence number, starting with "001".

0.6.9 (4/25/2013):
------------------

- Modified default banner to include the new rEFInd icon, provided by Erik
  Kemperman.

- Worked around a suspected firmware bug that caused rEFInd 0.6.6 to 0.6.8
  to hang at startup on some systems (DUET and some Macs).

- Modified rEFInd to search for gptsync under the names gptsync.efi and
  gptsync_{arch}.efi, where {arch} is ia32 or x64. (Previous versions
  searched only for gptsync.efi.)

- Added gptsync program from rEFIt project, but with some changes to
  improve flexibility and make it less likely that UEFI users will
  accidentally trash their systems.

- Changed timeout code so that the timeout continues if the keyboard is
  disconnected. This can help in booting a headless server or a system with
  a bluetooth or other keyboard that's not recognized by the EFI.

0.6.8 (3/18/2013):
------------------

- Added workaround for presumed EFI bug that was causing "Invalid
  Parameter" errors when scanning for boot loaders on some computers.

- Added search for an EFI shell called shell.efi in the root directory
  (previously this name was only accepted in EFI\tools).

- Fixed bug in install.sh that caused it to fail on some systems (Fedora
  18, for instance) because of a problem identifying the ESP.

- Fixed bug that caused icons named after boot loaders to not be used.

0.6.7 (2/3/2013):
-----------------

- Added a more explicit error message summarizing options when a launch of
  a program results in a Secure Boot failure.

- Changed MOK tool detection to scan all volumes, not just the rEFInd
  home volume. This is desirable because the Linux Foundation's HashTool
  can only scan its own volume, making it desirable to place copies of this
  program on every volume that holds EFI boot loader binaries.

- Added support for launching the Linux Foundation HashTool as a means of
  managing MOKs (or MOK hashes, at any rate).

- Fixed bug that caused rEFInd to present an entry for itself as a
  Microsoft OS if it was launched as EFI/Microsoft/Boot/bootmgfw.efi.

- Fixed bug that caused dont_scan_volumes option to be added to
  also_scan_dirs list.

- Fixed dont_scan_volumes so that it works with OS X boot loaders.

- Fixed broken mixing of PNG and ICNS icons when using a user-specified
  icons directory -- previously, an ICNS file in the default directory
  would override a PNG file in the user-specified directory.

0.6.6 (1/26/2013):
------------------

- rEFInd now ignores the fallback boot loader (EFI/BOOT/bootx64.efi or
  EFI/BOOT/bootia32.efi) if it's identical to another boot loader on
  the same volume. This is intended to help unclutter the display on
  systems that run Windows, since Windows tends to duplicate its own boot
  loader under the fallback name.

- Added new "font" token to refind.conf, which enables specifying a font in
  the form of a PNG file. This file must contain monospace glyphs for the
  95 characters from ASCII 32 to 126 (space through tilde), inclusive, plus
  a glyph to be displayed for characters outside of this range, for a total
  of 96 glyphs.

- Replaced the old font (inherited from rEFInd) with an anti-aliased
  version of Luxi Mono Regular 14 point.

- Fixed bug that caused rEFInd to ignore manual boot stanzas in files
  included via the "include" token in refind.conf.

- Fixed bug that caused ASSERT error on some systems (and conceivably a
  crash on startup on some) when default_selection line in refind.conf was
  commented out or empty.

- Fixed bug that caused "Binary is whitelisted" message to persist on
  screen after loading MOK-signed drivers in Secure Boot mode.

- Fixed bug that caused rEFInd to ignore the "icon" token in refind.conf
  manual boot stanzas.

- Fixed bug in install.sh that caused the script to fail to update
  drivers when rEFInd was installed in EFI/BOOT/.

0.6.5 (1/16/2013):
------------------

- Improved text color support: rEFInd now uses black text against light
  backgrounds and white text against dark backgrounds.

- Added support for PNGs as banners, icons, and selectors.

- Added icon for ALT Linux.

- Added "safemode" option to "hideui" token, to hide option to boot into
  safe mode for OS X ("-v -x" option to boot.efi).

- Added icon for Haiku (os_haiku.icns).

- Enable transparency of icons & main-menu text when the banner icon is
  sized to cover these areas.

- Fixed bug that could cause rEFInd to crash if fed a banner image that's
  too big. Note that "too big" can be substantially smaller than the screen
  resolution!

0.6.4 (1/8/2013):
-----------------

- Revised install.sh to copy ext2fs driver, rather than ext4fs driver, for
  ext2/3 filesystems. This can help keep non-functional entries from links
  from /vmlinuz to /boot/vmlinuz out of the menu if the system uses ext4fs
  on root and ext2fs or ext3fs on /boot.

- Fixed a couple of memory management bugs that cause rEFInd to hang at
  startup on some systems.

0.6.3 (1/6/2013):
-----------------

- Added the ability to specify a volume name or number in the
  "dont_scan_dirs" and "also_scan_dirs" tokens.

- Fixed a bug that caused removable EFI media to not appear in scan lists
  if rEFInd was installed as EFI/BOOT/boot{arch}.efi on a hard disk.

- Modified ISO-9660 driver so that it can handle discs with other than
  2048-byte sectors. This makes it useful for reading "hybrid ISO" images
  burned to USB flash disks.

- New mvrefind.sh script to move a rEFInd installation between a standard
  location (typically EFI/refind) and one of the fallback locations
  (EFI/BOOT or EFI/Microsoft/Boot). It can also do more exotic locations.

- The install.sh script now installs to EFI/BOOT/bootx64.efi or
  EFI/Microsoft/Boot/bootmgfw.efi if it's run in BIOS mode. This is
  intended to give some chance of producing a bootable installation should
  a user accidentally install Linux in EFI mode and then install rEFInd
  from that installation.

- The install.sh script now tries to find an existing rEFInd installation
  and upgrade it, even if it's in EFI/BOOT or EFI/Microsoft/Boot rather
  than in EFI/refind.

- New "--yes" option to install.sh to help with unattended or automated
  installations (as from an RPM or Debian package).

0.6.2 (12/30/2012):
-------------------

- Inclusion of a sample refind.spec file for the benefit of RPM
  distribution maintainers who might want to include rEFInd. It's a bit
  rough, but it gets you a good chunk of the way there....

- The EFI filesystem drivers can now be built with the GNU-EFI toolkit as
  well as with the TianoCore EDK2. See the BUILDING.txt file for details on
  how to build them with either toolkit. This improvement doesn't affect
  users of my binary packages, but it should make it easier for Linux
  distributions to adopt rEFInd into their package systems.

- Tweaked refind.inf file for better build results using "native" TianoCore
  EDK2 build process (vs. the Makefile-based build process that I use under
  Linux). This won't affect those who use my binary builds or build under
  Linux with the "make" command.

- Fixed bug that prevented Secure Boot launches from working when rEFInd
  was built with GNU-EFI rather than the TianoCore EDK2.

- Substantial reworking of Secure Boot code, based on James Bottomley's
  PreLoader program. This new code eliminates the limitation of launching
  just one driver in Secure Boot mode and is likely to be more reliable
  with future or obscure boot loaders. It should also work with non-x86-64
  systems, although this relies on a platform-specific shim program, which
  to date exists only for x86-64. The basic features are the same as before
  -- rEFInd relies on shim for authentication functions and will launch
  programs that are signed by Secure Boot keys, shim keys, or MOKs.

- Altered default for "textmode" option (when it's commented out) to not
  adjust the text mode at all. (Prior versions set it to mode 0 by
  default.)

0.6.1 (12/21/2012):
-------------------

- Added "--root" option to install.sh, to enable installation of rEFInd
  to something other than the currently-running OS. This is intended for
  use on emergency discs.

- Thanks to Stefan Agner, the ext4fs driver now supports the "meta_bg"
  filesystem feature, which distributes metadata throughout the disk. This
  feature isn't used by default, but can be set at filesystem creation time
  by passing the "-O meta_bg,^resize_inode" option to mke2fs. (Using
  "^resize_inode" is necessary because meta_bg is incompatible with
  resize_inode, which IS used by default.) This feature can be used on
  ext3fs and ext2fs as well as on ext4fs, so the ext4fs driver can now
  handle some ext3fs and ext2fs partitions that the ext2fs driver can't
  handle.

- Fixed some screen resolution-setting bugs.

- Added the "words" that make up a filesystem's label (delimited by spaces,
  dashes, or underscores) to the list of bases used to search for OS icons.
  For instance, if the filesystem's label is "Arch", rEFInd searches for
  os_Arch.icns; if it's "Fedora 17", it searches for os_Fedora.icns and
  os_17.icns; and if it's "NEW_GENTOO", it searches for os_NEW.icns and
  os_GENTOO.icns.

- Refined hints displays to be more context-sensitive, particularly in text
  mode.

- Instead of displaying a blank filesystem label when a filesystem has
  none, rEFInd now displays the size and/or type of the filesystem, as in
  "boot EFI\foo\bar.efi from 200 MiB ext3 volume" rather than "boot
  EFI\foo\bar.efi from".

- Fixed a bug that caused the screen to clear after displaying an error
  message but before displaying the "Hit any key to continue" message when
  a boot loader launch failed.

0.6.0 (12/16/2012):
-------------------

- Fixed a memory allocation bug that could cause a program crash when
  specifying certain values with the "also_scan_dirs", "dont_scan_volumes",
  "dont_scan_dirs", "dont_scan_files", and "scan_driver_dirs" refind.conf
  options.

- Modified Linux kernel initrd-finding code so that if an initrd is
  specified in refind_linux.conf, rEFInd will not add any initrd it finds.
  This enables an override of the default initrd, and is likely to be
  particularly helpful to Arch Linux users.

- Added ext4fs driver!

- Made "boot" the default value for "also_scan_dirs".

- Added identifying screen header to line editor.

- Fixed bug that caused rEFInd's display to be mis-sized upon return
  from a program that set the resolution itself.

- Adjusted "resolution" refind.conf parameter so that it can accept EITHER
  a resolution as width and height OR a single digit as a UEFI mode number
  (which is system-specific). This is done because some systems present the
  same mode twice in their mode lists, perhaps varying in refresh rate,
  monitor output, or some other salient characteristics; specifying the
  mode number enables selecting the higher-numbered mode, whereas using
  horizontal and vertical resolution values selects the lowest-numbered
  mode.

- Added "textmode" refind.conf parameter to set the text mode used in
  text-only displays, and for the line editor and boot-time handoff
  display even in graphics mode.

- Fixed bug that caused tools (shell, etc.) to launch when they were
  highlighted and F2 or Insert was pressed.

- Added "editor" option to the "hideui" token in refind.conf, which
  disables the boot options editor.

- Added hints text to rEFInd main menu and sub-menus. This can be disabled
  by setting the new "hints" option to the "hideui" token in refind.conf.

- Added "boot with minimal options" entry to refind_linux.conf file
  generated by install.sh. This entry boots without the options extracted
  from the /etc/default/grub file.

- Added keys subdirectory to main distribution, to hold public Secure
  Boot/shim keys from known sources.

- Changed install.sh --drivers option to --alldrivers, added new
  --nodrivers option, and made the default on Linux to install the one
  driver that's used on /boot (or the root filesystem if /boot isn't a
  separate partition). Of course, this won't install a non-existent driver,
  and it also won't work properly if run from an emergency disk unless you
  mount a separate /boot partition at that location.

- Fixed bug in install.sh that prevented creation of refind_linux.conf file
  on Linux systems.

0.5.1.1 (12/12/2012):
---------------------

- Fixed bug in install.sh that prevented it from working on OS X.

0.5.1 (12/11/2012):
-------------------

- Added support for "0" options to "textonly" and "scan_all_linux_kernels"
  to reverse the usual meaning of these tokens. This is useful for
  including these options in a secondary configuration file called with the
  new "include" token to override a setting set in the main file.

- Added "include" token for refind.conf, to enable including a secondary
  configuration file from a primary one.

- Modified install.sh so that it creates a simple refind_linux.conf file in
  /boot, if that file doesn't already exist and if install.sh is run from
  Linux. If that directory happens to be on a FAT, HFS+, ext2fs, ext3fs, or
  ReiserFS volume, and if the necessary drivers are installed, the result
  is that rEFInd will detect the Linux installation with no further
  configuration on many systems. (Some may still require tweaking of kernel
  options, though; for instance, adding "dolvm" on Gentoo systems that use
  LVM.)

- Added --shim and --localkeys options to install.sh to help simplify setup
  on systems with Secure Boot active.

- Fixed (maybe) bug that caused resolution options to not be displayed on
  recent Macs with GOP graphics when specifying an invalid resolution in
  refind.conf.

- Fixed bug that caused some programs (EFI shells, in particular) to hang
  when launching on some systems (DUET, in particular).

- Implemented a fix to enable ELILO to launch with Secure Boot active.
  This fix might help with some other boot loaders in Secure Boot mode,
  too, but I don't know of any specifics.

0.5.0 (12/6/2012):
------------------

- Added the ability to include quote marks ('"') in refind.conf and
  refind_linux.conf tokens by doubling them up, as in:
  "ro root=/dev/sda4 some_value=""this is it"""
  This example results in the following string being passed as an
  option:
  ro root=/dev/sda4 some_value="this is it"

- Changed refind.conf-sample to uncomment the scan_all_linux_kernels
  option by default. If this option is deleted or commented out, the
  program default remains to not scan all Linux kernels; but with
  increasing numbers of distributions shipping with kernels that include
  EFI stub loader support, setting the configuration file default to scan
  for them makes sense.

- Modified the "resolution" token so that it affects text mode as well
  as graphics mode. On my systems, though, the actual text area is still
  restricted to an 80x25 area. (This seems to be a firmware limitation; my
  EFI shells are also so limited.)

- Fixed a bug that caused the options line editor to blank out lines that
  were not actually edited.

- Added support for using Matthew Garrett's Shim program and its Machine
  Owner Keys (MOKs) to extend Secure Boot capabilities. If rEFInd is
  launched from Shim on a computer with Secure Boot active, rEFInd will
  launch programs signed with either a standard UEFI Secure Boot key or a
  MOK. For the moment, this feature works only on x86-64 systems.

- Added new "dont_scan_files" (aka "don't_scan_files") token for
  refind.conf. The effect is similar to dont_scan_dirs, but it creates a
  blacklist of filenames within directories rather than directory names.
  I'm initially using it to place shim.efi and MokManager.efi in the
  blacklist to keep these programs out of the OS list. (MokManager.efi is
  scanned separately as a tool; see below.) I've moved checks for
  ebounce.efi, GraphicsConsole.efi, and TextMode.efi to this list. (These
  three had previously been blacklisted by hard-coding in ScanLoaderDir().)

- Added the directory from which rEFInd launched to dont_scan_dirs. This
  works around a bug in which rEFInd would show itself as a bogus Windows
  entry if it's installed as EFI/Microsoft/boot/bootmgfw.efi.

- Added support for launching MokManager.efi for managing the Machine Owner
  Keys (MOKs) maintained by the shim boot loader developed by Fedora and
  SUSE. This program is scanned and presented as a second-row tool.

- Added support for Apple's Recovery HD partition: If it's detected, a new
  icon appears on the second row. This icon can be removed by explicitly
  setting the "showtools" option in refind.conf and excluding the
  "apple_recovery" option from that line.

- Fixed bug that caused text-mode ("textonly" refind.conf option enabled)
  menu entries to be right-aligned rather than left-aligned when rEFInd was
  compiled with the TianoCore EDK2.

- Added "--usedefault {devicename}" and "--drivers" options to the
  install.sh script and changed the "esp" option to "--esp". 

0.4.7 (11/6/2012):
------------------

- Added an icon for gummiboot.

- Added a boot option editor: Pressing the Insert or F2 key from a boot
  tag's options menu opens a simple text-mode line editor on which the boot
  options may be edited for a one-time boot with altered options.

- Modified the "scan_delay" feature to delay and then perform a re-scan,
  which may work better than the first attempt at this feature (which I'm
  told isn't working as planned).

- Modified rEFInd to add a space after the command-line options only when
  launching Mac OS X. On some early Macs, the extra space (which had been
  present by default, as a carryover from rEFIt) causes problems when
  booting Linux kernels from FAT partitions.

0.4.6 (10/6/2012):
------------------

- Fixed some minor memory management issues.

- Added new "scan_delay" feature to impose a delay before scanning
  for disks.

- Changed default "scanfor" option from internal-external-optical to either
  internal-external-optical-manual (for non-Macs) or
  internal-hdbios-external-biosexternal-optical-cd-manual (for Macs). I've
  done this for two reasons:
  - Many Mac users have been confused by the fact that rEFInd needs
    reconfiguration to detect Windows (or Linux installed in BIOS mode),
    since rEFIt scans BIOS devices by default. Adding the BIOS options as
    default for them should help them.
  - Adding the "manual" option enables users to simply add manual boot
    stanzas and have them work, which is more intuitive. Adding the
    "manual" option will have no effect unless manual stanzas are created
    or uncommented, so this part of the change won't affect users' working
    default configurations.

- Added new legacy (BIOS) boot support for UEFI-based PCs.

0.4.5 (8/12/2012):
------------------

- Fixed bug that caused a failure to boot BIOS-based OSes on Macs.

- Fixed bug in install.sh that caused it to fail to detect rEFItBlesser.

0.4.4 (6/23/2012):
------------------

- Fixed bug that caused filesystem labels to be corrupted by rEFInd on
  32-bit systems.

- Fixed bug that caused filesystem labels to be truncated in the drivers
  on 32-bit systems.

- Fixed bug in use_graphics_for option parsing that caused most options
  to set graphics mode for OS X and/or Linux but not other boot
  loaders/OSes.

- Tweaked install script to better isolate the ESP under OS X.

0.4.3 (6/21/2012):
------------------

- rEFInd now supports compilation using the TianoCore UDK2010/EDK2
  development kit in addition to GNU-EFI.

- Added new "use_graphics_for" option to control which OSes to boot in
  graphics mode. (This effect lasts for a fraction of a second on most
  systems, since the boot loader that rEFInd launches is likely to set
  graphics or text mode itself.)

- Graphics-mode booting now clears the screen to the current rEFInd
  background color (rather than black) and does NOT display boot messages.
  The intent is for a smoother transition when booting OS X, or perhaps
  other OSes that don't display boot loader messages. In practice, this
  effect will be tiny for many OSes, since the boot loader generally clears
  the screen within a fraction of a second of being launched; but the
  "flicker" of a rEFInd message in that time can sometimes be distracting.

- Filesystem drivers now work on EFI 1.x systems, such as Macs.

- Removed "linux.conf" as a valid alternative name for "refind_linux.conf"
  for holding Linux kernel options. The kernel developers plan to use
  "linux.conf" themselves.

0.4.2 (6/3/2012):
-----------------

- Added a message to install.sh when run on Macs to remind users to update
  the "scanfor" line in refind.conf if they need to boot BIOS-based OSes
  via rEFInd.

- Modified install.sh script to be smarter about running efibootmgr on
  Linux. It now uses the whole path to the rEFInd binary as a key to
  determine whether an existing entry exists, rather than just the filename
  portion. If an entry exists and is the first entry in the boot order, the
  script does nothing to the NVRAM entries. If such an entry exists but is
  not the default, the script deletes that entry and creates a new one
  (implicitly making it the first in the boot order). If such an entry does
  not exist, the script creates a new one (again, making it the first in
  the boot order).

- Added "dont_scan_dirs" configuration file option, which adds directories
  to a "blacklist" of directories that are NOT scanned for boot loaders.

0.4.1 (5/25/2012):
------------------

- Added "scanning for new boot loaders" message to the re-scan function
  (hitting Esc at the main menu). It usually flashes up too quickly to
  be of importance, but if the scan function takes a while because of
  access to a CD that must be spun up, it should make it clear that the
  system hasn't hung.

- Modified install.sh script to detect rEFItBlesser on Macs, and if
  present, to ask the user if it should be removed.

- Cleaned up the Make.common file for the filesystem drivers.

- Changed HFS+ driver to return volume label of "HFS+ volume" rather than
  an empty label. (The driver doesn't currently read the real volume
  label.)

- Fixed bug that could cause rEFInd to appear in its own menu after
  running a shell and then re-scanning for boot loaders.

0.4.0 (5/20/2012):
------------------

- Inclusion of drivers for ISO-9660, HFS+, ReiserFS, and ext2fs. Most of
  these drivers originated with rEFIt, although the HFS+ driver seems to
  have come from Oracle's VirtualBox, with some files from Apple. I hadn't
  included these drivers previously because the build process proved
  challenging. As it is, they don't work on my Mac Mini, I suspect because
  the build process with the UDK2010 development kit may not work with the
  EFI 1.x that Apple uses.

- Addition of support for drivers in the "drivers_{arch}" subdirectory of
  the main rEFInd binary directory (e.g., "drivers_x64" or "drivers_ia32").
  Drivers may continue to be placed in the "drivers" subdirectory.

- Added new feature to eject CDs (and other removable media): Press F12 to
  eject all such media. This function works only on some Macs, though (it
  relies on an Apple-specific EFI extension, and this extension isn't even
  implemented on all Macs, much less on UEFI-based PCs).

- Fixed a problem that could cause GRUB 2 to fail to read its configuration
  file when launched from rEFInd.

0.3.5 (5/15/2012):
------------------

- Removed the GRUB 2 detection "reciped" added with 0.3.2, since I've
  received reports that it's not working as intended.

- Added re-scan feature: Press the Esc key to have rEFInd re-read its 
  configuration file, tell the EFI to scan for new filesystems, and re-scan
  those filesystems for boot loaders. The main purpose is to enable
  scanning a new removable medium that you insert after launching rEFInd;
  however, it can also be used to immediately implement changes to the
  configuration file or new drivers you load from an EFI shell.

- Fixed a bug that could cause the scroll-right arrow to be replaced by the
  scroll-left arrow under some circumstances.

0.3.4 (5/9/2012):
-----------------

- Added new configuration file option: "icons_dir", which sets the name
  of the subdirectory in which icons are found. See the documentation or
  sample configuration file for a full description.

- Modified Makefile to generate rEFInd binary that includes architecture
  code -- refind_ia32.efi or refind_x64.efi, rather than the generic
  refind.efi. This is done mainly to help the install.sh script. The
  program can be named anything you like on the disk. (The generic name
  refind.efi is used on unknown architectures.)

- Improved install.sh script: Fixed bug on OS X 10.7 and enable it to be
  used after building from source code (or via new "make install" Makefile
  target).

- Improved screen redraws to produce less flicker when moving among the
  second-row tags or to the last tag on the first row.

0.3.3 (5/6/2012):
-----------------

- Improved menu navigation:
  - In graphics mode, left & right arrow keys move left & right, while up &
    down arrows move between rows.
  - Page Up and Page Down now move through chunks of visible tags (in both
    text & graphics modes), jumping from one row to another only when at
    the edge of the row. In text mode, the "rows" are broken down as in
    graphics mode, but they aren't visibly distinguished on the screen.

- Improved text-mode use: rEFInd now displays the proper number of entries
  when first started in text mode and scrolling is done sensibly when too
  many entries exist to fit on the screen.

0.3.2 (5/4/2012):
-----------------

- Added the install.sh script to install rEFInd on Linux and Mac OS X
  systems. This script must be run as root (or via sudo). It requires
  no options, but on Mac OS X, passing it the "esp" option causes it
  to install rEFInd on the computer's ESP rather than the default of the
  currently OS X boot partition. (Under Linux, the default is to install to
  the ESP.) Note that there may be some unusual cases in which this script
  will fail to work.

- Does a better job of clearing the screen when launching OSes in text
  mode.

- Added detection "recipe" for GRUB 2's BIOS Boot Partition.

- Fixed bogus detection of ESPs created by Linux's mkdosfs utility or
  Windows as  bootable partitions when "scanfor" includes BIOS scanning
  options.


0.3.1 (4/27/2012):
------------------

- Fixed bug that caused spurious "Unsupported while scanning the root
  directory" messages under some conitions on Macs.

- Modified loader scanning code to sort boot loader entries within a
  directory by modification time, so that the most recently-modified loader
  is first among those in a given directory. Thus, if you specify a
  directory name (or volume name, for loaders stored in the root directory
  of a volume) as the default_selection, the most recent of those loaders
  will be the default. This is intended to help with Linux kernel
  maintenance when using the EFI stub loader; set up this way, the most
  recent kernel copied to your kernel directory will be the default,
  obviating the need to adjust the refind.conf file when adding a new
  kernel. If you want to change the default among those in the default
  directory, you can use "touch" to adjust the modification timestamp.

- Tweaked code to find loader-specific .icns file so that it finds files
  for Linux kernels without .efi extensions. In this case, files should be
  named the same as the kernels they match, but with .icns extensions. For
  instance, bzImage-3.3.2 should have an icon called bzImage-3.3.2.icns.
  (The old code would have looked for an icon called bzImage-3.3.icns.)

- Eliminated bogus OS loader tags for filenames that end in ".icns" when
  the scan_all_linux_kernels option is set.

0.3.0 (4/22/2012):
------------------

- I'm officially upgrading this project's status from "alpha" to "beta" and
  giving it a bump from 0.2.x to 0.3.0. This doesn't reflect any major
  milestone with this version; rather, it reflects my sense that rEFInd has
  been "out there" for a while, and although I've gotten bug reports,
  they've been minor and/or have been fixed. The program still has known
  bugs, but my impression is that it is, overall, usable by ordinary users.

- Added "resolution" option to refind.conf, which enables setting the video
  resolution. To use it, pass two numeric values, as in "resolution 1024
  768" to use a 1024x768 video mode. Note that not all modes are supported.
  If you specify a non-supported video mode on a UEFI system, a message
  appears listing the supported video modes and you must then press a key
  to continue, using the default video mode (usually 800x600).
  Unfortunately, I don't know the calls to get a list of supported video
  modes on older EFI 1.x systems (including Macs), so on Macs setting an
  incorrect video mode silently fails (you keep using the default mode).
  This makes changing your video mode a hit-or-miss proposition on Macs.
  CAUTION: It's possible to set a legal video mode that your monitor can't
  handle, in which case you'll get a blank display until you boot an OS
  that resets the video mode.

- Fixed (maybe) a bug that caused rEFInd to crash when returning from an
  EFI shell or other programs on Macs, particularly when rEFInd used
  graphical mode. I'm not 100% sure this bug is squashed because I still
  don't understand the cause and I only have one Mac for testing. See
  comments in the ReinitRefitLib() function in refit/lib.c for more
  details.

- Added new refind.conf option: scan_all_linux_kernels, which causes Linux
  kernels that lack ".efi" extensions to be included in scans for EFI boot
  loaders. This may help integration with Linux distributions that don't
  give their kernels such names by default. Beware, though: It can detect
  unwanted files, such as older non-stub-loader kernels or .icns files used
  to give kernels with .efi extensions custom icons.

- Improved EFI boot loader detection on boards with Gigabyte's Hybrid EFI,
  and perhaps other EFIs with a buggy StriCmp() function. Files with both
  ".efi" and ".EFI" extensions should now be detected as boot loaders.

- Fixed a bug that caused rEFInd to fail to scan for drivers if the
  filesystem driver didn't set a volume name (that is, if the relevant
  field was set to NULL rather than even an empty string). In such
  situations, rEFInd now reports the volume name as "Unknown".

0.2.7 (4/19/2012):
------------------

- After much trial and tribulation, I've overcome a GNU-EFI limitation and
  enabled rEFInd to load EFI drivers. This feature was present in the
  original build of rEFIt but was removed in the versions that could
  compile under Linux, but now it's back -- and still being compiled under
  Linux! To use it, you should place your drivers in a convenient directory
  on the ESP (or whatever partition you use to launch rEFInd) and add a
  "scan_driver_dirs" entry to refind.conf to tell rEFInd where to look. (As
  always, you should specify the driver directory relative to the root of
  the filesystem.) Note that you can't launch drivers from another
  filesystem; they must be on the same volume that holds rEFInd. Those who
  compile from source code should note that implementing this feature
  necessitated using a more recent version of the GNU-EFI library. I'm
  currently using version 3.0p, and version 3.0i does NOT work. I don't
  know where the change occurred, but you may need to upgrade your GNU-EFI
  installation.

- Fixed bug that caused rEFInd to show up in its own menu sometimes.

- Added new refind.conf token: also_scan_dirs. When scanning volumes for
  EFI boot loaders, rEFInd always scans the root directory and every
  subdirectory of the /EFI directory, but it doesn't recurse into these
  directories. The also_scan_dirs token adds more directories to the scan
  list. It defaults to "elilo,boot", but you can set it to any directory or
  directories you like.

0.2.6 (4/14/2012):
------------------

- Added "volume" keyword to configuration file's stanza options. This
  option changes the volume from which subsequent files (specified by
  "loader" and "icon") are loaded. You pass "volume" the name/label of the
  FILESYSTEM you want to use (not the GPT partition name), or a number
  followed by a colon (e.g., "1:"). The former should reliably identify a
  filesystem, assuming the name is unique. The latter assigns numbers based
  on the order in which they're scanned, which may not be as reliable but
  should work when a volume is unnamed.

- Fixed bug in 0.2.5 that caused failure of Linux initial RAM disk
  mapping on some (but not all) systems. Affected computers include at
  least some Intel motherboards, maybe others.

0.2.5 (4/9/2012):
-----------------

- Fixed bug that caused an inability to associate initial RAM disks with
  Linux kernels stored in a volume's root directory.

- Volume badges (that override default badges) are now stored in
  .VolumeBadge.icns. Although undocumented, rEFInd formerly loaded custom
  volume badges from .VolumeIcon.icns. This carryover from rEFIt was a
  confusing name, given the next (new) feature, so I've changed and
  documented the name....

- Added ability to set a default icon for a loader stored in the root
  directory of a volume: The icon is stored in .VolumeIcon.icns. This icon
  is also used for Mac OS X volumes booted from the standard location.

- Fixed bug that caused icons to drop back to generic icons when rEFInd
  was launched in certain ways (such as from an EFI shell in rEFInd's
  directory) on certain systems.

- Fixed bug that caused "unknown disable flag" to be shown (very briefly)
  instead of "unknown hideui flag" when an improper hideui flag was set.

0.2.4 (4/5/2012):
-----------------

- Created new refind.conf entry: "showtools". This entry takes options of
  "shell", "gptsync", "about", "exit", "reboot", and "shutdown". This
  option is in some respects an affirmative version of portions of the old
  "disable" and "hideui" options; however, it enables users to specify the
  order in which these options appear on the screen. Also, the "exit"
  option is new; it terminates the program. The effect is usually to return
  to whatever tool launched it or to launch a default OS; however, this is
  somewhat unpredictable. The default therefore omits the "exit" option, as
  well as "gptsync", which has always been dangerous (but necessary on most
  MacOS/Windows dual-boot setups on Macs). As part of this reconfiguration,
  I've eliminated the "rescue Linux" option, which always seemed pointless
  to me.

- Folded "disable" and "hideui" refind.conf entries into one ("disable"),
  and reduced the number of options to six: "banner", "label",
  "singleuser", "hwtest", "arrows", and "all". ("arrows" is new and
  disables the scroll arrows when a system has too many tags to display
  simultaneously.)

- Added max_tags option to the refind.conf file, enabling users to reduce
  the maximum number of OS loader tags that can be displayed at once.

- Updated rEFIt icon, based on the 128x128 volume label from the rEFIt CD
  image.

- Added x86 and x86-64 EFI shells to the CD image version of the binary,
  but NOT to the binary zip file. The logic is that the CD image is more
  likely to be used directly as an emergency disc and so may need this
  feature, even though the source isn't part of the rEFInd project. (The
  source is readily available from the TianoCore project.)

- EFI shells may now be stored at /shellx64.efi for x86-64 systems or at
  /shellia32.efi for x86 systems. The /EFI/tools/shell.efi name is also
  recognized; however, if both files are present, two EFI shell icons will
  appear on the main menu. The /efi/{refind-path/apps/shell.efi filename,
  which was never officially documented but worked as a carryover from
  rEFIt, is no longer valid.

0.2.3 (3/26/2012):
------------------

- Fixed (maybe) a bug that caused hangs when launching a second program
  after returning from a first. There are some weird system-to-system
  differences, though, and this fix causes (apparently harmless) error
  messages about "(re)opening our installation volume" on at least one
  system (a 32-bit Mac Mini). I'm committing this change because, imperfect
  though it is, it's preferable to the earlier version, at least on my
  small sample of computers.

- Because of news that the Linux kernel developers are planning to use the
  filename linux.conf to hold Linux kernel configuration data for EFI
  booting, I'm transitioning rEFInd away from that name and to
  refind_linux.conf to avoid a conflict. This version can use either name,
  with refind_linux.conf taking precedence if both are present.

- Added logo for Arch Linux.

0.2.2 (3/23/2012):
------------------

- Fixed bug that caused program failure when Linux kernels with EFI stub
  support were detected with no associated version numbers. rEFInd now
  permits automatic linking of *ONE* versionless kernel to *ONE*
  versionless initrd file.

- Fixed bug that caused program hangs when a boot loader filename or label
  was too long. Such names are now properly truncated and program execution
  continues.

- Fixed bug that caused no text to appear in submenus on UEFI systems with
  small screens (800x600). NOTE: Problem still occurs on screens smaller
  than this, but such systems are very rare.

0.2.1 (3/19/2012):
------------------

- Added ability to set a "default_selection" that's a title or a substring
  of one -- the name given to a stanza in a "menuentry" or the boot
  loader's filename, in most cases, although "Mac OS X", "Windows XP
  (XoM)", and "Microsoft EFI boot" are also titles.

- Added support for semi-automatic scans of Linux kernels with EFI stub
  loader support. The program auto-detects matching initial RAM disk files
  and loads additional options from the "linux.conf" file in the same
  directory as the kernel.

- Added support for "submenuentry" keyword and associated sub-stanza
  entries in refind.conf file.

- Renamed icons/os_mint.icns to icons/os_linuxmint.icns to match the
  filename Linux Mint ACTUALLY uses for its ESP boot loader directory.


0.2.0 (3/14/2012):
------------------

- Initial public release