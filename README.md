# linux-apple-resources

Instructions to boot Linux on A7-A11, T2 Apple devices.

Excluded devices:
  - HomePod (not tested, so no device tree included)

Supported firmware versions: iOS/iPadOS/tvOS 9.0 - 18.0

## Preparing files

### Required files

- [checkra1n 1337](https://checkra.in/1337)
- [PongoOS (with bootm)](https://github.com/checkra1n/PongoOS/tree/iOS15)
- [Linux Kernel](https://github.com/asdfugil/linux-apple) branch `apple`
- [m1n1-idevice](https://github.com/asdfugil/m1n1-idevice)
- [pongoterm.c](https://github.com/palera1n/PongoOS/raw/iOS15/scripts/pongoterm.c)
- An arm64 initramfs, an example is included here as well.

Prebuilt PongoOS and m1n1 binaries can be found in this repository.
Note: palera1n's pongoOS already have bootm

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

[KERNEL.md](./KERNEL.md)

To compile the kernel with clang:
```
make -j$(nproc) ARCH=arm64 LLVM=1 Image.gz dtbs
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

----

*Linux® is the registered trademark of Linus Torvalds in the U.S. and other countries.*
