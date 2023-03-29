# An OpenCore Hackintosh Setup Guide and Reflection

The purpose of this file is to provide guidance for the user of this build to setup and stress test their machine, as well as provide reflection of the setup process through OpenCore for the specifications of this machine. The hope is that the work done here is used to help someone who is struggling through their Hackintosh build, like I was multiple times.

---

## Main Sources

### <a href="https://dortania.github.io/OpenCore-Install-Guide/" target="_blank">Dortania's OpenCore Install Guide</a>

### <a href="https://www.technolli.com/downloads" target="_blank">Technolli</a>

### <a href="https://www.tonymacx86.com" target="_blank">TonyMacx86.com</a>

### <a href="https://discord.gg/8aKs69x" target="_blank">Discord Server</a>

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

# Out of the Box: Setting up MacOS Ventura

Follow the on screen MacOS Ventura setup process. If FileVault comes up as an option, save this for later.

## Time Machine

Time Machine is STRONGLY RECOMMENDED. While Time Machine can use up resources, updating your device is also recommended. If not followed carefully, however, various complications can arise. In these cases, it is important to have a backup that you can revert to.

## Downloads

<a href="https://github.com/ic005k/OCAuxiliaryTools/releases/download/20230019/OCAT_Mac.dmg" target="_blank">OC Auxiliary Tools</a> - Used to make changes to your EFI and update OpenCore, Kexts, or Drivers.

<a href="https://github.com/benbaker76/Hackintool/archive/refs/heads/master.zip" target="_blank">Hackintool</a> - NOT REQUIRED - Considered the Swiss army knife of vanilla Hackintoshing

<a href="https://installer.maxon.net/cinebench/CinebenchR23.dmg" target="_blank">Cinebench</a> - NOT REQUIRED - Used to stress test the CPU, monitor temperature during testing. The longer the test, the more reliable the results. 30-minutes is considered a better system stability test.

<a href="https://download.bjango.com/istatmenus/" target="_blank">iStat Menus</a> - NOT REQUIRED - Used to monitor various system resources while stress testing the CPU or GPU. This item is added to the list of start up items by default, which can slow your boot. After stress testing, it is recommended you either remove the program or turn off the ability to run in the background in system settings. To change to Celsius, open the iStat Menus app, click Sensors and you'll find temperature units.

To turn off in background - System Settings -> General -> Login Items -> Set "Allow in background" to False.

<a href="https://assets.unigine.com/d/Unigine_Heaven-4.0.dmg">Unigine Heaven:</a> - NOT REQUIRED - Used to stress test the GPU and measure performance

## Creating a Bootable USB Recovery Drive

It is strongly recommended that you create a recovery drive. This [process is described later](#creating-a-bootable-usb-with-macos-ventura) because it takes a basic understanding of how the EFI Partition works. The step-by-step is comprehensive and I aimed to give enough detail that you could navigate it without really understanding what is happening, but it is easier to follow if you read this whole document and follow the links. I also encourage you to follow Dortania's Guide.

## FileVault - VERY IMPORTANT

Upon the user receiving this Hackintosh, the machine was setup for FileVault to run based on Dortania's settings. This includes the UEFI output UI-Scale set to 02, which is for HiDPI displays (Apple's Retina 4k Displays).

The unwanted side effect is that OpenCore's booter resolution makes the image appear twice the size than it should. This side effect is more noticeable when FileVault is active as the login screen appears before booting (thus the hard drive is protected behind encryption and the user's password). The login screen is quite unsightly.

The truth is I have scoured all the resources at my disposal to find out the reason why UI-Scale matters to FileVault and have not found or received an answer. My best guess doesn't seem like a good one and I tested FileVault using a UI-Scale setting of 01, which is for standard definition. This did not seem to cause any issues, although it's possible that it has to do with the fact that this device utilizes a dedicated GPU.

If you are finding the large login screen too unsightly, I recommend taking the following steps as long as you have a STANDARD DEFINITION display (1920x1080).

1. Turn OFF FileVault:
   System Settings -> Privacy & Security -> FileVault: Turn Off
