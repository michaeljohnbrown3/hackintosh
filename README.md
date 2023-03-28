# Hackintosh

EFI and README for first Hackintosh build.

## Hardware

### CPU

Intel Core i3-9100F Desktop Processor 4 Core Up to 4.2 GHz without Processor Graphics LGA1151 300 Series 65W

<a href="https://www.intel.com/content/www/us/en/products/sku/190886/intel-core-i39100f-processor-6m-cache-up-to-4-20-ghz/specifications.html" target="_blank">Specifications</a>

### GPU

XFX Radeon RX 570 RS XXX Edition 1286MHz, 8gb GDDR5, DX12 VR Ready, Dual BIOS, 3xDP HDMI DVI, AMD Graphics Card (RX-570P8DFD6)

<a href="https://www.xfxforce.com/gpus/amd-radeon-tm-rx-570-rs-8gb-xxx-edition-2" target="_blank">Specifications</a>

### Memory

Corsair Vengeance LPX 16GB (2 X 8GB) DDR4 3600 (PC4-28800) C18 1.35V Desktop Memory - Black

<a href="https://www.corsair.com/us/en/Categories/Products/Memory/VENGEANCE-LPX/p/CMK16GX4M2D3600C18" target="_blank">Specifications</a>

### Motherboard

GIGABYTE B365M DS3H (LGA1151/Intel/Micro ATX/USB 3.1 Gen 1 (USB3.0) Type A/DDR4/Motherboard)

<a href="https://www.gigabyte.com/us/Motherboard/B365M-DS3H-WIFI-rev-1x#kf" target="_blank">Specifications</a>

### Storage

TC SUNBOW 1TB SSD 3D NAND Performance Boost SATA III 2.5" 7mm Internal Solid State Drive 1TB Back to School for College Students School Supply School Suppies

<a href="https://www.hardware-corner.net/ssd-database/TC-Sunbow-X3/" target="_blank">Specifications</a>

### Power Supply

Thermaltake Smart 500W 80+ White Certified PSU, Continuous Power with 120mm Ultra Quiet Cooling Fan, ATX 12V V2.3/EPS 12V Active PFC Power Supply PS-SPD-0500NPCWUS-W

<a href="https://www.thermaltakeusa.com/smart-500w.html" target="_blank">Specifications</a>

### Peripheral

#### Wireless Internet/Bluetooth Card

WiFi Card AX 5400Mbps 6E PCIe WiFi Card, Bluetooth 5.2, Intel WiFi 6E AX210 Chip, 6G/5G/2.4G PCIe WiFi 6 Card for PC Windows 11/10(64Bit)

<a href="http://www.ziyituod.net/prodetail.aspx?ProID=127" target="_blank">Specifications</a>

## Setting up your Hackintosh

Go through the MacOS Ventura setup process however you see fit.

### Downloads

#### OC Auxiliary Tools

<a href="https://github.com/ic005k/OCAuxiliaryTools/releases/download/20230019/OCAT_Mac.dmg" target="_blank">Download Here</a>

OC Auxiliary Tools is used to make changes to your EFI file.

### Benchmarking

#### Cinebench23

#### Unigine Heaven

## Issues while configuring and their resolutions with sources

The vast majority of this project was completed using <a href="https://dortania.github.io/OpenCore-Install-Guide/" target="_blank">Dortania's OpenCore Install Guide</a>

Help and Tools were usually found in Technolli's <a href="https://www.technolli.com/downloads" target="_blank">website</a> and <a href="https://www.youtube.com/@TechNolli" target="_blank">YouTube channel</a>.

Device can only be woken from sleep using the power button. Currently support for USB wake hasn't been figured out yet.

DRM is not supported for Netflix and Amazon on Safari, however it has been tested and is supported through Chrome and likely works in other browsers as well. I found part of the issue was an incorrect PCIList name. Source:

After hours spent trying to figure out Ethernet connectivity, ensure that LAN Controller is ENABLED in your BIOS.

There is no "Wake from Lan" on this motherboard's BIOS, you must use the GPRW to XPRW patch <a href="https://dortania.github.io/OpenCore-Post-Install/usb/misc/instant-wake.html" target="_blank">found on Dortania's guide</a>. I had trouble configuring the patch through OCAuxiliaryTool due to the hexidecimal input requirement. This process was made easy by opening the config.plist in VS Code and copying and pasting the patch directly into the file.

For the Wi-Fi and Bluetooth card to work, <a href="https://www.youtube.com/watch?v=8ztViUoN8h8" target="_blank">**the order of the kexts matters**</a>. Once placed in the correct order, both functioned perfectly, although there appears to be a slight delay in Wi-Fi connecting on boot. This is consistent and once connected there are no issues.

USB Mapping was difficult as I only had a USB 2.0 keyboard and mouse.
