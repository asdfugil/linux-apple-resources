# Kernel Config

Enable:

- ARCH_APPLE
```
Platform Selection
  -> Apple Silicon SoC family
```
Enable support for Apple platforms.

- ARM64_WORKAROUND_APPLE_FUSION
```
Kernel Features
  -> ARM errata workarounds via the alternatives framework
    -> Apple Hurricane-Zephyr: Variations between physical P-core and E-core presented as a single logical core
```
Enable workarounds for Apple A10(X) Fusion.

- APPLE_PMGR_PWRSTATE
```
Device Drivers
  -> PM Domains
    -> Apple SoC PMGR power state control
```
Enable PMGR support for turning hardware blocks on and off.

- USB_DWC2
```
Device Drivers
  -> USB support
    -> DesignWare USB2 DRD Core Support
```
Enable support for the USB 2 device mode PHY found on A7-A11 SoCs.

- SERIAL_SAMSUNG
```
Device Drivers
  -> Character devices
    -> Enable TTY
      -> Serial drivers
        -> Samsung SoC serial support
```
Enable support for the Samsung serial ports found on A7-A11 SoCs.

- PINCTRL_APPLE_GPIO
```
Device Drivers
  -> Pin controllers
    -> Apple SoC GPIO pin controller driver
```
Enable support for the pin controller found on A7-A11 SoCs, 
which the buttons are wired to on A7-A10X.

For A7-A8X, enable:
- CONFIG_ARM64_4K
```
Kernel Features
  -> Page size (4KB)
```
A7-A8X supports 4K and 64K page sizes. This builds a 4K kernel.

For A9-A11, enable:
- CONFIG_ARM64_16K
```
Kernel Features
  -> Page size (16KB)
```
A9-A11 supports 4K and 64K page sizes. This builds a 16K kernel.

- CONFIG_USB_GADGETFS
- CONFIG_USB_FUNCTIONFS
- CONFIG_USB_FUNCTIONFS_ETH
- CONFIG_USB_G_MULTI
- CONFIG_USB_G_MULTI_CDC

Disable:
We only want to use the CDC multifunction gadget.

- CONFIG_USB_ETH
- CONFIG_USB_G_NCM
- CONFIG_USB_G_DBGP
- CONFIG_USB_G_HID
- CONFIG_USB_G_MULTI_RNDIS
- CONFIG_USB_G_SERIAL
- CONFIG_USB_G_NCM
- CONFIG_USB_MASS_STORAGE
- CONFIG_USB_G_PRINTER
- CONFIG_USB_CDC_COMPOSITE
- CONFIG_USB_G_ACM_MS
