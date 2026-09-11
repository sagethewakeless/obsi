Inside the working host, create a mount point:

```bash
sudo mkdir /mnt-root
```

Then mount the `@` subvolume:

```bash
sudo mount -o subvol=@ /dev/sda2 /mnt-root
```