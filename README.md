# An OpenCore Hackintosh Setup Guide and Reflection

The purpose of this file is to provide guidance for the user of this build to setup and stress test their machine, as well as provide reflection of the setup process through OpenCore for the specifications of this machine. The hope is that the work done here is used to help someone who is struggling through their Hackintosh build, like I was multiple times.

---

## Main Sources

### <a href="https://dortania.github.io/OpenCore-Install-Guide/" target="_blank">Dortania's OpenCore Install Guide</a>

### <a href="https://www.technolli.com/downloads" target="_blank">Technolli</a>

### <a href="https://www.tonymacx86.com" target="_blank">TonyMacx86.com</a>

---

## Hardware

**<a href="https://www.intel.com/content/www/us/en/products/sku/190886/intel-core-i39100f-processor-6m-cache-up-to-4-20-ghz/specifications.html" target="_blank">CPU:</a>** Intel Core i3-9100F Desktop Processor 4 Core Up to 4.2 GHz without Processor Graphics LGA1151 300 Series 65W

**<a href="https://www.xfxforce.com/gpus/amd-radeon-tm-rx-570-rs-8gb-xxx-edition-2" target="_blank">GPU:</a>** XFX Radeon RX 570 RS XXX Edition 1286MHz, 8gb GDDR5, DX12 VR Ready, Dual BIOS, 3xDP HDMI DVI, AMD Graphics Card (RX-570P8DFD6)

**<a href="https://www.corsair.com/us/en/Categories/Products/Memory/VENGEANCE-LPX/p/CMK16GX4M2D3600C18" target="_blank">Memory:</a>** Corsair Vengeance LPX 16GB (2 X 8GB) DDR4 3600 (PC4-28800) C18 1.35V Desktop Memory - Black

**<a href="https://www.gigabyte.com/us/Motherboard/B365M-DS3H-WIFI-rev-1x#kf" target="_blank">Motherboard:</a>** GIGABYTE B365M DS3H (LGA1151/Intel/Micro ATX/USB 3.1 Gen 1 (USB3.0) Type A/DDR4/Motherboard)

**<a href="https://www.hardware-corner.net/ssd-database/TC-Sunbow-X3/" target="_blank">Storage:</a>** TC SUNBOW 1TB SSD 3D NAND Performance Boost SATA III 2.5" 7mm Internal Solid State Drive 1TB Back to School for College Students School Supply School Suppies

**<a href="https://www.thermaltakeusa.com/smart-500w.html" target="_blank">Power Supply:</a>** Thermaltake Smart 500W 80+ White Certified PSU, Continuous Power with 120mm Ultra Quiet Cooling Fan, ATX 12V V2.3/EPS 12V Active PFC Power Supply PS-SPD-0500NPCWUS-W

**<a href="http://www.ziyituod.net/prodetail.aspx?ProID=127" target="_blank">Wireless Internet/Bluetooth Card:</a>** WiFi Card AX 5400Mbps 6E PCIe WiFi Card, Bluetooth 5.2, Intel WiFi 6E AX210 Chip, 6G/5G/2.4G PCIe WiFi 6 Card for PC Windows 11/10(64Bit)

---

# Setting up your Hackintosh

Follow the MacOS Ventura setup process.

<!-- ## Creating a Bootable USB Recovery Drive -->

## Time Machine

Time Machine is STRONGLY RECOMMENDED. While Time Machine can use up resources, updating your device is also recommended. If not followed carefully, however, various complications can arise. In these cases, it is important to have a backup that you can revert to.

## Downloads

<a href="https://github.com/ic005k/OCAuxiliaryTools/releases/download/20230019/OCAT_Mac.dmg" target="_blank">OC Auxiliary Tools</a> - Used to make changes to your EFI, update OpenCore, kexts, or drivers, and .

<a href="https://installer.maxon.net/cinebench/CinebenchR23.dmg" target="_blank">Cinebench</a> - Used to stress test the CPU, monitor temperature during testing. The longer the test, the more reliable the results. 30-minutes is considered a better system stability test.

<a href="https://download.bjango.com/istatmenus/" target="_blank">iStat Menus</a> - Used to monitor various system resources while stress testing the CPU or GPU. This item is added to the list of start up items by default, which can slow your boot. After stress testing, it is recommended you either remove the program or turn off the ability to run in the background in system settings. To change to Celsius, open the iStat Menus app, click Sensors and you'll find temperature units.

To turn off in background - System Settings -> General -> Login Items -> Set "Allow in background" to False.

<a href="https://assets.unigine.com/d/Unigine_Heaven-4.0.dmg">Unigine Heaven:</a> - Used to stress the GPU and measure performance

## FileVault - VERY IMPORTANT

Upon the user receiving this Hackintosh, the machine was setup for FileVault to run based on Dortania's settings. This includes the UEFI output UI-Scale set to 02, which is for HiDPI displays (Apple's Retina 4k Displays).

The unwanted side effect is that OpenCore's booter resolution makes the image appear twice the size than it should. This side effect is more noticeable when FileVault is active as the login screen appears before booting (thus the hard drive is protected behind encryption and the user's password). The login screen is quite unsightly.

The truth is I have scoured all the resources at my disposal to find out the reason why UI-Scale matters to FileVault and have not found or received an answer. My best guess doesn't seem like a good one and I tested FileVault using a UI-Scale setting of 01, which is for standard definition. This did not seem to cause any issues, although it's possible that it has to do with the fact that this device utilizes a dedicated GPU.

If you are finding the large login screen too unsightly, I recommend taking the following steps as long as you have a STANDARD DEFINITION display (1920x1080).

