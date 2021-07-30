# Dell Optiplex 7040 SFF OpenCore Setup
### Hardware
**CPU**: Intel Core i5-6500T  
**iGPU**: Intel HD Graphics 530  
**Ethernet**: Intel 1gb Onboard   
**WiFi & BT**: Intel 8260NGW  
**NVMe SSD**: PM951 Samsung 256GB

This is a sample configuration that contains the following:
- Required Kext's for loading Ethernet, WiFi, Bluetooth, and unsupported NVME SSD's
- USB Map file, to ensure all USB ports work in USB3.0 & USB 2.0 Mode

### Required Files
**ACPI**  
SSDT-EC-USBX-DESKTOP.aml  
SSDT-PLUG-DRTNIA.aml  

**Drivers**  
AudioDxe.efi  
HfsPlus.efi  
OpenCanopy.efi  
OpenRuntime.efi

**Kexts**  
AirportItlwm  
AppleALC  
IntelBluetoothFirmware  
IntelBluetoothInjector  
IntelMausi  
Lilu  
NVMeFix  
SMCDellSensors  
SMCProcessor  
SMCSuperIO  
USBMap (included in this repo)  
VirtualSMC   
WhateverGreen  


