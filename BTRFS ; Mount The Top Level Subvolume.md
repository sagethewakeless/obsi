Inside the working host, create a mount point:

```bash
sudo mkdir /mnt-root
```

Then mount the top-level subvolume (always ID 5):

```bash
sudo mount -o subvolid=5 /dev/sda2 /mnt-root
```