1. Turn OFF FileVault:
   System Settings -> Privacy & Security -> FileVault: Turn Off
2. Set UI-Scale to 1 Read [Accessing the EFI](doc:#EFI#Accessing-the-EFI):
   OCAuxiliaryTools -> Mount ESP (button on top, hover to see names) -> The main hard drive should already be selected -> Mount and open Config.plist -> UEFI -> Output -> UIScale set to 1 -> SAVE
3. REBOOT
4. Turn ON FileVault:
   System Settings -> Privacy & Security -> FileVault: Turn On

Be sure to allow FileVault to complete its process before rebooting, whether turning it on or off. I've read many forum anecdotes of people panicking, believing they are locked out of their device, but it's simply FileVault finishing it's setup before booting. I prefer not utilizing my AppleID for recovery purposes, and instead would opt for a recovery key, just in case the Hackintosh runs into internet issues at the same time it needs recovering, but I've also not experimented to much with this.

Note: If you intend to swap displays, you might consider going through the process above, especially if you are upgrading to a higher resolution display.

## Updating MacOS or OpenCore

In order to ensure utmost compatibility, refer to Dortania's Guide to <a href="https://dortania.github.io/OpenCore-Post-Install/universal/update.html#updating-opencore" target="_blank">Updating MacOS or OpenCore</a>

Note that all the steps provided can be accomplished in OCAuxiliaryTools and does not require utilizing the Terminal. OCAuxiliaryTools has the ability to update your kexts and OpenCore with just a few clicks.

1. Open OCAuxiliaryTools
2. Mount the EFI - Read [Accessing the EFI](doc:#EFI#Accessing-the-EFI)
3. To the left of the "Mount ESP" button is the "Upgrade OpenCore and Kexts" button, click this.
4. On the right side, click "Get the latest version of OpenCore".
5. On the left side, click "Select all" -> "Check for Kext updates"
6. After this process finishes, click "Update Kexts" if it is available.
7. On the right side again, click Start Sync.
8. SAVE

OpenCore and your Kexts should now be updated. Reboot to test. Here's where having the EFI in this repository and a Time Machine backup are critical.

## Utilizing the EFI Folder in this Repository for Recovery

Should you run into boot issues, you can follow the following steps to recover.

1. Power down the Hackintosh.
2. Insert the USB Recovery flash drive you created earlier.
3. Power on the Hackintosh and start mashing the Delete key.
4. In the system BIOS, set the boot priority to the UEFI Flash Drive, Save & Exit
5. Upon rebooting, you should see the OpenCore picker, where you can selected MacOS to boot from. If you do not make a selection, it should automatically select the MacOS partition.

Keep in mind, the reason this works is best understood by reading through Dortania's explanation of the <a href="https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/boot.html#opencore-booting" target="_blank">MacOS Boot Process</a>.

---

# EFI

## Accessing the EFI

To explain it plainly, your storage drive has a <a href="https://en.wikipedia.org/wiki/EFI_system_partition" target="_blank">EFI partition</a> that essentially contains boot information. This partition is managed by a Config.plist file. This partition is inaccesible without some form of scripting. OCAuxiliaryTools contains a simple function to mount a drive's EFI and open it's Config.plist file into a very readable application.

To access an EFI partition, open OCAuxiliaryTools. On the top of the window, you'll see the OpenCore Version and a row of buttons. You should be able to hover over the buttons to see their names. The sixth button should be "Mount ESP". This stands for Mount EFI System Partition. If you click this button you'll see all the available drives. Ensure the hard drive that you boot from is selected and click "Mount and open Config.plist".

This will inject your Config.plist file into the OCAuxiliaryTools application. Now you have the ability to change any settings in your Config.plist file. Use the Finder application to add/remove files, kexts, and drivers into the booter. Anything added or removed from the file director of the EFI MUST be updated in the Config.plist file or the system will run into boot issues.

<!-- ## Creating a Bootable USB with MacOS Ventura -->

---

## Issues while configuring and their resolutions with sources

The vast majority of this project was completed using <a href="https://dortania.github.io/OpenCore-Install-Guide/" target="_blank">Dortania's OpenCore Install Guide</a>

Help and Tools were usually found in Technolli's <a href="https://www.technolli.com/downloads" target="_blank">website</a> and <a href="https://www.youtube.com/@TechNolli" target="_blank">YouTube channel</a>.

Device can only be woken from sleep using the power button. Currently support for USB wake hasn't been figured out yet.

DRM is not supported for Netflix and Amazon on Safari, however it has been tested and is supported through Chrome and likely works in other browsers as well. I found part of the issue was an incorrect PCIList name. Source:

After hours spent trying to figure out Ethernet connectivity, ensure that LAN Controller is ENABLED in your BIOS.

There is no "Wake from Lan" on this motherboard's BIOS, you must use the GPRW to XPRW patch <a href="https://dortania.github.io/OpenCore-Post-Install/usb/misc/instant-wake.html" target="_blank">found on Dortania's guide</a>. I had trouble configuring the patch through OCAuxiliaryTool due to the hexidecimal input requirement. This process was made easy by opening the config.plist in VS Code and copying and pasting the patch directly into the file.

For the Wi-Fi and Bluetooth card to work, <a href="https://www.youtube.com/watch?v=8ztViUoN8h8" target="_blank">**the order of the kexts matters**</a>. Once placed in the correct order, both functioned perfectly, although there appears to be a slight delay in Wi-Fi connecting on boot. This is consistent and once connected there are no issues.

USB Mapping was difficult as I only had a USB 2.0 keyboard and mouse.