2. Set UI-Scale to 1 Read [Accessing the EFI](#accessing-the-efi):
   OCAuxiliaryTools -> Mount ESP (button on top, hover to see names) -> The main hard drive should already be selected -> Mount and open Config.plist -> UEFI -> Output -> UIScale set to 1 -> SAVE
3. REBOOT
4. Turn ON FileVault:
   System Settings -> Privacy & Security -> FileVault: Turn On

Be sure to allow FileVault to complete its process before rebooting, whether turning it on or off. I've read many forum anecdotes of people panicking, believing they are locked out of their device, but it's simply FileVault finishing it's setup before booting. I prefer not utilizing my AppleID for recovery purposes, and instead would opt for a recovery key, just in case the Hackintosh runs into internet issues at the same time it needs recovering, but I've also not experimented to much with this.

Note: If you intend to swap displays, you might consider going through the process above, especially if you are upgrading to a higher resolution display.

## Updating MacOS or OpenCore

In order to ensure utmost compatibility, refer to Dortania's Guide to <a href="https://dortania.github.io/OpenCore-Post-Install/universal/update.html#updating-opencore" target="_blank">Updating MacOS or OpenCore</a>. My advice would be to hold off on MacOS updates until the next OpenCore updates (beginning of each month), and at the very least, do a quick Google search to see if there are any common issues that users are experiencing with a particular update.

Note that all the steps provided can be accomplished in OCAuxiliaryTools and does not require utilizing the Terminal. OCAuxiliaryTools has the ability to update your kexts and OpenCore with just a few clicks.

1. Open OCAuxiliaryTools
2. Mount the EFI - Read [Accessing the EFI](#accessing-the-efi)
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

Keep in mind, the reason this works is best understood by reading through Dortania's explanation of the <a href="https://dortania.github.io/OpenCore-Install-Guide/troubleshooting/boot.html#opencore-booting" target="_blank">MacOS Boot Process</a>. This version of the EFI has boot picker, verbose mode, and debugging all engaged, so [read this section](#boot-picker-verbose-mode-and-debugging) to learn how to turn these settings off.

---

# EFI

## Accessing the EFI

To explain it plainly, your storage drive has a <a href="https://en.wikipedia.org/wiki/EFI_system_partition" target="_blank">EFI partition</a> that essentially contains boot information. This partition is managed by a Config.plist file. This partition is inaccesible without some form of scripting. OCAuxiliaryTools contains a simple function to mount a drive's EFI and open it's Config.plist file into a very readable application.

To access an EFI partition, open OCAuxiliaryTools. On the top of the window, you'll see the OpenCore Version and a row of buttons. You should be able to hover over the buttons to see their names. The sixth button should be "Mount ESP". This stands for Mount EFI System Partition. If you click this button you'll see all the available drives. Ensure the hard drive that you boot from is selected and click "Mount and open Config.plist".

This will inject your Config.plist file into the OCAuxiliaryTools application. Now you have the ability to change any settings in your Config.plist file. Use the Finder application to add/remove files, kexts, and drivers into the booter. Anything added or removed from the file director of the EFI MUST be updated in the Config.plist file or the system will run into boot issues.

[Back to FileVault in Setup](#filevault---very-important)

[Back to Updating MacOS or OpenCore](#updating-macos-or-opencore)

## Creating a Bootable USB with MacOS Ventura

Dortania has a <a href="https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html#downloading-macos-modern-os" target="_blank">very indepth</a> guide on this process. The challenge for me was completing the process on MacOS El Capitan created some continuity issues with the guide. The complications are actually the reason why I used OpenCore's Legacy Installer on my 2009 MacBook Pro, which is now running Ventura with only very minor graphical bugs. Having this newer OS made creating the USB a breeze.

1. Plug in a blank 16GB USB Drive and open Disk Utility
2. Select the highest level of your USB flash drive (picture shown in Dortania if you need further clarification), select erase. You should be prompted with three options. The name should be: MyVolume. Note: we name this "MyVolume" so that when we copy and paste the command in step 4, we won't have to change anything. You could really make the name whatever you would like, as long as you change "MyVolume" in the command to the new name. The Format should be MacOS Extended (Journaled). The Scheme should be GUID Partition Map. Note: if the scheme is not an option, then you are likely not in the highest level of the drive, or there is an issue with the drive.
3. Download a copy of MacOS Ventura - this can be tricky because the App Store doesn't always want to give you a download. If you can do this, great, if not try downloading the latest installer from <a href="https://osxdaily.com/download-macos-ventura-13-full-installer/" target="_blank">OSX Daily</a>. The download you received needs to be launched to install the installer (I know how this sounds, but trust me, I wasted time not understanding why my .pkg wouldn't install to the USB), and make sure the installer ends up in your Applications Folder.
4. Use <a href="https://support.apple.com/en-us/HT201372" target="_blank">Apple's directions</a> for creating a Bootable USB with MacOS Ventura on it. When I did this, I ran into some issues with the Terminal that would say "sudo: command not found". The reason for this is your user might not have root permissions. The work around for this was to run the command "sudo su", which then prompts you for your password. Once entered, copy and paste the rest of the command and run it. This process will take some time.
5. Download the EFI Folder to your Desktop from this repository. This version has boot picker, verbose mode, and debugging all enabled so that you are able to select how you wish to proceed at boot. I recommend you update the EFI on this drive with the last known working EFI from your device. That said, if it's a recovery disk you are making, you should enable boot picker, verbose mode, and debugging. More on this in the next section.
6. Use OCAuxiliaryTools to Mount the EFI of the USB Flash Drive (Navigate to it in Finder and it should be empty)
7. Drag and drop the EFI Folder from your Desktop to the EFI Partition and voila! You should now have a Bootable USB Recovery Flash Drive.

Note: While it seems logical that you might be able to simply place the EFI from this repository on an empty USB drive and boot off ot it, I have not tested this myself and am skeptical that it would be that simple. Besides, it's always good to have a USB copy of your OS sitting around.

[Back to Creating a USB Recovery Drive in Setup](#creating-a-bootable-usb-recovery-drive)

## Boot Picker, Verbose mode, and Debugging

If you're creating a bootable recovery usb using your own EFI, or you used the recovery USB, ensured your device is stable and are ready to remove these arguments, you'll need to know where to access them, and how to enable or disable them.

To access verbose mode and debugging, mount your EFI as we've done before. Click on NVRAM then 7C436110-AB2A-4BBB-A880-FE41995C9F82. You should see a property called "boot-args". This is where arguments are specified at boot and are enabled for the duration of the system's session.

To enable verbose mode and debugging, double click on value cell, move your cursor to the front of the line of text and add in "-v debug=0x100 ". Ensure there is a space between the two as well as before any other boot arguments.

To access boot picker, mount your EFI if you haven't done so already. Click on Misc -> Boot (if not selected automatically) -> you should see a checkbox next to "Show Picker". If selected, you will see a drive picker at boot. If empty you will not.

[Back to Utilizing the EFI Folder in this Repository for Recovery](#utilizing-the-efi-folder-in-this-repository-for-recovery)

---

# Building this Hackintosh

If you've gotten this Hackintosh set up and functioning, enjoy it! You're done reading! However, if you're someone going through a build right now and looking for any resource you can find to help get you unstuck, I hope you can find the rest of this document useful.

I ran into many issues and spent hours upon hours on my first build. I learned so much and only made progress because people would share what they did in forums or in their own Github repos. I hope this helps someone in need.

The vast majority of this project was completed using <a href="https://dortania.github.io/OpenCore-Install-Guide/" target="_blank">Dortania's OpenCore Install Guide</a>

Help and Tools were usually found in Technolli's <a href="https://www.technolli.com/downloads" target="_blank">website</a> and <a href="https://www.youtube.com/@TechNolli" target="_blank">YouTube channel</a>.

## OpenCore Resolution at Boot - Not resolved

The symptom here is that the Apple logo at the beginning of the boot process is double the size of what it should be. Although this corrects itself, it is quite unsightly when FileVault is turned on as the login screen is oversized. The solution for this is the UI-Scale, but according to Dortania, the UI-Scale needs to be set to 02 for FileVault to work properly on HiDPI Displays (better known as Retina 4k Displays). I do not have one of these displays so I was not able to test it, although I attempted to run FileVault with the UI-Scale set to 01 on my 1080p display and experienced no visible issues. Since the logo is still twice the size, I will list this as unresolved.

## Wake from sleep using USB Keyboard/Mouse - Not resolved.

Device can only be woken from sleep using the power button. Currently support for USB wake hasn't been figured out yet. I have read multiple forums about this topic and feel this is a motherboard issue. Various applications of fixes have all failed. I do believe that using the Apple Bluetooth Magic Keyboard and Magic Mouse will likely resolve the issue from a user perspective, but I will leave this listed as unresolved as I have not found a solution, given the current hardware. For now the user will have to use the power button to wake.

## DRM support (or lackthereof) - SORT OF RESOLVED

DRM is not supported for Netflix and Amazon on Safari, however it has been tested and is supported through Chrome and likely works in other browsers as well. I found part of the issue was an incorrect PCIList name. Source was a <a href="https://discord.gg/8aKs69x" target="_blank">Discord</a>

## Ethernet - EMBARRASSINGLY RESOLVED

After hours spent trying to figure out Ethernet connectivity, ensure that LAN Controller is ENABLED in your BIOS. With the time that you would have wasted troubleshooting this issue, not realizing it was disabled in your bios, go do something fun!

## Sleep issue - RESOLVED

There is no "Wake from Lan" on this motherboard's BIOS, you must use the GPRW to XPRW patch <a href="https://dortania.github.io/OpenCore-Post-Install/usb/misc/instant-wake.html" target="_blank">found on Dortania's guide</a>. I had trouble configuring the patch through OCAuxiliaryTool due to the hexidecimal input requirement. This process was made easy by opening the config.plist in VS Code and copying and pasting the patch directly into the file.

## Wi-FI/Bluetooth Card - RESOLVED

For the Wi-Fi and Bluetooth card to work, <a href="https://www.youtube.com/watch?v=8ztViUoN8h8" target="_blank">**the order of the kexts in your config.plist matters**</a>. Once placed in the correct order, both functioned perfectly, although there appears to be a slight delay in Wi-Fi connecting on boot. This is consistent and once connected there are no issues. I used OCAuxiliaryTool even though the video uses a different tool.

## USB Mapping - RESOLVED

USB Mapping was difficult as I only had a USB 2.0 keyboard and mouse. This meant that every time I booted with XhciPortLimit enabled, I could not use my keyboard and mouse and had to reboot from my recovery USB. I've seen multiple posts that XhciPortLimit is broken, but that wasn't what I found. What I found was MacOS was able to list all my inputs, listed USB ports twice that were both USB 2.0 and USB 3.0, and only noted the USB 3.0 devices I plugged in (I had a USB 3.0 Flash Drive lying around). As a result I used the iOS app called "Remote Mouse & Keyboard" to access controls of the device without needing anything plugged into USB. You'll need to download the companion app to your device as well for it to work. Test it BEFORE trying the following steps. Using <a href="https://github.com/benbaker76/Hackintool/archive/refs/heads/master.zip" target="_blank">Hackintool</a>, I was able to map the ports in this order:

1. Open Hackintool, plug a USB 2.0 device in and out of every port (this device has 9 ports, plus the USB header that the Wi-Fi/Bluetooth Card is plugged into, so 10 devices).
2. Open OCAuxiliaryTool and set XhciPortLimit to TRUE and reboot.
3. Use Remote Mouse & Keyboard to open Hackintool again, the USB 2.0 devices should still be green, but you'll notice a LOT more devices. These are your USB 3.0 devices. Now plug a USB 3.0 device into all the USB 3.0 ports.
4. Press export at the bottom and it should create the Kext file you need now for USB Mapping.

<a href="https://www.youtube.com/watch?v=2hZPMHSfkS0" target="_blank">Technolli's video</a> on this was my primary source for this step-by-step. Do note that the Remote Mouse & Keyboard app is a PAID app, but contains a 3-day trial. Be sure to cancel once you've completed your port mapping.
