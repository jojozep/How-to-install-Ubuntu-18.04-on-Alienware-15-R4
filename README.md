# How to install Ubuntu 18.04 on Alienware 15 R4
## Overview
This guide descibes how to set up a dual-boot Alienware 15 R4 laptop with Windows 10 and Ubuntu 18.04 The system has the following drives:

|Purpose|Size|Type|Form|Source|Description|
|-------|----|----|----|------|-----------|
|Windows|256GB|SSD NVMe|2.5in|Standard with 15R4|Toshiba THNSN5256GPUK NVMe/PCIe M.2 PCIe|
|Ubuntu|240GB|SSD NVMe|2242|Optional: [Amazon](https://www.amazon.com/gp/product/B07DD4FWRL)|Toshiba RC100 THN-RC10Z2400G8 M.2 PCIe|
|Shared|1TB|SATA III|2280|Standard with 15R4|HGST Travelstar 7K1000 7200 RPM 32MB Cache|
## Acknowledgement
This guide borrows heavily from [alienware15r3_ubuntu14 guide](https://github.com/awesomebytes/alienware15r3_ubuntu14) and [Alienware 17 R4: Dual Boot Windows 10 Ubuntu 16.04](https://github.com/stormhart/dev-journal/wiki/Alienware-17-R4:-Dual-Boot-Windows-10---Ubuntu-16.04). Thanks [awesomebytes](https://github.com/awesomebytes) and [stormhart](https://github.com/stormhart). A new guide was needed as the ubuntu version, laptop model and some steps have changed. 
## Pre-installation
### Setup Ubuntu installation drive 
You need to have a drive to set up Ubuntu on. You have two choices:
1. Use a new drive<br/>I bought a new SSD drive as they are relatively inexpensive. The 15R4 has 2 spare M.2 SSD slots: a 2242 and 2280. I kept the 2280 slot as a future spare, and bought a 2242 form SSD drive. They are hard to find, but I would recommend the [Toshiba RC100](https://www.amazon.com/gp/product/B07DD4FWRL). Installation of the drive is relatively simple, you will need a small philips-head screwdriver and remove the 7 screws at the bottom of the laptop to access the SSD slot (see [Alienware Documentation](https://www.dell.com/support/home/us/en/19/product-support/product/alienware-15/manuals) for diagrams).
2. Resize your existing drive to create space for Ubuntu<br/>Use the Windows “Disk Management” utility (in Control Panel>>Administrative Tools>>Computer Management), select the drive, and "then Shrink Volume" (right-mouse click on the drive). You should allocate at least 30GB, and preferably 50GB.
### Create a USB boot drive
1. Download `Ubuntu 18.04 LTS` ISO from the [Ubuntu download site](https://www.ubuntu.com/download/desktop)
2. Download `Rufus` from the [Rufus download site](https://rufus.akeo.ie/)
3. Use Rufus to create a `bootable USB stick` by setting options similar to below<br/> This may take 15-20 minutes.
**Be careful to set Device as the USB drive**.
### Disable Secure Boot
This is required so that you can also boot to Ubuntu later
1. Turn on your system
2. Tap the `F2` key as the system turns on
3. You will access the system setup utility
4. Click the `Boot` tab
5. Highlight `Secure Boot`
6. Press `Enter`, select `Disable` and press `Enter` again.
7. Press `F10` and select `Yes`<br/>
`NOTE:` If you are using a PIN instead of a password to login, you may get an error when logging in. If so, select the option to login using a password, and then reset the PIN when you have logged in.
### Set Boot Mode to AHCI
The default boot mode is RAID. Directly setting boot mode to AHCI in the BIOS will result in Windows 10 boot failure. Follow the steps below to boot in AHCI successfully.
1. Run `Command Prompt` as Administrator<br/>In the search bar, search for `Command Prompt`, right-mouse click on the icon and select `Run as Administrator`
2. Enter `bcdedit /set {current} safeboot minimal` exactly, to invoke a Safe Mode boot
3. Restart the PC and enter your BIOS during bootup (see above)
4. In tab `Advanced` tab, change option `SATA Operation` from `RAID` to `AHCI mode`.
5. Press `F10` to Save and Exit
6. Windows 10 will now launch in Safe Mode.
7. Right click the `Window icon` and select to `run the Command Prompt in Admin mode` from among the various options.
8. Enter `bcdedit /deletevalue {current} safeboot` exactly, to cancel Safe Mode booting
9. Restart your PC once more and this time it will boot up normally but with AHCI mode activated.
`NOTE:` While you can disable combine BIOS changes (i.e. disabling secure boot and setting boot mode to AHCI), its recommended to do these separately due to PIN issue.
## Installation of Ubuntu
`NOTE:` You will need a USB mouse, as the touchpad is not support in Ubuntu.
1. Click the `Install Ubuntu` icon on the desktop.
2. **Do not connect to the internet**. Do not check the Download Updates option.
3. Select `Something Else` from the different installation options.
4. You should see some partitions like */dev/nvme0n1*
5. Select the empty SSD from the list of drives.<br/>**DO NOT ACCIDENTALLY SELECT THE SSD WHERE YOU HAVE YOUR WINDOWS 10 INSTALLED. THIS CANNOT BE UNDONE**
6. Click `New Partition Table`
7. Select the free space partition which was freshly created and press `+` (Or just double click the free space)
8. Allocate the entire space for Ubuntu, using `ext4` format
9. Do not set up any swap drive.<br/>While swap drives were needed in old systems, any modern system with adeqaute memory doesn't need it. More importantly, the performance of SSD drives degrades with access so high read/write access like swap drives will degrade the drive.
Click `Install Now`
## Post Installation
### Add support for the touchpad
1. In a terminal windows enter `sudo echo 'blacklist i2c_hid' >> /etc/modprobe.d/blacklist.conf`
2. Enter `sudo depmod -a`
3. Enter `sudo update-initramfs -u`
### Do nothing
* gparted is already in 18.04, no need to add gparted driver
* Wireless and wired ethernet will work, no need to add new drivers
* The share drive will be available automatically (in `File Manager` select `+Other Locations`), no need to add NVMe drivers
* Do not add any drivers from any source you don't know, even github. This installation explicitly doesn't require anything except standard tools and setup.
## Caveats
* Sleep/Suspend in Ubuntu has show some problems - still investigating - stay tuned
* Windows 10 Professional has issues with the latest versions of Anaconda3
* Cuda drivers are not available for 18.04, see https://askubuntu.com/questions/1028830/how-do-i-install-cuda-on-ubuntu-18-04
