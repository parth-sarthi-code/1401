# Final EFI Report

This EFI was tested on an Acer Predator PHN16-71 with BIOS V1.20.

## Hardware

- CPU: Intel Core i5-13500HX
- Wi-Fi: Intel/Killer AX201 / AX1650i, PCI `8086:7A70`, subsystem `1A56:1652`
- Bluetooth: Intel USB device `8087:0026`
- Ethernet: Realtek Killer E2600, PCI `10EC:2600`
- NVMe: WD PC SN810 / Black SN850, PCI `15B7:5011`
- Touchpad: PIXA3848 I2C HID, `093A:3848`

## Changes in this EFI

- Added and enabled `HoRNDIS.kext` for Android USB RNDIS tethering during the macOS installer.
- Kept `itlwm.kext` enabled for the Intel AX201/AX1650i Wi-Fi adapter.
- Kept `IntelBluetoothFirmware.kext` and `IntelBTPatcher.kext` enabled for Bluetooth.
- Kept `RealtekRTL8111.kext` enabled; its match list includes `10EC:2600` for the Killer E2600.
- Kept `NVMeFix.kext` enabled for the WD NVMe controller.
- Disabled `VoodooPS2Mouse.kext` and `VoodooPS2Trackpad.kext` so the I2C HID touchpad stack owns the touchpad.
- Kept `VoodooI2C.kext`, `VoodooI2CHID.kext`, `VoodooGPIO.kext`, and `VoodooInput.kext` enabled.

## Verified working

- USB tethering from an Android phone was confirmed working with HoRNDIS.
- The EFI boots and the configuration parses as valid XML and plist data.
- The Intel Wi-Fi adapter is supported by the OpenIntelWireless device match list.
- The Ethernet controller is detected by Linux and matched by the included Realtek kext.
- The NVMe controller is detected by Linux without controller errors.

## Not confirmed

- macOS Wi-Fi requires `itlwm.kext` with HeliPort; it does not provide the normal native Wi-Fi menu by itself.
- macOS touchpad behavior remains unconfirmed after the PS/2 conflict cleanup.
- The NVMe disk currently contains BitLocker, NTFS, and ext4 partitions and has no APFS macOS target.
- The included RealtekRTL8111 version is 2.4.2; a newer upstream release exists but was not substituted here.
- The NVIDIA RTX 4050 is not supported natively by macOS and remains outside this EFI report.

## Rollback

The EFI deployed before HoRNDIS was backed up at `backup-before-horndis-20261021-192200` during local testing. Keep a separate copy of this final EFI before replacing a working boot stick.