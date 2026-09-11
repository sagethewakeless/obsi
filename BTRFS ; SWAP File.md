Creating a SWAP file on BTRFS differs from other filesystems.

First, create a subvolume for SWAP:

```bash
sudo btrfs subvolume create /swap
```

Create the SWAP file:

```bash
sudo btrfs filesystem mkswapfile --size <value>G --uuid clear /swap/swapfile
```

Activate it:

```bash
sudo swapon /swap/swapfile
```

Add it to `/etc/fstab`:

```bash
/swap/swapfile none swap defaults 0 0
```