This is a fork of https://github.com/serpro69/dell-precision-5530_ubuntu-win_dualboot

# Instructions on provisioning Dell XPS 15 9650 with dualbooted Ubuntu (currently 18.04) + Windows 10

## BIOS
### Updating BIOS
As a first step after booting up a new laptop I always update the BIOS to latest version. This can be found on [DELL's support site](https://www.dell.com/support/home/en/us/nodhs1/product-support/product/precision-15-5530-laptop/drivers). Simply download and run the executable file and the runner will do the job.
*Note: it is advisable to charge the laptop and connect it to AC. Any interruption during BIOS update may render the laptop unusable*

### Tweaking BIOS for dual-boot
Several changes to BIOS are needed. I've separated them into mandatory and optional as show below.
Mandatory:
* General -> Boot sequence - UEFI
* General -> Advanced boot options -> Uncheck 'Enable Legacy Option ROMs'
* Secure boot -> Secure boot enabled -> Disable
* Switch SATA operation to ACHI (Do this after installing windows and setting safeboot to minimal. See [Windows won't boot with ACHI mode](#windows-won't-boot-with-achi-mode) for details.)

Optionally:
* Battery -> Set change level from 50 to 80 (As I mostly use AC I don't want the battery to always keep charged to 100%, this should prolong the battery life in the long run.)  

*Some extra info on setting up BIOS for dual-boot can be found on [DELL's support page](https://www.dell.com/support/article/no/no/nodhs1/sln301754/how-to-install-ubuntu-and-windows-8-or-10-as-a-dual-boot-on-your-dell-pc?lang=en#BIOS).*

## Reinstalling Windows 10
It is advisable to reinstall Windows 10 on a newly-bought laptop.
The default OS often comes bundled with bloatware, outdated drivers, etc., hence I prefer to always start with a clean install.

### Downloading drivers and software
It is advisable to make some changes to Windows 10 prior to connecting to internet.
A clean Windows 10 is sh\*t and a huge spyware. If you are concerned about your privacy (which you should be) then you should follow the guidelines on how to disable telemetry, cortana and apply other privacy related tweaks to Windows 10 (See below.) prior to connecting to internet. Therefore you will need some software and drivers to be downloaded first.

Prior to wiping the hard drive and installing clean Windows 10 download the following drivers, software and win10 tweak files.
They will be needed later after installing fresh OS.
Download the drivers from [DELL Precision 5530 Drivers](https://www.dell.com/support/home/en/us/nodhs1/product-support/product/xps-15-9560-laptop/drivers)

### Downloading Windows 10 image
Windows 10 images are available for [download from Microsoft support site](https://www.microsoft.com/en-us/software-download/windows10ISO).
However, if downloading from Windows OS, then Microsoft will try to detect your OS and will want you to use their Media Creation Tool.
IF you don't like to use the tool there is still an option to download the iso file directly, all you need to do is make MS servers think you're using an unsupported OS.
To do this simply switch the user agent that your browser sends, for example to Safari on iPad, and it will allow you to get a direct download link for the iso image of Windows.

### Creating bootable flash drive with Win10
There is plenty of software that can create a bootable flash drive from the iso image, I personally prefer [Rufus](https://rufus.ie/).
- First verify the iso image downloaded from Microsoft. This can also be done with Rufus. For more info see [FAQ](https://github.com/pbatard/rufus/wiki/FAQ#How_can_I_validate_that_a_Windows_ISO_is_a_genuine_retail_version).
- Then create the bootable flash drive in UEFI mode.
*For more details see full [Rufus FAQ](https://github.com/pbatard/rufus/wiki/FAQ).*

### Installing Windows 10
- Reboot and press F12 for one-time boot menu.
- Select UEFI flash drive
- Follow [Windows 10 Clean Install Guide](http://forum.notebookreview.com/threads/windows-10-clean-installation-guide.781178/)

## Installing Ubuntu
### Downloading iso image
For this guide I have used the latest LTS version available at the time - Ubuntu 18.04.2 LTS.
I haven't tried the short-term support versions and can't guarantee everything will work the same.  

Download Ubuntu Desktop version from here: [link](https://ubuntu.com/download/desktop).

### Creating bootable flash drive with Ubuntu
The instructions are same as for Windows 10 bootable flash drive - see this [section](#creating-bootable-flash-drive-with-win10) for more details.

### Installing Ubuntu as second boot
This step is pretty straight-forward. After creating a bootable drive with Ubuntu just boot from the flash-drive using a one-time-boot menu (F12)
and follow the on-screen instructions.

### Partitioning SDD drive for Ubuntu installation
I have a 1 TB SSD drive and I have partitioned it the following way:  

- Windows and shared:
    - System - 90G
    - Shared Data - 600G
- Ubuntu
    - `/` - 36G
    - `/home` - 274G
    - SWAP - 18G (Though it's recommended to have twice of RAM on SWAP I think allocating 64G for SWAP would be an overkill.)

*Note: after installing Windows OS I have left the rest of the space unallocated and partitioned the rest during ubuntu installation.*  

**Additional read:**  
- [How to use manual partitioning during installation on AskUbuntu](https://askubuntu.com/questions/343268/how-to-use-manual-partitioning-during-installation)
- [Drives and Partitions on Ubuntu Help Wiki](https://help.ubuntu.com/community/DrivesAndPartitions)

### Dell Precision 5530 tweaks for Ubuntu
The first step I took after installing Ubuntu is running a script from this repo:
[DELL XPS 15 9570 Ubuntu 18.04 respin and post installation script](https://github.com/JackHack96/dell-xps-9570-ubuntu-respin)  

To run the script without cloning the repo execute the following command (requires curl):
```
bash -c "$(curl -fsSL https://raw.githubusercontent.com/JackHack96/dell-xps-9570-ubuntu-respin/master/xps-tweaks.sh)"
```

Or clone the repo and run from the cloned dir (requires git):
```
$ cd ~ && git clone git@github.com:JackHack96/dell-xps-9570-ubuntu-respin.git
$ sudo /bin/bash ~/dell-xps-9570-ubuntu-respin/xps-tweaks.sh
```

This will install several drivers (including prime drivers for nvidia/intel graphics) and add some extra tweaks to the OS.

### Creating shared drive for Ubuntu and Windows

### Installing software packages and drivers
This is an optional list of things that I use. Feel free to skip entirely or pick out what you might need.  

### Gnome Shell Extensions
First install `$ sudo apt install gnome-shell-extensions gnome-tweaks`

Then install the following extensions:
- [AlternateTab](https://extensions.gnome.org/extension/15/alternatetab/)
- [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock/)
- [Hide Activities Button](https://extensions.gnome.org/extension/744/hide-activities-button/)
- [KStatusNotifierItem/AppIndicator Support](https://extensions.gnome.org/extension/615/appindicator-support/)
- [Launch new instance](https://extensions.gnome.org/extension/600/launch-new-instance/)
- [Minimize All](https://extensions.gnome.org/extension/760/minimize-all/)
- [Native Window Placement](https://extensions.gnome.org/extension/18/native-window-placement/)
- [NoAnnoyance](https://extensions.gnome.org/extension/1236/noannoyance/)
- [Places Status Indicator](https://extensions.gnome.org/extension/8/places-status-indicator/)
- [Removable Drive Menu](https://extensions.gnome.org/extension/7/removable-drive-menu/)
- [Remove Dropdown Arrows](https://extensions.gnome.org/extension/800/remove-dropdown-arrows/)
- [Syncthing Icon](https://extensions.gnome.org/extension/989/syncthing-icon/)
- [User Themes](https://extensions.gnome.org/extension/19/user-themes/)
- [windowNavigator](https://extensions.gnome.org/extension/10/windownavigator/)

### Optional software
- [Dropbox](https://www.dropbox.com/install-linux)
- [vscode](https://code.visualstudio.com/)
- [Firefox Browser](https://www.mozilla.org/en-US/firefox/new/)
  - Extensions:
    - [NoScript](https://addons.mozilla.org/en-US/firefox/addon/noscript/)
    - [uBLock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/)
