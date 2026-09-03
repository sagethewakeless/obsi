# Installation

Package: `qemu-full`

> `qemu-base` apparently provides only headless functionality.

Make sure virtualization is enabled in the host's UEFI, as well as the `kvm` module is loaded:

```bash
lsmod | grep kvm
```

# Usage

## System Installation

Create disk image (storage):

```bash
qemu-img create -f qcow2 disk -o nocow=on 12G
```

It does:

- Create an image file with the `qcow2` format.
- Name it `disk`.
- Set `nocow` to `on` which is suggested for BTRFS.
- Set the size to 12 GiB.

Create socket file pointing at the directory to share:

```bash
/usr/lib/virtiofsd --socket-path=/tmp/vm-share.sock --shared-dir /home/sage
```

Alongside with the `virtiofsd`, run QEMU itself:

```bash
qemu-system-x86_64 \
-enable-kvm \
-cdrom /dev/sdc \
-boot menu=on \
-drive file=disk \
-vga qxl \
-display gtk \
-m 4G \
```

It does:

- Run QEMU for x86_64.
- Enable KVM.
- Set CD ROM at `/dev/sdc`.
- Enable BIOS/UEFI menu.
- Set the drive image at `./disk`.
- Set VGA backend to QXL.
- Set graphical toolkit to GTK.
- Give 4 GiB of RAM.

### (Optional) Install UEFI

Install: `edk2-ovmf`

Copy:

```bash
cp /usr/share/edk/x64/OVMF_VARS.4m.fd /home/sage/Downloads
```

Add arguments:

```bash
-drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/x64/OVMF_CODE.4m.fd \
-drive if=pflash,format=raw,file=/home/sage/Downloads/OVMF_VARS.4m.fd \
```

It does:

- Load UEFI software from `/usr/share/edk2/x64/OVMF_CODE.4m.fd`
- Load UEFI variable file from `/home/sage/Downloads/OVMF_VARS.4m.fd`

> The 1st is read-only, the 2nd one is writable.

## (Optional) Host directory access 

Install: `virtiofsd`

Add arguments:

```bash
-chardev socket,id=char0,path=/tmp/vm-share.sock \
-device vhost-user-fs-pci,chardev=char0,tag=myfs \
-object memory-backend-memfd,id=mem,size=4G,share=on \
-numa node,memdev=mem
```

It does:

- Look for socket at `/tmp/vm-share.sock` and give it ID `char0`.
- Create device with the type `vhost-user-fs-pci`, connect it to the socket `char0`, and tag it `myfs`.
- Allocate 4 GiB of RAM for the filesystem with the ID `mem`.
- Use the memory object `mem`.

%% God knows the reasoning behind the last two %%

## Finally

Install the system. Reboot.

## Post-Installation

Just remove the `cdrom` argument, so it won't load it.

For instance:

```bash
qemu-system-x86_64 \
-enable-kvm \
-boot menu=on \
-drive file=disk \
-vga qxl \
-display gtk \
-m 4G \
-drive if=pflash,format=raw,readonly=on,file=/usr/share/edk2/x64/OVMF_CODE.4m.fd \
-drive if=pflash,format=raw,file=/home/sage/Downloads/OVMF_VARS.4m.fd \
```