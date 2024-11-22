# Example files

- `Pongo.bin`: pongoOS binary
- `m1n1.bin`: m1n1 binary
- `initramfs.gz`: example ramdisk

## About the example ramdisk

The example ramdisk is a modified postmarketOS ramdisk. It sets the device
as a composite USB device: CDC Ethernet + CDC Serial + Mass storage.

Access a telnet shell at 172.16.42.1 port 23.
Access a virtual serial shell, usually at `/dev/ttyACM0` in Linux,
or prefixed with `/dev/tty.usbmodem` in macOS.
View logs by inspecting the mass storage volume.

