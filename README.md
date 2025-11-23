# Ventura Gigabyte Z390 Aorus Master (OpenCore)

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.6-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS-Stable](https://img.shields.io/badge/macOS-13.7.8-brightgreen.svg)](https://www.apple.com/macos/ventura)

<img width="348" height="611" alt="ventura_1378" src="https://github.com/user-attachments/assets/2bad2bde-2afe-4bad-98dd-0ad8a7c7b406" />

## My PC Build
<details>
  <summary><strong>Hardware</strong></summary>
  
  | Category          | Component                                                | Note                                                  |
  | ----------------- | -------------------------------------------------------  | ----------------------------------------------------- |
  | CPU               | Intel Core i9-9900K                                      |                                                       |
  | GPU               | Asus RX 6900XT 16GB GDDR6 TUF Gaming                     | Native support                                        |
  | Motherboard       | Gigabyte Z390 AORUS MASTER                               |                                                       |
  | Storage (Windows) | Crucial P1 500GB 3D NAND NVMe PCIe (`M2M` slot)          | Internal NVME, Disable `PCIe Storage Dev on Port 17` on BIOS.|
  | Intel Optane      | MEMPEK1W032GAXT M.2 80mm PCIe 3.0  (`M2P` slot)          | Kernel Panic while macOS install on Internal NVME M2A. Enable `PCIe Storage Dev on Port 21` on BIOS. Cannot disable, use NVMeFix Kext + boot arg nvme=-1  |
  | Storage (macOS)   | Silicon Power SSD 512GB NVMe 1.3 P34A80                  | Box Lexar E6, Front USB C 3.1 Gen 1                   |
  | Memory            | Corsair Vengeance LPX 64GB (4x16GB) 3200MHz DDR4         |                                                       |
  | CPU Cooler        | EKWB EK-KIT Performance Series PC Watercooling Kit P360  |                                                       |
  | Power Supply      | Corsair RMX Series 80PLUS Gold 1000W                     |                                                       |
  | Case              | Cooler Master MasterCase H500M ARGB                      |                                                       |
  | Monitor           | LG UHD 4K IPS VESA DisplayHDR™ 400 27UP600K-W            |                                                       |
  | LAN               | Intel® i219v GbE LAN                                     | I use LAN for network                                 |
  | Wifi & BT         | Broadcom BCM943602CS 802.11 AC + Adapter PCIe x1         | Native support for both wifi and bluetooth, airdrop   |
  
</details>

<details>

<summary><strong>Kernel extensions</strong></summary>
<br>

| Kext                   | Version        |
|:---------------------- | -------------- |
| Lilu                   | 1.7.1          |
| NVMeFix                | 1.1.3          |
| VirtualSMC             | 1.3.7          |
| RestrictEvents         | 1.1.6          |
| SMCProcessor           | 1.3.2          |
| SMCSuperIO             | 1.3.2          |
| AppleALC               | 1.8.3          |
| IntelMausi             | 1.0.7          |
| USBMap                 | Manual         |

</details>

<details>
  <summary><strong>Working ✅ / Not Working ☑️</strong></summary>
  
  * ✅ Ethernet
  * ✅ Onboard Audio
  * ✅ iMessage
  * ✅ Sleep/Wake
  * ✅ Bluetooth & Wi-Fi
  * ✅ Airdrop
  * ✅ Handoff
  
</details>

## History
<details>
  <summary><strong>Changes</strong></summary>
  
  * **[23/11/2025]** enable Resize BAR (Smart Access Memory) on RX6900XT.
     - In BIOS changes `Above 4G Decoding` → Enable, `Re-Size BAR support` → Auto
     - macOS limitation to address BARs over 1 GB. `ResizeAppleGpuBars` set to `0` (BAR0). `ResizeGpuBars` set to `-1` for windows and motherboard handling during booting in windows.
  * **[1/10/2025]** remove WhateverGreen, macpro7,1 only use dedicated GPU, disable IGPU.
  * **[18/02/2023]** remove SSDT-PLUG due to macOS version >= 12.3 [link](https://dortania.github.io/OpenCore-Post-Install/universal/pm.html)
  
</details>

## Resources

* [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
* [Wifi-Bluetooth](https://openintelwireless.github.io/General/Installation.html)
* [Dortania build-repo](https://github.com/dortania/build-repo/releases)

## Tools
* [Hackintool](https://github.com/headkaze/Hackintool)
* [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools)
* [Wifi-Bluetooth kext](https://github.com/OpenIntelWireless)
* [OpenCore Configurator](https://mackie100projects.altervista.org/opencore-configurator/)
* [OC X Gen](https://github.com/Pavo-IM/OC-Gen-X)


## Thanks
* [cmer](https://github.com/cmer) : this is the first guide that I followed and try with Catalina 10.15.1
* [AudioGod](https://www.insanelymac.com/forum/topic/340936-audiogods-aorus-z390-master-patched-dsdt-efi-for-catalina-mini-guide-and-discussion/) : Currently, I use from him and change a little bit to make something well.
* [Colin Sullender](https://github.com/shiruken) : Previous, I try many times but cannot boot into Macintosh. Thanks for using Intel CNVI in your system. I just rebuild USB Map kext and everything works.
