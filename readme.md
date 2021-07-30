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


