# Touchscreen Fix for Matebook14 2025 (FLMH-X) running LinuxMint

1. [Introduction](#intro)
2. [Background](#background)
3. [Settings](#settings)


<a name="intro"></a>
## Introduction

These instructions are intended for anyone who wants to have a permanently functioning touchscreen under Linux Mint on a Huawei Matebook 14 from 2025.
I have tested it on my own device and can verify that it works on devices with the specific model number FLMH-X.

## Background

The MateBook 14 (2025) touchscreen is connected via an internal Goodix/FTSC1000 I²C-HID controller that depends heavily on firmware (ACPI) information provided by the manufacturer. On this model, that firmware is primarily tailored for Windows and does not supply Linux with ideal initialization data. As a result, the Linux i2c_hid driver attempts to communicate with the touchscreen too early during boot, when the controller is not yet fully ready to respond. This leads to failed feature-report requests and incomplete device setup. Additionally, the specific FTSC1000 controller variant used in this laptop lacks a fully optimized device-specific quirk in the mainline kernel, so Linux treats it like a generic touchscreen which causes the initialization sequence to fail and prevents touch input from working properly.

## Settings

Locate your grub file under:
`/etc/default/grub`

Search for following line:
```
GRUB_CMDLINE_LINUX_DEFAULT="..."
```

Replace it with:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash i2c_hid.quirks=0x2808:0x5662:0x4 i2c_hid_acpi_force=1 hid_multitouch.ignore_special_drivers=1 i2c_hid.initial_delay=1000"
```

Save the changes.
Update your grub file with:
```
sudo update-grub
```

Restart Linux.

Try using your touchscreen. Does it work? Yes? Great! But this is just a temporary fix. 
