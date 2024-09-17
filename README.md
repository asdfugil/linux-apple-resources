# linux-apple-resources

Instructions to boot Linux on A7-A11 Apple devices.

Excluded devices:
  - HomePod (not tested, so no device tree included)
  - T2 (broken bootloader)

Supported firmware versions: iOS/iPadOS/tvOS 9.0 - 18.x

## Preparing files

### Required files

- [checkra1n 1337](https://checkra.in/1337)
- [PongoOS (with bootm)](https://github.com/asdfugil/PongoOS/tree/mini)
- [Linux Kernel](https://github.com/asdfugil/linux-apple) branch `asahi`
- [m1n1-idevice](https://github.com/asdfugil/m1n1-idevice)
- [pongoterm.c](https://github.com/palera1n/PongoOS/raw/iOS15/scripts/pongoterm.c)
- An arm64 initramfs, an example is included here as well.

Prebuilt PongoOS and m1n1 binaries can be found in this repository.

### pongoterm.c compile instructions

Linux:
```
cc pongoterm.c -DUSE_LIBUSB -Os -lusb-1.0 -o pongoterm
```

macOS:
```
clang -x objective-c pongoterm.c -framework IOKit -framework CoreFoundation -framework Foundation -Os -o pongoterm
```

### Kernel config

Enable:

- CONFIG_ARCH_APPLE
```
Platform Selection
  -> Apple Silicon SoC family
```

For A7-A8, enable:
- CONFIG_ARM64_4K
```
Kernel Features
  -> Page size (4KB)
```

For A9-A11, enable:
- CONFIG_ARM64_16K
```
Kernel Features
  -> Page size (16KB)
```

When trying to run on A10(X) also enable:
- CONFIG_ARM64_WORKAROUND_APPLE_FUSION
```
Kernel Features
  -> ARM errata workarounds via the alternatives framework
    -> Apple Hurricane-Zephyr: Variations between physical P-core and E-core presented as a single logical core
```

### Prepare m1n1 blob

Assuming you are in the m1n1 source tree, the command should look like this.

```
cat build/m1n1.bin <(echo 'chosen.bootargs=<kernel command line here>')  \
	/path/to/linux-apple/arch/arm64/boot/dts/apple/*.dtb \
	/path/to/linux-apple/arch/arm64/boot/Image.gz  \
	/path/to/initramfs.gz  > m1n1-linux.bin
```

## Booting

### DFU mode

You can use [palera1n](https://github.com/palera1n/palera1n/releases) to
help you enter DFU mode. Command: `palera1n -Dl`

[Instructions](https://theapplewiki.com/wiki/DFU_Mode) to enter DFU without
palera1n.

### Booting PongoOS

Run checkra1n as root:

```
sudo /path/to/checkra1n -pEk /path/to/Pongo.bin
```

### Booting m1n1 and kernel

After booting pongoOS, run pongoterm as root:

```
printf '/send /path/to/m1n1-linux.bin\nbootm\n' | sudo pongoterm
```
