# Dell Optiplex 7040 SFF OpenCore Setup

This is my configuration for the build mentioned which contains the following:
- Required Kext's for loading Ethernet, WiFi, Bluetooth, and unsupported NVME SSD's
- USB Map file, to ensure all USB ports work in USB3.0 & USB 2.0 Mode

### Known Issues  
If you put the machine to sleep, when it wakes monitors will not wake. This is a known issue, with no fix.  
I have gotten around this by disabling sleep inside macOS. Credit to [pwnsdx](https://gist.github.com/pwnsdx/2ae98341e7e5e64d32b734b871614915) 
# To disable sleep
sudo pmset -a sleep 0; sudo pmset -a hibernatemode 0; sudo pmset -a disablesleep 1;  


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
[AirportItlwm](https://github.com/OpenIntelWireless/itlwm/releases) #Enables Intel WiFi  
[AppleALC](https://github.com/acidanthera/AppleALC/releases) #Enables Audio  
[IntelBluetoothFirmware](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) #Enables Intel Bluetooth  
[IntelBluetoothInjector](https://github.com/OpenIntelWireless/IntelBluetoothFirmware/releases) #Enables Intel Bluetooth  
[IntelMausi](https://github.com/acidanthera/IntelMausi/releases) #Enables Intel Ethernet  
[Lilu](https://github.com/acidanthera/Lilu/releases)  
[NVMeFix](https://github.com/acidanthera/NVMeFix/releases/) #Enables unsupported NVMe SSD's  
[SMCDellSensors](https://github.com/acidanthera/VirtualSMC/releases)  
[SMCProcessor](https://github.com/acidanthera/VirtualSMC/releases)  
[SMCSuperIO](https://github.com/acidanthera/VirtualSMC/releases)    
[VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases)  
USBMap (included in this repo)     
[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) #Enables iGPU

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

[**Kernel**](https://dortania.github.io/OpenCore-Install-Guide/config.plist/skylake.html#kernel)   
Quirks  
-AppleXcpmCfgLock:bool: True #Not needed if CFG-Lock is disabled in the BIOS. This wasn't an option on my BIOS version.   
-DisableIoMapper:bool: True #Not needed if VT-D is disabled in the BIOS. This wasn't an option on my BIOS version.   
-PanicNoKextDump:bool: True  
-PowerTimeoutKernelPanic:bool: True  
-XhciPortLimit:bool: False  

[**Misc**](https://dortania.github.io/OpenCore-Install-Guide/config.plist/skylake.html#misc)  
Security  
-AllowNvramReset: True  
-AllowSetDefault: True  
-BlacklistAppleUpdates: True  
-SecureBootModel: Default  
-Vault: Optional  

**NVRAM**  
Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82  
-SystemAudioVolume:data: 46  
-boot-args:string: igfxonln=1  #Enables the use of both HDMI & DisplayPort ports at the same time. 
-prev-lang:kbd, String: en-US:0   

[**PlatformInfo**](https://dortania.github.io/OpenCore-Install-Guide/config.plist/skylake.html#platforminfo)  
Read the guide linked above, and use [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to generate these values below. We're using iMac17,1 for our build. 
Generic  
-MLB: **   
-SystemProductName: iMac17,1  
-SystemSerialNumber: **  
-SystemUUID: **  

If you want to enable the classic Mac boot chime, and get a pretty Bootloader, [read this first](https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html). 
Both of these items will require the latest copy of the [Resources folder](https://github.com/acidanthera/OcBinaryData), and OpenCanopy (OpenCanopy is included with the OpenCore release). 

**UEFI**  
Audio  
-AudioCodec:Number: 0  
-AudioDevice:string: PciRoot(0x0)/Pci(0x1F,0x3)  
-AudioOut:number: 0  
-AudioSupport:bool: True  
-Minimum Volume:number: 70  
-Play Chime:string: Enabled  
-ResetTrafficClass:bool: False  
-SetupDelay:number: 0  
-VolumeAmplifier:number: 143  

**Misc**  
Boot  
-PickerAttributes:Number:144  
-PickerMode:String:External  
-PickerVariant:String:Default  
-ShowPicker:True/False #Up to you, if False, PC will boot straight into MacOS, otherwise you'll see the boot menu. 

