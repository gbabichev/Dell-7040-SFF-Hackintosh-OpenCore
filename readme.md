# Dell Optiplex 7040 SFF OpenCore Setup

This is my configuration for the build mentioned which contains the following:
- Required Kext's for loading Ethernet, WiFi, Bluetooth, and unsupported NVME SSD's
- USB Map file, to ensure all USB ports work in USB3.0 & USB 2.0 Mode

### Hardware
**CPU**: Intel Core i5-6500T  
**iGPU**: Intel HD Graphics 530  
**Ethernet**: Intel 1gb Onboard   
**WiFi & BT**: Intel 8260NGW  
**NVMe SSD**: PM951 Samsung 256GB


### Required Files
**ACPI**  
[SSDT-PLUG-DRTNIA.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html)  
[SSDT-EC-USBX-DESKTOP.aml](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html)  

**Drivers**  
AudioDxe.efi (Included with OpenCore)  
[HfsPlus.efi](https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi)  
OpenCanopy.efi (Included with OpenCore)  
OpenRuntime.efi (Included with OpenCore)  

**Kexts**  
[AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases)  
[AppleALC](https://github.com/acidanthera/AppleALC/releases)  
[IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)  
[IntelBluetoothInjector](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases)  
[IntelMausi](https://github.com/acidanthera/IntelMausi/releases)  
[Lilu](https://github.com/acidanthera/Lilu/releases)  
[NVMeFix](https://github.com/acidanthera/NVMeFix/releases/)  
[SMCDellSensors](https://github.com/acidanthera/VirtualSMC/releases)  
[SMCProcessor](https://github.com/acidanthera/VirtualSMC/releases)  
[SMCSuperIO](https://github.com/acidanthera/VirtualSMC/releases)    
[VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)  
USBMap (included in this repo)     
[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases)  

### How to Build config.plist  

1. Read through the entire [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)  
2. Add the ACPI files, Drivers, and Kexts.  
3. Using the latest 'sample.plist' file from Open Core, do a Clean Snapshot using [ProperTree](https://github.com/corpnewt/ProperTree)  
4. Apply my changes:  

[**DeviceProperties**](https://dortania.github.io/OpenCore-Install-Guide/config.plist/skylake.html#deviceproperties)    
Enables the iGPU and sets the appropriate HDMI/DVI Ports  
Add -> PciRoot(0x0)/Pci(0x2,0x0)  (dict)
-AAPL,ig-platform-id:data:00001219  #Sets the iGPU to Intel HD 530, per the [WhateverGreen Patching FAQ](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md)  
-framebuffer-patch-enable:data:01000000  
-framebuffer-stolenmem:data:00003001  
-framebuffer-fbmem:data:00009000  
-framebuffer-con1-enable:data:01000000  
-framebuffer-con1-type:data:00080000  

Add -> PciRoot(0x0)/Pci(0x1F,0x3) (dict)  
-layout-id:number: 18 #This enables the built-in speaker inside the PC. Will be used for Audio, and boot-chime   

**Kernel**  
[Quirks](https://dortania.github.io/OpenCore-Install-Guide/config.plist/skylake.html#kernel)  
-AppleXcpmCfgLock:bool: True #Not needed if CFG-Lock is disabled in the BIOS. This wasn't an option on my BIOS version. 
-DisableIoMapper:bool: True #Not needed if VT-D is disabled in the BIOS. This wasn't an option on my BIOS version. 
-PanicNoKextDump:bool: True
-PowerTimeoutKernelPanic:bool: True
-XhciPortLimit:bool: False
