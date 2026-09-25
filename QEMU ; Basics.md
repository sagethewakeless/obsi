# Installation

Package: `qemu-full`

> `qemu-base` apparently provides headless functionality only.

Make sure virtualization is enabled in the host's UEFI, as well as the `kvm` module is loaded:

```bash
lsmod | grep kvm
```
# Usage
## System Installation For x64

Create disk image (storage):

```bash
qemu-img create -f qcow2 <disk-file-path> -o nocow=on 12G
```

It does:

- Create an image file with the `qcow2` format;
- Save it at `<disk-file-path>`;
- Set `nocow` to `on` which is suggested for BTRFS;
- Set the size to 12 GiB.

Alongside with the `virtiofsd`, run QEMU itself:

```bash
qemu-system-x86_64 \
-enable-kvm \
-cdrom <medium-path> \
-boot menu=on \
-drive file=<disk-file-path> \
-vga qxl \
-display gtk \
-m 4G
```

The `-cdrom` flag is the path to the installation image and `-drive` is the VM's drive to install the system onto.

It does:

- Run QEMU for x86_64;
- Enable KVM;
- Set the CD-ROM to the image at `<medium-path>`;
- Enable boot menu;
- Set the drive to the image at `<disk-file-path>`;
- Set VGA backend to QXL;
- Set graphical toolkit to GTK;
- Give 4 GiB of RAM.
## Post-Installation

After installing the system, shutdown the VM.

Remove the `cdrom` argument, so it won't load the installation medium.

For instance:

```bash
qemu-system-x86_64 \
-enable-kvm \
-boot menu=on \
-drive file=<disk-file-path> \
-vga qxl \
-display gtk \
-m 4G \
-drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/x64/OVMF_CODE.4m.fd \
-drive if=pflash,format=raw,file=<ovmf-vars-file-path>
```

Launch. That's it.
## ARM64

```bash
qemu-system-aarch64 \
-m 4G \
-M virt \
-kernel /mnt/iso/boot/grub/efi.img \
-drive file=/home/zz/qemu/deb-netinst/drive \
-display gtk,gl=on \
-device virtio-gpu-pci \
-drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/aarch64/QEMU_EFI.fd \
-drive if=pflash,format=raw,file=/home/zz/qemu/QEMU_VARS.fd
```
## Drafts

```bash
qemu-system-aarch64 -M virt \
-m 2G \
-drive if=pflash,file=/usr/share/AAVMF/AAVMF_CODE.fd,format=raw,readonly=on \
-drive if=pflash,file=/home/zz/qemu/deb-netinst/AAVMF_VARS.fd,format=raw \
-device virtio-scsi-pci,id=scsihw0 \
-drive file=/home/zz/qemu/deb-netinst/debian.iso,if=none,id=cdrom,format=raw,readonly=on \
-device scsi-cd,bus=scsihw0.0,drive=cdrom,bootindex=100 \
-device virtio-net-pci \
-device virtio-gpu \
-device qemu-xhci \
-display gtk
```
## Extras
### Install UEFI

Install: `edk2-ovmf`

Copy:

```bash
cp /usr/share/edk2/x64/OVMF_VARS.4m.fd <path>
```

Add arguments:

```bash
-drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/x64/OVMF_CODE.4m.fd \
-drive if=pflash,format=raw,file=<ovmf-vars-file-path>
```

It does:

- Load UEFI software from `/usr/share/edk2/x64/OVMF_CODE.4m.fd`;
- Load UEFI variable file from `<ovmf-vars-file-path>`.

> The 1st is read-only, the 2nd one is writable.
### Host Directory Access 

Install: `virtiofsd`

Create socket file pointing at the directory to share:

```bash
/usr/lib/virtiofsd --socket-path=/tmp/vm-share.sock --shared-dir <dir-path>
```

Add arguments:

```bash
-chardev socket,id=char0,path=/tmp/vm-share.sock \
-device vhost-user-fs-pci,chardev=char0,tag=myfs \
-object memory-backend-memfd,id=mem,size=4G,share=on \
-numa node,memdev=mem
```

It does:

- Look for socket at `/tmp/vm-share.sock` and give it ID `char0`;
- Create device with the type `vhost-user-fs-pci`, connect it to the socket `char0`, and tag it `myfs`;
- Allocate 4 GiB of RAM for the filesystem with the ID `mem`;
- Use the memory object `mem`.

%% God knows the reasoning behind the last two %%
### Printer Passthrough

Find out the printer's vendor and product IDs: `lsusb`

For instance:

```bash
@desk ➜ ~  lsusb
Bus 005 Device 005: ID 04b8:1143 Seiko Epson Corp. L3150 Series
```

From there:

- `Bus 005` is the bus number;
- `Device 005` is the device number.

And:

- `04b8` is the vendor ID;
- `1143` is the product ID.


Allow your user to use the printer's bus device:

```bash
sudo chmod 777 /dev/bus/usb/<bus-number>/<device-number>
```

%% ^ This part needs a proper fix, not a workaround like that %%

And add these arguments:

```bash
-device qemu-xhci,id=xhci \
-device usb-host,bus=xhci.0,vendorid=0x<vendor-id>,productid=0x<product-id>
```

%% The arguments need more explanation %%
### Launch With No Network Devices

Use: `-nic none` — Do not configure any network devices.
### QEMU Monitor
#### Launch & Exit

Press `Ctrl + Alt + 2` when the VM is running.

%% Idk how to exit it yet %%
#### Useful Commands

- `info usb`
## Hotkeys

- `Ctrl + Alt + `
	- `F` — Fullscreen;
	- `+` — Enlarge the screen;
	- `-` — Shrink the screen;
	- `0` — Reset the screen scale;
	- `G` — Toggle mouse and keyboard grab;
	- `2` — Open QEMU monitor;
