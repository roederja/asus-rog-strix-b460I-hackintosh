# Hackintosh ASUS ROG STRIX B460I

This repository is about a Hackintosh based on the **Asus ROG STRIX B460I** mother board.

The Hackintosh is based on OpenCore (0.6.1 at time of writing) and macOS Catalina 10.15.6 following the [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/) for [Comet Lake](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#starting-point).

The focus of this Hackintosh was looks, functionality and quiet operation rather than performance per Dollar. This has been achieved since there is currently nothing that doesn't work and the fans barely spin at all.

## Hardware

* Case: Louque Ghost S1
* Motherboard: Asus ROG STRIX B460-I (BIOS version 0401)
* WiFi module: Broadcom BCM94360NG NGFF M.2. This replaces the intel chip that comes with the Asus board. See [here](https://www.tonymacx86.com/threads/the-everything-works-asus-z390-i-gaming-i7-8700k-sapphire-nitro-radeon-rx-vega-64-build.272572/#DW1560) for instructions on how to do this. The B460 board was chosen because it doesn't have a CNVi wifi module like the ROG STRIX Z490I baord for example that can't be replaced.
* CPU: Intel i5-10600
* Cooler: Noctua NH-L12 Ghost S1 Edition
* GPU: Intel UHD630
* RAM: CORSAIR VENGEANCE LPX DDR4 3000 16GB(8G×2)
* Storage: Western Digital SN750 1 TB M.2-2280 NVME
* PSU: Corsair SF 600W 80+ Platinum

## Details

### BIOS
There is no CFG-lock issue with this board.

Things I changed from default:
* Fast boot: OFF
* Virtualisation: ON
* OS type: Windows UEFI
* Enable XMP memory profile.
* I cleared the platform key as this seems to disable secure boot.

### SSDTs
Compiled by following the [Dortania's ACPI Guide](https://dortania.github.io/Getting-Started-With-ACPI/), the `.dsl` SSDT files can be found in SSDTS folder. You will need [MaciASL](https://github.com/acidanthera/MaciASL) to compile them.

* SSDT-AWAC (enable the legacy RTC clock)
* SSDT-EC-USBX (Fix embedded controller)
* SSDT-PLUG (Power management)
* SSDT-RHUB (reset USB controller)
* SSDT-SBUS-MCHC (SMBus support)

### Kexts
Download them from their official repo
* [AppleALC.kext](https://github.com/acidanthera/AppleALC) - Audio
* [FakePCIID.kext](https://github.com/RehabMan/OS-X-Fake-PCI-ID) and FakePCIID_Intel_HDMI_Audio.kext - Also needed for audio to work
* [HibernationFixup.kext](https://github.com/acidanthera/HibernationFixup) - Not sure this is needed. Sleep seems to work without it - need to experiment more to see if this enables deeper sleep.
* [IntelMausi.kext](https://github.com/acidanthera/IntelMausi) - Ethernet
* [Lilu.kext](https://github.com/acidanthera/Lilu) - Enables various patching
* [NVMeFix.kext](https://github.com/acidanthera/NVMeFix) - Better NVMe support
* [VirtualSMC](https://github.com/acidanthera/VirtualSMC) SMCProcessor.kext, SMCSuperIO.kext, VirtualSMC.kext - SMC emulator
* USB-Map.kext - Available from the kexts folder in this repo. This maps the 6 USB3 ports and the two internal ones used for Bluetooth and the Aura header. See USB section.
* [WhateverGreen.kext](https://github.com/acidanthera/WhateverGreen) - Various graphics related patches
* [XHCI-unsupported.kext](https://github.com/RehabMan/OS-X-USB-Inject-All) - There is a patched version of this in this repo. Needed for USB3 to work.

### USB
This board has two USB controllers. The Intel one that drives the 6 USB3 ports on the rear panel as well as Bluetooth and the Aura header. I'm not using the internal USB ports - so the supplied USB Map will not map these. Currently 14 ports are mapped - so there is room for one more internal port. The second controller is for the rear USB-C port - I currently don't have a USB-C device so can't test this. Looking at Hackintool it seems like a mapping might not be needed for this controller.

In addition to the USB-Map.kext you also need the modified XHCI-unsuported.kext to enable USB3 ports. The modification is to add an entry for device id 0xa3af8086. See [here](https://github.com/RehabMan/OS-X-USB-Inject-All/issues/29)

### Config.plist
bla bla

### Benchmarks
