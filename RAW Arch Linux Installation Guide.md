
Installation Part I: Filesystem Setup and pacstrap Installation
On host system
Activate the Arch Bootstrap Environment

    Determine the appropriate method of changing root into bootstrap image -- as there are two possible depending on the capabilities of the unshare program available on the host system -- by using the unshare command, as in:

    unshare --help

    The command with the output:

    [root:/home/brook/DataEXT4/SoftwareDownloads/arch] # unshare --help

    Usage:
     unshare [options] [<program> [<argument>...]]

    Run a program with some namespaces unshared from the parent.

    Options:
     -m, --mount[=<file>]      unshare mounts namespace
     -u, --uts[=<file>]        unshare UTS namespace (hostname etc)
     -i, --ipc[=<file>]        unshare System V IPC namespace
     -n, --net[=<file>]        unshare network namespace
     -p, --pid[=<file>]        unshare pid namespace
     -U, --user[=<file>]       unshare user namespace
     -C, --cgroup[=<file>]     unshare cgroup namespace
     -T, --time[=<file>]       unshare time namespace

     -f, --fork                fork before launching <program>
     --map-user=<uid>|<name>   map current user to uid (implies --user)
     --map-group=<gid>|<name>  map current group to gid (implies --user)
     -r, --map-root-user       map current user to root (implies --user)
     -c, --map-current-user    map current user to itself (implies --user)

     --kill-child[=<signame>]  when dying, kill the forked child (implies --fork)
                                 defaults to SIGKILL
     --mount-proc[=<dir>]      mount proc filesystem first (implies --mount)
     --propagation slave|shared|private|unchanged
                               modify mount propagation in mount namespace
     --setgroups allow|deny    control the setgroups syscall in user namespaces
     --keep-caps               retain capabilities granted in user namespaces

     -R, --root=<dir>          run the command with root directory set to <dir>
     -w, --wd=<dir>            change working directory to <dir>
     -S, --setuid <uid>        set uid in entered namespace
     -G, --setgid <gid>        set gid in entered namespace
     --monotonic <offset>      set clock monotonic offset (seconds) in time namespaces
     --boottime <offset>       set clock boottime offset (seconds) in time namespaces

     -h, --help                display this help
     -V, --version             display version

    For more details see unshare(1).

    Verify that the command options

    --fork

    and

    --pid

    are available. If these options are available on the host system's
    unshare, program as indicated by the above output, the arch-chroot command included in the bootstrap image can be used, as below. Otherwise the traditional method of changing root is used with the following series of commands:

    mount --bind ./root.x86_64 /tmp/root.x86_64
    cd /tmp/root.x86_64
    cp /etc/resolv.conf etc
    mount -t proc /proc proc
    mount --make-rslave --rbind /sys sys
    mount --make-rslave --rbind /dev dev
    mount --make-rslave --rbind /run run
    chroot /tmp/root.x86_64

    Change root to the Arch bootstrap environment using arch-chroot command included in the bootstrap image.

    [root:/home/brook/DataEXT4/SoftwareDownloads/arch] # ./root.x86_64/bin/arch-chroot /home/brook/DataEXT4/SoftwareDownloads/arch/root.x86_64/

    The command with the output:

    [root:/home/brook/DataEXT4/SoftwareDownloads/arch] # ./root.x86_64/bin/arch-chroot /home/brook/DataEXT4/SoftwareDownloads/arch/root.x86_64/
    [root@G5-openSUSE /]#

    Note the change in the prompt.

In the Arch Bootstrap Environment

After the last command executed on the host system, we are now in the Arch bootstrap image activated through the changing root command above, arch-chroot, or if this method was determined to not be appropriate for the system, through the traditional method of changing root.
Initialize pacman and Install Needed Programs in the Bootstrap Environment

    Initialize pacman keys.

    [root@G5-openSUSE /]# pacman-key --init

    The command with the output:

    [root@G5-openSUSE /]# pacman-key --init
    gpg: /etc/pacman.d/gnupg/trustdb.gpg: trustdb created
    gpg: no ultimately trusted keys found
    gpg: starting migration from earlier GnuPG versions
    gpg: porting secret keys from '/etc/pacman.d/gnupg/secring.gpg' to gpg-agent
    gpg: migration succeeded
    ==> Generating pacman master key. This may take some time.
    gpg: Generating pacman keyring master key...
    gpg: key 99ADF834B8D42BD9 marked as ultimately trusted
    gpg: directory '/etc/pacman.d/gnupg/openpgp-revocs.d' created
    gpg: revocation certificate stored as '/etc/pacman.d/gnupg/openpgp-revocs.d/4150675F845EAD056DDD09DB99ADF834B8D42BD9.rev'
    gpg: Done
    ==> Updating trust database...
    gpg: marginals needed: 3  completes needed: 1  trust model: pgp
    gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u

    Populate the pacman keyring.

    [root@G5-openSUSE /]# pacman-key --populate archlinux

    The command with the output:

    [root@G5-openSUSE /]# pacman-key --populate archlinux
    ==> Appending keys from archlinux.gpg...
    ==> Locally signing trusted keys in keyring...
    gpg: checking the trustdb
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10

    ... truncated ...

    gpg: key 1EB2638FF56C0C53: no user ID for key signature packet of class 10
    gpg: marginals needed: 3 completes needed: 1 trust model: pgp
    gpg: depth: 0 valid: 1 signed: 1 trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1 valid: 1 signed: 77 trust: 1-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2021-12-01
    gpg: checking the trustdb
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10

    ... truncated ...

    gpg: key 1EB2638FF56C0C53: no user ID for key signature packet of class 10
    gpg: key 1EB2638FF56C0C53: no user ID for key signature packet of class 10
    gpg: marginals needed: 3 completes needed: 1 trust model: pgp
    gpg: depth: 0 valid: 1 signed: 5 trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1 valid: 5 signed: 83 trust: 5-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2021-12-01
    -> Locally signed 6 keys.
    ==> Importing owner trust values...
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    gpg: setting ownertrust to 4
    gpg: inserting ownertrust of 4
    gpg: setting ownertrust to 4
    ==> Disabling revoked keys in keyring...
    gpg: checking the trustdb
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10
    gpg: key 786C63F330D7CB92: no user ID for key signature packet of class 10

    ... truncated ...

    gpg: key 1EB2638FF56C0C53: no user ID for key signature packet of class 10
    gpg: marginals needed: 3 completes needed: 1 trust model: pgp
    gpg: depth: 0 valid: 1 signed: 6 trust: 0-, 0q, 0n, 0m, 0f, 1u
    gpg: depth: 1 valid: 6 signed: 83 trust: 0-, 0q, 0n, 6m, 0f, 0u
    gpg: depth: 2 valid: 78 signed: 25 trust: 78-, 0q, 0n, 0m, 0f, 0u
    gpg: next trustdb check due at 2021-12-01

    Refresh the package database.

    [root@G5-openSUSE /]# pacman -Syy

    The command with the output:

    [root@G5-openSUSE /]# pacman -Syy
    :: Synchronizing package databases...
     core                                               137.2 KiB   252 KiB/s 00:01 [##############################################] 100%
     extra                                             1572.0 KiB  2015 KiB/s 00:01 [##############################################] 100%
     community                                            5.8 MiB  2.06 MiB/s 00:03 [##############################################] 100%

    Install needed programs into the bootstrap image.

    [root@G5-openSUSE /]# pacman -S btrfs-progs nano which tree

    (Also install
    tree, not shown in the command.) The command with the output:

    [root@G5-openSUSE /]# pacman -S btrfs-progs nano which tree
    resolving dependencies...
    looking for conflicting packages...

    Packages (5) lzo-2.10-3  btrfs-progs-5.14.1-1  nano-5.8-1  tree-1.8.0-2  which-2.21-5

    Total Download Size:   1.55 MiB
    Total Installed Size:  7.96 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...

    ... truncated ...

    :: Processing package changes...
    (1/5) installing lzo                                                            [##############################################] 100%
    (2/5) installing btrfs-progs                                                    [##############################################] 100%
    Optional dependencies for btrfs-progs
        python: libbtrfsutil python bindings
        e2fsprogs: btrfs-convert [installed]
        reiserfsprogs: btrfs-convert
    (3/5) installing nano                                                           [##############################################] 100%
    (4/5) installing which                                                          [##############################################] 100%
    (5/5) installing tree                                                           [##############################################] 100%
    :: Running post-transaction hooks...
    (1/3) Reloading system manager configuration...
      Skipped: Running in chroot.
    (2/3) Reloading device manager configuration...
      Skipped: Running in chroot.
    (3/3) Arming ConditionNeedsUpdate...

Make Filesystems

We now reformat two existing partitions (all data will be lost on these partitions) for use as our installation targets. As mentioned before, one will be used for the filesystem hierarchy at /home and the other for the filesystem hierarchy at / excluding /home.

    Make the Btrfs filesystem for /, noting the UUID in the output.

    [root@G5-openSUSE /]# mkfs.btrfs -L ARCH-B-ROOT -f -n 32k /dev/nvme0n1p7

    The
    -f option is necessary to force the creation of a new filesystem if there is an existing filesystem on the partition. The option -n 32k specifies the node size for metadata, with possible values ranging from 16k to 64k. This value represents a tradeoff compromise between the benefits and detriments of the minimum and maximum possible values. The Arch wiki page on Btrfs:

        According to man mkfs.btrfs(8) § OPTIONS, "[a] smaller node size increases fragmentation but leads to taller b-trees which in turn leads to lower locking contention. Higher node sizes give better packing and less fragmentation at the cost of more expensive memory operations while updating the metadata blocks". 

    The command with the output:

    [root@G5-openSUSE /]# mkfs.btrfs -L ARCH-B-ROOT -f -n 32k /dev/nvme0n1p7
    btrfs-progs v5.14.1 
    See http://btrfs.wiki.kernel.org for more information.

    Detected a SSD, turning off metadata duplication.  Mkfs with -m dup if you want to force metadata duplication.
    Label:              ARCH-B-ROOT
    UUID:               eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b
    Node size:          32768
    Sector size:        4096
    Filesystem size:    68.36GiB
    Block group profiles:
      Data:             single            8.00MiB
      Metadata:         single            8.00MiB
      System:           single            4.00MiB
    SSD detected:       yes
    Zoned device:       no
    Incompat features:  extref, skinny-metadata
    Runtime features:   
    Checksum:           crc32c
    Number of devices:  1
    Devices:
       ID        SIZE  PATH
        1    68.36GiB  /dev/nvme0n1p7


    Make an ext4 filesystem for /home, noting the UUID.

    [root@G5-openSUSE /]# mkfs.ext4 -L ARCH-B-HOME /dev/sda5

    The command with the output:

    [root@G5-openSUSE /]# mkfs.ext4 -L ARCH-B-HOME /dev/sda5
    mke2fs 1.46.4 (18-Aug-2021)
    /dev/sda5 contains a ext4 file system labelled 'ARCH-B-HOME'
            last mounted on /home on Sat Oct  2 23:26:04 2021
    Proceed anyway? (y,N) y
    Discarding device blocks: done                            
    Creating filesystem with 5505024 4k blocks and 1376256 inodes
    Filesystem UUID: 7d4a342f-fe14-47f4-89db-53cb7e700272
    Superblock backups stored on blocks: 
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
            4096000

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done   

    Get the UUID of the ESP of the computer.

    [root@G5-openSUSE /]# lsblk -o UUID /dev/nvme0n1p1 | grep -v UUID

    The command with the output:

    [root@G5-openSUSE /]# lsblk -o UUID /dev/nvme0n1p1 | grep -v UUID
    3E5E-D18F

    Get the UUID of swap partition.

    [root@G5-openSUSE /]# lsblk -o UUID /dev/sda8 | grep -v UUID

    The command with the output:

    [root@G5-openSUSE /]# lsblk -o UUID /dev/sda8 | grep -v UUID
    db67ee67-01d7-494e-bc12-e5c122ffe6de

Create Subvolumes

    Verify that nothing is mounted at /mnt in the bootstrap environment.

    [root@G5-openSUSE /]# mount | grep mnt

    The output should be empty.
    Mount the newly created Btrfs filesystem.

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b /mnt

    There will be no output if there are no errors. Since this is a newly created Btrfs filesystem, when mounted without options the main subvolume (
    subvolid=5, synonymous with subvolid=0 in older versions of Btrfs) is the one that is attached to the mount point /mnt. When we create other subvolumes after mounting it, it will be the parent of those subvolumes.
    Verify that the mount was successful and view the result of the mount operation.

    [root@G5-openSUSE /]# mount | grep mnt

    The command with the output:

    [root@G5-openSUSE /]# mount | grep mnt
    /dev/nvme0n1p7 on /mnt type btrfs (rw,relatime,ssd,space_cache,subvolid=5,subvol=/)

    We see that the mounted subvolume is the main subvolume created by the
    mkfs.btrfs command as part of the filesystem creation. This main subvolume always has subvolid of 5 and has the subvolume name /. These properties are mount options as are the other values in the parentheses. Had there been other subvolumes besides the default and we wanted to mount them, we would have had to specify these subvolume identifiers in the mount.
    Make the @ subvolume.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@

    The command with the output:

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@
    Create subvolume '/mnt/@'

    This subvolume will be a child of of the main subvolume (
    subvolid=5). When we create other subvolumes in it, as indicated by the path in the btrfs subvolume create command, it will be the parent of those subvolumes. The first subvolume created inside the main subvolume will always be given the subvolid of 256 (subvolid=256) and subsequently created subvolumes will increment by 1. We can verify the created subvolume with the btrfs subvolume list command, as in:

    [root@G5-openSUSE /]# btrfs subvolume list /mnt

    The command with the output:

    [root@G5-openSUSE /]# btrfs subvolume list /mnt
    ID 256 gen 8 top level 5 path @

    The output shows the subvolumes extant in the filesystem. It gives the subvolume id of the subvolumes, the subvolume id of a subvolume's parent, and the path relative to the main subvolume. The output lists only one item -- the subvolume we just created -- since there is only one subvolume at this time.(besides the main subvolume (subvolid=5) that is created at filesystem creation).
    Create a subvolume for snapshots named /.snapshots with the path /@/.snapshots. Since the filesystem is mounted at /mnt and the subvolume attached to /mnt is the main subvolume, the subvolume path /@/.snapshots is equivalent to /mnt/@/.snapshots

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/.snapshots

    Note that at this point we have made a subvolume
    .snapshots with the path /@/.snapshots but we have not made a directory named .snapshots. However if we execute

    ls -la /mnt/@

    one of the listed directories is
    .snapshots This is an important detail that illustrates that subvolumes behave like directories to a certain extent. Another related detail will be apparent after we make our next subvolume.
    Create a subvolume for the initial snapshot which will be the target of the installation. First a directory inside the /@/.snapshots subvolume (accessed as /mnt/@/.snapshots inside our chroot environment) needs to be created that conforms to Snapper's expectations, which is that there exists a directory with the same name as the snapshot number within the /@/.snapshots subvolume for each snapshot. Create the directory with:

    [root@G5-openSUSE /]# mkdir /mnt/@/.snapshots/1

    Then create the subvolume:

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/.snapshots/1/snapshot

    Create a subvolume for the filesystem hierarchy in and under /boot/grub/. This will first require a directory /boot/ to be created. Make the directory with:

    [root@G5-openSUSE /]# mkdir /mnt/@/boot

    Then create the subvolume:

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/boot/grub

    Create the /@/opt subvolume for the filesystem hierarchy under /opt. This will exclude /opt and the filesystem hierarchy beneath it to be excluded from snapshots.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/opt

    Create the /@/root subvolume for the filesystem hierarchy under /root. This will exclude /root and the filesystem hierarchy beneath it to be excluded from snapshots.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/root

    Create the /@/srv subvolume for the filesystem hierarchy under /srv. This will exclude /srv and the filesystem hierarchy beneath it to be excluded from snapshots.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/srv

    Create the /@/tmp subvolume for the filesystem hierarchy under /tmp. This will exclude /tmp and the filesystem hierarchy beneath it to be excluded from snapshots.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/tmp

    Create a subvolume for filesystem hierarchy in and under /usr/local. This will first require a directory to be created at /@/usr. First create the directory:

    [root@G5-openSUSE /]# mkdir /mnt/@/usr

    Then create the subvolume:

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/usr/local

    Create the /@/var/cache subvolume for filesystem hierarchy in and under /var/cache. This will first require a directory to be created at /@/var. where the subvolume will be created. The other subvolumes under /@/var will also be created at this location.

    [root@G5-openSUSE /]# mkdir /mnt/@/var

    Then create the subvolume

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/var/cache

    Create the /@/var/log subvolume for filesystem hierarchy in and under /var/log.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/var/log

    Create the /@/var/spool subvolume for filesystem hierarchy in and under /var/spool

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/var/spool

    Create the /@/var/tmp subvolume for filesystem hierarchy in and under /var/tmp.

    [root@G5-openSUSE /]# btrfs subvolume create /mnt/@/var/tmp

    Snapper stores metadata for each snapshot in the snapshot's directory /@/.snapshots/# where "#" represents the snapshot number in an .xml file. For our initial snapshot this will be /@/.snapshots/1 One of the metadata items is the snapshot creation time, in the format YYYY-MM-DD HH:MM:SS. The current date and time string in the appropriate format can be obtained with the command:

    [root@G5-openSUSE /]# date +"%Y-%m-%d %H:%M:%S"

    Create the metadata required by
    Snapperfor the initial installation snapshot.

    [root@G5-openSUSE /]# nano /mnt/@/.snapshots/1/info.xml

    Add the following, replacing
    2021-09-23 21:56:17 with your current time and date string.

    <?xml version="1.0"?>
    <snapshot>
    	<type>single</type>
    	<num>1</num>
    	<date>2021-09-23 21:56:17</date>
    	<description>First Root Filesystem Created at Installation</description>
    </snapshot>

    Next, we set the default subvolume to the initial installation snapshot. Remember that a snapshot is just a subvolume. The terms "snapshot" just makes it clear that it is a subvolume that is a copy of another subvolume made at a certain point in time that reflects its state at that time. Before we set new default subvolume, it is informative to get the current default subvolume before we make changes and compare it with that after we make changes. This can be done with the command btrfs subvolume get-default

    [root@G5-openSUSE /]# btrfs subvolume get-default /mnt

    The command with the output:

    [root@G5-openSUSE /]# btrfs subvolume get-default /mnt
    ID 5 (FS_TREE)

    The output indicates that the default subvolume is the one with ID 5 (
    subvolid=5). As mentioned previously, the subvolume ID of 5 is always reserved for the subvolume created at the same time as the Btrfs filesystem when the filesystem is created with the mkfs.btrfs command. Make the initial snapshot subvolume, /@/mnt/.snapshots/1/snapshot, the default subvolume.

    [root@G5-openSUSE /]# btrfs subvolume set-default $(btrfs subvolume list /mnt | grep "@/.snapshots/1/snapshot" | grep -oP '(?<=ID )[0-9]+') /mnt

    After setting the subvolume that will be the target of the initial installation's filesystem hierarchy root, running the command to get the default subvolume yields:

    [root@G5-openSUSE /]# btrfs subvolume get-default /mnt
    ID 258 gen 12 top level 257 path @/.snapshots/1/snapshot

    Now the default subvolume is the one with an ID of 258 and the subvolume path
    /@/.snapshots/1/snapshot. Now if we were to mount the Btrfs partition as we did above, without any mount options to identify the subvolume, this subvolume would be attached to /mnt instead of the main subvolume (subvolid=5).
    Optionally, enable quotas in the Btrfs filesystem.

    [root@G5-openSUSE /]# btrfs quota enable /mnt

    Quota's are required for the
    Snapper's snapshot cleanup algorithms that are based on an awareness of space on the filesystem. The Btrfs documentation does state that it is not stable, although the information may be outdated. man btrfs-qgroup also has a warning regarding btrfs qgroup.
    If quotas were enabled in the previous step, create a quota group. This will be necessary later when editing the Snapper configuration file if using Btrfs quota groups as a basis for cleaning up Snapper snapshots.

    [root@G5-openSUSE /]# btrfs qgroup create 1/0 /mnt

    Disable copy-on-write for the /@/var subvolumes this will require the nodatacow mount option, which will disable compression for these subvolumes.

    [root@G5-openSUSE /]# chattr +C /mnt/@/var/cache

    [root@G5-openSUSE /]# chattr +C /mnt/@/var/log

    [root@G5-openSUSE /]# chattr +C /mnt/@/var/spool

    [root@G5-openSUSE /]# chattr +C /mnt/@/var/tmp

    Verify the created subvolumes with the btrfs subvolume list command.

    [root@G5-openSUSE /]# btrfs subvolume list /mnt

    The command with the output:

    [root@G5-openSUSE /]# btrfs subvolume list /mnt
    ID 256 gen 22 top level 5 path @
    ID 257 gen 23 top level 256 path @/.snapshots
    ID 258 gen 10 top level 257 path @/.snapshots/1/snapshot
    ID 259 gen 12 top level 256 path @/boot/grub
    ID 260 gen 13 top level 256 path @/opt
    ID 261 gen 14 top level 256 path @/root
    ID 262 gen 15 top level 256 path @/srv
    ID 263 gen 16 top level 256 path @/tmp
    ID 264 gen 17 top level 256 path @/usr/local
    ID 265 gen 26 top level 256 path @/var/cache
    ID 266 gen 26 top level 256 path @/var/log
    ID 267 gen 27 top level 256 path @/var/spool
    ID 268 gen 27 top level 256 path @/var/tmp

    Note that the main subvolume is mounted at
    /mnt and it is not shown in the list. The first subvolume created (ID 256) is a child of the main subvolume (ID 5) as indicated by the "top level 5" The top level ... in the remaining output lines similarly show that each subvolume is a child of the /@ subvolume (ID 256), except the /@/.snapshots/1/snapshot (ID 258) which is a child of the /@/.snapshots subvolume (ID 257). Compare the subvolume list to the output of the directory listings at /mnt, where the main subvolume is attached and at /mnt/@ whcih is the first subvolume created under the main subvolume, as shown in the following listing.

    [root@G5-openSUSE /]# ls -la /mnt
    total 36
    drwxr-xr-x  1 root root    2 Oct  3 08:12 .
    drwxr-xr-x 17 root root 4096 Oct  3 08:21 ..
    drwxr-xr-x  1 root root   66 Oct  3 08:33 @
    [root@G5-openSUSE /]# ls -la /mnt/@
    total 32
    drwxr-xr-x 1 root root 66 Oct  3 08:33 .
    drwxr-xr-x 1 root root  2 Oct  3 08:12 ..
    drwxr-xr-x 1 root root  2 Oct  3 08:22 .snapshots
    drwxr-xr-x 1 root root  8 Oct  3 08:29 boot
    drwxr-xr-x 1 root root  0 Oct  3 08:31 opt
    drwxr-xr-x 1 root root  0 Oct  3 08:31 root
    drwxr-xr-x 1 root root  0 Oct  3 08:31 srv
    drwxr-xr-x 1 root root  0 Oct  3 08:32 tmp
    drwxr-xr-x 1 root root 10 Oct  3 08:32 usr
    drwxr-xr-x 1 root root 32 Oct  3 08:33 var

    Unmount the Btrfs filesystem.

    [root@G5-openSUSE /]# umount /mnt

    Verify that the Btrfs filesystem is unmounted.

    [root@G5-openSUSE /]# mount | grep mnt

    The output should be empty.

Mount the Btrfs Filesystem

Mount the Btrfs filesystem again. Unlike the previous time, when we mount it this time, since we have set the default subvolume, the subvolume attached to /mnt will be the subvolume @/.snapshots/1/snapshot with a subvolume ID of 258 and not the main subvolume with a subvolume ID of 5.

[root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o compress=zstd /mnt

We can verify the mounted subvolume with:

[root@G5-openSUSE /]# mount | grep /mnt

The command with the output:

[root@G5-openSUSE /]# mount | grep mnt
/dev/nvme0n1p7 on /mnt type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=258,subvol=/@/.snapshots/1/snapshot)

As expected the mounted subvolume is the subvolume set as default previously, the subvolume that will act as an initial snapshot which will be the target of our installation's root filesystem hierarchy. Note that when we mounted the filesystem the option compress=zstd is used, causing files written to the partition to be compressed in the zstd compression format. Also note that, as indicated in the output to mount | grep mnt, the default zstd compression level was automatically set by mount to the default level of 3 from a possible range of 1 to 15, where a level of 1 compresses files the least, but has the lowest overhead for the compression operations in terms of memory and CPU resources, and a compression level of 15 compresses files the most but has the highest overhead costs in terms of memory and CPU resource use. Also as indicated in the output other options were automatically set by mount including the option ssd which tailors operation of the filesystem to the automatically detected SSD.
Make Mountpoints for Subvolumes

The previously created directories @/.snapshots/1, @/usr, @/boot/grub when the main subvolume (subvolid=5) was mounted at /mnt only exist within that subvolume and were only created to make subvolumes within it. They do not exist in the subvolume currently mounted at /mnt (subvolid=258, @/.snapshots/1/snapshot). We can verify that this is true with:

[root@G5-openSUSE /]# ls -la /mnt

The output is empty:

[root@G5-openSUSE /]# ls -la /mnt
total 4
drwxr-xr-x  1 root root    0 Oct  3 08:23 .
drwxr-xr-x 17 root root 4096 Oct  3 08:21 ..

So we need to make mountpoints for the subvolumes to be mounted at hierarchy locations under / for our installation process and for when this subvolume (subvolid=258) is mounted at / in our installed system.

We will also make mountpoints for the partition for home and the ESP partition.

    Make a mount point for the @/.snapshots subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/.snapshots

    Make a mount point for the @/boot/grub subvolume. Since /boot does not exist in the current mounted subvolume, the -p option is used to make it while making /boot/grub.

    [root@G5-openSUSE /]# mkdir -p /mnt/boot/grub

    Make a mount point for the @/opt subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/opt

    Make a mount point for the @/root subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/root

    Make a mount point for the @/srv subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/srv

    Make a mount point for the @/tmp subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/tmp

    Make a mount point for the @/usr/local subvolume.

    [root@G5-openSUSE /]# mkdir -p /mnt/usr/local

    Make a mount point for the @/var/cache subvolume.

    [root@G5-openSUSE /]# mkdir -p /mnt/var/cache

    Make a mount point for the @/var/log subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/var/log

    Make a mount point for the @/var/spool subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/var/spool

    Make a mount point for the @/var/tmp subvolume.

    [root@G5-openSUSE /]# mkdir /mnt/var/tmp

    Make a mountpoint for the ESP (EFI System Partition).

    [root@G5-openSUSE /]# mkdir /mnt/efi

    Make a mount point for the /home partition.

    [root@G5-openSUSE /]# mkdir /mnt/home

    Verify that all of the mountpoints were created successfully by using the tree command. The command with the output:

    [root@G5-openSUSE /]# tree -L 3 /mnt
    /mnt
    |-- boot
    |   `-- grub
    |-- efi
    |-- home
    |-- opt
    |-- root
    |-- srv
    |-- tmp
    |-- usr
    |   `-- local
    `-- var
        |-- cache
        |-- log
        |-- spool
        `-- tmp

    15 directories, 0 files

Mount the Subvolumes and Partitions

We now mount the subvolumes to the mount points. When we mount the subvolumes now we are using the subvolume names as subvolume identifiers in the mount options (e.g. subvol=@/var/cache). The subvolume ID could have been used instead of, or in addition to the subvolume name (e.g. subvolid=265,subvol=@/var/cache). We also specify the compress=zstd option for all subvolumes and the nodatacow for the @/var/XXX subvolumes.

    Mount the @/.snapshots subvolume.

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/.snapshots,compress=zstd /mnt/.snapshots 

    Mount the @/boot/grub subvolume.

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/boot/grub,compress=zstd /mnt/boot/grub

    Mount the @/opt subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/opt,compress=zstd /mnt/opt

    Mount the @/root subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/root,compress=zstd /mnt/root

    Mount the @/srv subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/srv,compress=zstd /mnt/srv

    Mount the @/tmp subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/tmp,compress=zstd /mnt/tmp

    Mount the @/usr/local subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/usr/local,compress=zstd /mnt/usr/local

    Mount the @/var/cache subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/var/cache,nodatacow /mnt/var/cache

    Mount the @/var/log subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/var/log,nodatacow /mnt/var/log

    Mount the @/var/spool subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/var/spool,nodatacow /mnt/var/spool

    Mount the @/var/tmp subvolume

    [root@G5-openSUSE /]# mount UUID=eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b -o subvol=@/var/tmp,nodatacow /mnt/var/tmp

    Mount the ESP partition.

    [root@G5-openSUSE /]# mount UUID=3E5E-D18F /mnt/efi

    Mount the partition for /home.

    [root@G5-openSUSE /]# mount UUID=7d4a342f-fe14-47f4-89db-53cb7e700272 /mnt/home

    Activate the swap partition.

    [root@G5-openSUSE /]# swapon UUID=db67ee67-01d7-494e-bc12-e5c122ffe6de

pacstrap Installation

Now that our Btrfs filesystem has been configured and mounted, as well as our other partitions for /home, the ESP, and the swap partition has been activated, we our ready to install using the pacstrap command included in the bootstrap environment. Below, we install various sets of packages with separate invocations of pacstrap, but all desired packages can be installed in one pacstrap invocation. Also, if using separate pacstrap invocations all of the desired optional dependencies from those indicated in the output could be installed at the same time with one last pacstrap invocation instead of a series of two pacstrap commands, where the first installs the desired packages and the second installs the desired optional dependencies of packages installed in the first command.

    Install the base system, the kernel, the processor microcode, firmware, filesystem utilities, including those necessary for Btrfs, sudo, and a text editor.

    [root@G5-openSUSE /]# pacstrap /mnt base linux linux-firmware intel-ucode btrfs-progs ntfs-3g sudo nano

    Note the warnings, notifications of optional dependencies, so we can address them later. The command with the output

    [root@G5-openSUSE /]# pacstrap /mnt base linux linux-lts linux-firmware intel-ucode btrfs-progs ntfs-3g sudo nano
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core                                               137.3 KiB   140 KiB/s 00:01 [##############################################] 100%
     extra                                             1572.2 KiB  2.02 MiB/s 00:01 [##############################################] 100%
     community                                            5.8 MiB  9.61 MiB/s 00:01 [##############################################] 100%
    resolving dependencies...
    :: There are 3 providers available for initramfs:
    :: Repository core
       1) mkinitcpio
    :: Repository extra
       2) booster  3) dracut

    Enter a number (default=1): 
    looking for conflicting packages...

    Packages (123) acl-2.3.1-1  archlinux-keyring-20210902-1  argon2-20190702-3  attr-2.5.1-1  audit-3.0.5-1  bash-5.1.008-1
                   brotli-1.0.9-4  bzip2-1.0.8-4  ca-certificates-20210603-1  ca-certificates-mozilla-3.70-1
                   ca-certificates-utils-20210603-1  coreutils-9.0-2  cryptsetup-2.4.1-1  curl-7.79.1-1  dbus-1.12.20-1
                   device-mapper-2.03.13-1  diffutils-3.8-1  e2fsprogs-1.46.4-1  expat-2.4.1-1  file-5.40-6  filesystem-2021.05.31-1
                   findutils-4.8.0-1  fuse-common-3.10.5-1  fuse2-2.9.9-4  gawk-5.1.0-1  gcc-libs-11.1.0-1  gettext-0.21-1
                   glib2-2.70.0-1  glibc-2.33-5  gmp-6.2.1-1  gnupg-2.2.29-1  gnutls-3.7.2-2  gpgme-1.16.0-1  grep-3.7-1  gzip-1.11-1
                   hwids-20210613-1  iana-etc-20210903-1  icu-69.1-1  iproute2-5.14.0-1  iptables-1:1.8.7-1  iputils-20210722-1
                   json-c-0.15-1  kbd-2.4.0-2  keyutils-1.6.3-1  kmod-29-1  krb5-1.19.2-1  less-1:590-1  libarchive-3.5.2-1
                   libassuan-2.5.5-1  libcap-2.58-1  libcap-ng-0.8.2-3  libcroco-0.6.13-2  libelf-0.185-1  libffi-3.3-4
                   libgcrypt-1.9.4-1  libgpg-error-1.42-1  libidn2-2.3.2-1  libksba-1.6.0-1  libldap-2.4.59-2  libmnl-1.0.4-3
                   libnetfilter_conntrack-1.0.8-1  libnfnetlink-1.0.1-4  libnftnl-1.2.0-1  libnghttp2-1.45.0-1  libnl-3.5.0-3
                   libp11-kit-0.24.0-1  libpcap-1.10.1-1  libpsl-0.21.1-1  libsasl-2.1.27-3  libseccomp-2.5.2-1  libsecret-0.20.4-1
                   libssh2-1.10.0-1  libtasn1-4.17.0-1  libtirpc-1.3.2-1  libunistring-0.9.10-3  libxcrypt-4.4.26-1  libxml2-2.9.12-2
                   licenses-20200427-1  linux-api-headers-5.12.3-1  lz4-1:1.9.3-2  lzo-2.10-3  mkinitcpio-30-2
                   mkinitcpio-busybox-1.33.1-1  mpfr-4.1.0.p13-1  ncurses-6.2-2  nettle-3.7.3-1  npth-1.6-3  openssl-1.1.1.l-1
                   p11-kit-0.24.0-1  pacman-6.0.1-2  pacman-mirrorlist-20210822-1  pam-1.5.2-1  pambase-20210605-2  pciutils-3.7.0-1
                   pcre-8.45-1  pcre2-10.37-1  pinentry-1.1.1-1  popt-1.18-1  procps-ng-3.3.17-1  psmisc-23.4-1  readline-8.1.001-1
                   sed-4.8-1  shadow-4.8.1-4  sqlite-3.36.0-1  systemd-249.4-1  systemd-libs-249.4-1  systemd-sysvcompat-249.4-1
                   tar-1.34-1  tzdata-2021b-1  util-linux-2.37.2-1  util-linux-libs-2.37.2-1  xz-5.2.5-2  zlib-1:1.2.11-4  zstd-1.5.0-1
                   base-2-2  btrfs-progs-5.14.1-1  intel-ucode-20210608-1  linux-5.14.8.arch1-1  linux-firmware-20210919.d526e04-1
                   linux-lts-5.10.70-1  nano-5.8-1  ntfs-3g-2021.8.22-1  sudo-1.9.8.p2-1

    Total Download Size:    501.63 MiB
    Total Installed Size:  1408.95 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...

    ... truncated ...

    :: Processing package changes...
    (  1/123) installing iana-etc                                                   [##############################################] 100%
    (  2/123) installing filesystem                                                 [##############################################] 100%
    warning: directory permissions differ on /mnt/root/
    filesystem: 755  package: 750
    warning: directory permissions differ on /mnt/var/tmp/
    filesystem: 755  package: 1777
    (  3/123) installing linux-api-headers                                          [##############################################] 100%
    (  4/123) installing tzdata                                                     [##############################################] 100%
    (  5/123) installing glibc                                                      [##############################################] 100%
    Optional dependencies for glibc
        gd: for memusagestat
    (  6/123) installing gcc-libs                                                   [##############################################] 100%
    (  7/123) installing ncurses                                                    [##############################################] 100%
    Optional dependencies for ncurses
        bash: for ncursesw6-config [pending]
    (  8/123) installing readline                                                   [##############################################] 100%
    (  9/123) installing bash                                                       [##############################################] 100%
    Optional dependencies for bash
        bash-completion: for tab completion
    ( 10/123) installing attr                                                       [##############################################] 100%
    ( 11/123) installing acl                                                        [##############################################] 100%
    ( 12/123) installing gmp                                                        [##############################################] 100%
    ( 13/123) installing util-linux-libs                                            [##############################################] 100%
    ( 14/123) installing e2fsprogs                                                  [##############################################] 100%
    ( 15/123) installing openssl                                                    [##############################################] 100%
    Optional dependencies for openssl
        ca-certificates [pending]
        perl
    ( 16/123) installing libsasl                                                    [##############################################] 100%
    ( 17/123) installing libldap                                                    [##############################################] 100%
    ( 18/123) installing keyutils                                                   [##############################################] 100%
    ( 19/123) installing krb5                                                       [##############################################] 100%
    ( 20/123) installing libtirpc                                                   [##############################################] 100%
    ( 21/123) installing pambase                                                    [##############################################] 100%
    ( 22/123) installing libcap-ng                                                  [##############################################] 100%
    ( 23/123) installing audit                                                      [##############################################] 100%
    ( 24/123) installing libxcrypt                                                  [##############################################] 100%
    ( 25/123) installing pam                                                        [##############################################] 100%
    ( 26/123) installing libcap                                                     [##############################################] 100%
    ( 27/123) installing coreutils                                                  [##############################################] 100%
    ( 28/123) installing zlib                                                       [##############################################] 100%
    ( 29/123) installing xz                                                         [##############################################] 100%
    ( 30/123) installing bzip2                                                      [##############################################] 100%
    ( 31/123) installing libseccomp                                                 [##############################################] 100%
    ( 32/123) installing file                                                       [##############################################] 100%
    ( 33/123) installing findutils                                                  [##############################################] 100%
    ( 34/123) installing mpfr                                                       [##############################################] 100%
    ( 35/123) installing gawk                                                       [##############################################] 100%
    ( 36/123) installing pcre                                                       [##############################################] 100%
    ( 37/123) installing grep                                                       [##############################################] 100%
    ( 38/123) installing libgpg-error                                               [##############################################] 100%
    ( 39/123) installing libgcrypt                                                  [##############################################] 100%
    ( 40/123) installing libtasn1                                                   [##############################################] 100%
    ( 41/123) installing libffi                                                     [##############################################] 100%
    ( 42/123) installing libp11-kit                                                 [##############################################] 100%
    ( 43/123) installing lz4                                                        [##############################################] 100%
    ( 44/123) installing zstd                                                       [##############################################] 100%
    ( 45/123) installing systemd-libs                                               [##############################################] 100%
    ( 46/123) installing procps-ng                                                  [##############################################] 100%
    ( 47/123) installing sed                                                        [##############################################] 100%
    ( 48/123) installing tar                                                        [##############################################] 100%
    ( 49/123) installing glib2                                                      [##############################################] 100%
    Optional dependencies for glib2
        python: gdbus-codegen, glib-genmarshal, glib-mkenums, gtester-report
        libelf: gresource inspection tool [pending]
    ( 50/123) installing libunistring                                               [##############################################] 100%
    ( 51/123) installing icu                                                        [##############################################] 100%
    ( 52/123) installing libxml2                                                    [##############################################] 100%
    ( 53/123) installing libcroco                                                   [##############################################] 100%
    ( 54/123) installing gettext                                                    [##############################################] 100%
    Optional dependencies for gettext
        git: for autopoint infrastructure updates
    ( 55/123) installing hwids                                                      [##############################################] 100%
    ( 56/123) installing kmod                                                       [##############################################] 100%
    ( 57/123) installing pciutils                                                   [##############################################] 100%
    ( 58/123) installing psmisc                                                     [##############################################] 100%
    ( 59/123) installing shadow                                                     [##############################################] 100%
    ( 60/123) installing util-linux                                                 [##############################################] 100%
    Optional dependencies for util-linux
        python: python bindings to libmount
        words: default dictionary for look
    ( 61/123) installing pcre2                                                      [##############################################] 100%
    ( 62/123) installing less                                                       [##############################################] 100%
    ( 63/123) installing gzip                                                       [##############################################] 100%
    ( 64/123) installing licenses                                                   [##############################################] 100%
    ( 65/123) installing expat                                                      [##############################################] 100%
    ( 66/123) installing libarchive                                                 [##############################################] 100%
    ( 67/123) installing p11-kit                                                    [##############################################] 100%
    ( 68/123) installing ca-certificates-utils                                      [##############################################] 100%
    ( 69/123) installing ca-certificates-mozilla                                    [##############################################] 100%
    ( 70/123) installing ca-certificates                                            [##############################################] 100%
    ( 71/123) installing brotli                                                     [##############################################] 100%
    ( 72/123) installing libidn2                                                    [##############################################] 100%
    ( 73/123) installing libnghttp2                                                 [##############################################] 100%
    ( 74/123) installing libpsl                                                     [##############################################] 100%
    ( 75/123) installing libssh2                                                    [##############################################] 100%
    ( 76/123) installing curl                                                       [##############################################] 100%
    ( 77/123) installing npth                                                       [##############################################] 100%
    ( 78/123) installing libksba                                                    [##############################################] 100%
    ( 79/123) installing libassuan                                                  [##############################################] 100%
    ( 80/123) installing libsecret                                                  [##############################################] 100%
    Optional dependencies for libsecret
        org.freedesktop.secrets: secret storage backend
    ( 81/123) installing pinentry                                                   [##############################################] 100%
    Optional dependencies for pinentry
        gtk2: gtk2 backend
        qt5-base: qt backend
        gcr: gnome3 backend
    ( 82/123) installing nettle                                                     [##############################################] 100%
    ( 83/123) installing gnutls                                                     [##############################################] 100%
    Optional dependencies for gnutls
        guile: for use with Guile bindings
    ( 84/123) installing sqlite                                                     [##############################################] 100%
    ( 85/123) installing gnupg                                                      [##############################################] 100%
    Optional dependencies for gnupg
        libldap: gpg2keys_ldap [installed]
        libusb-compat: scdaemon
        pcsclite: scdaemon
    ( 86/123) installing gpgme                                                      [##############################################] 100%
    ( 87/123) installing pacman-mirrorlist                                          [##############################################] 100%
    ( 88/123) installing archlinux-keyring                                          [##############################################] 100%
    ( 89/123) installing pacman                                                     [##############################################] 100%
    Optional dependencies for pacman
        perl-locale-gettext: translation support in makepkg-template
    ( 90/123) installing device-mapper                                              [##############################################] 100%
    ( 91/123) installing popt                                                       [##############################################] 100%
    ( 92/123) installing json-c                                                     [##############################################] 100%
    ( 93/123) installing argon2                                                     [##############################################] 100%
    ( 94/123) installing cryptsetup                                                 [##############################################] 100%
    ( 95/123) installing dbus                                                       [##############################################] 100%
    ( 96/123) installing libmnl                                                     [##############################################] 100%
    ( 97/123) installing libnftnl                                                   [##############################################] 100%
    ( 98/123) installing libnl                                                      [##############################################] 100%
    ( 99/123) installing libpcap                                                    [##############################################] 100%
    (100/123) installing libnfnetlink                                               [##############################################] 100%
    (101/123) installing libnetfilter_conntrack                                     [##############################################] 100%
    (102/123) installing iptables                                                   [##############################################] 100%
    (103/123) installing kbd                                                        [##############################################] 100%
    (104/123) installing libelf                                                     [##############################################] 100%
    (105/123) installing systemd                                                    [##############################################] 100%
    Initializing machine ID from random generator.
    Creating group sys with gid 3.
    Creating group mem with gid 8.
    Creating group ftp with gid 11.
    Creating group log with gid 19.
    Creating group smmsp with gid 999.
    Creating group proc with gid 26.
    Creating group games with gid 50.
    Creating group network with gid 90.
    Creating group floppy with gid 94.
    Creating group scanner with gid 96.
    Creating group power with gid 98.
    Creating group adm with gid 998.
    Creating group optical with gid 997.
    Creating group storage with gid 996.
    Creating group uucp with gid 995.
    Creating group rfkill with gid 994.
    Creating user ftp (n/a) with uid 14 and gid 11.
    Creating group http with gid 33.
    Creating user http (n/a) with uid 33 and gid 33.
    Creating group dbus with gid 81.
    Creating user dbus (System Message Bus) with uid 81 and gid 81.
    Creating group systemd-journal-remote with gid 993.
    Creating user systemd-journal-remote (systemd Journal Remote) with uid 993 and gid 993.
    Creating group systemd-oom with gid 992.
    Creating user systemd-oom (systemd Userspace OOM Killer) with uid 992 and gid 992.
    Creating group uuidd with gid 68.
    Creating user uuidd (n/a) with uid 68 and gid 68.
    Created symlink /etc/systemd/system/getty.target.wants/getty@tty1.service → /usr/lib/systemd/system/getty@.service.
    Created symlink /etc/systemd/system/multi-user.target.wants/remote-fs.target → /usr/lib/systemd/system/remote-fs.target.
    :: Append 'init=/usr/lib/systemd/systemd' to your kernel command line in your
       bootloader to replace sysvinit with systemd, or install systemd-sysvcompat
    chgrp: invalid group: 'systemd-journal-remote'
    error: command failed to execute correctly
    Optional dependencies for systemd
        libmicrohttpd: remote journald capabilities
        quota-tools: kernel-level quota management
        systemd-sysvcompat: symlink package to provide sysvinit binaries [pending]
        polkit: allow administration as unprivileged user
        curl: machinectl pull-tar and pull-raw [installed]
        libfido2: unlocking LUKS2 volumes with FIDO2 token
        tpm2-tss: unlocking LUKS2 volumes with TPM2
    (106/123) installing systemd-sysvcompat                                         [##############################################] 100%
    (107/123) installing iputils                                                    [##############################################] 100%
    (108/123) installing iproute2                                                   [##############################################] 100%
    Optional dependencies for iproute2
        db: userspace arp daemon
        libcap: tipc [installed]
        linux-atm: ATM support
    (109/123) installing base                                                       [##############################################] 100%
    Optional dependencies for base
        linux: bare metal support [pending]
    (110/123) installing mkinitcpio-busybox                                         [##############################################] 100%
    (111/123) installing diffutils                                                  [##############################################] 100%
    (112/123) installing mkinitcpio                                                 [##############################################] 100%
    Optional dependencies for mkinitcpio
        gzip: Use gzip compression for the initramfs image [installed]
        xz: Use lzma or xz compression for the initramfs image [installed]
        bzip2: Use bzip2 compression for the initramfs image [installed]
        lzop: Use lzo compression for the initramfs image
        lz4: Use lz4 compression for the initramfs image [installed]
        mkinitcpio-nfs-utils: Support for root filesystem on NFS
    (113/123) installing linux                                                      [##############################################] 100%
    Optional dependencies for linux
        crda: to set the correct wireless channels of your country
        linux-firmware: firmware images needed for some devices [pending]
    (114/123) installing linux-lts                                                  [##############################################] 100%
    Optional dependencies for linux-lts
        crda: to set the correct wireless channels of your country
        linux-firmware: firmware images needed for some devices [pending]
    (115/123) installing linux-firmware                                             [##############################################] 100%
    (116/123) installing intel-ucode                                                [##############################################] 100%
    (117/123) installing lzo                                                        [##############################################] 100%
    (118/123) installing btrfs-progs                                                [##############################################] 100%
    Optional dependencies for btrfs-progs
        python: libbtrfsutil python bindings
        e2fsprogs: btrfs-convert [installed]
        reiserfsprogs: btrfs-convert
    (119/123) installing fuse-common                                                [##############################################] 100%
    (120/123) installing fuse2                                                      [##############################################] 100%
    (121/123) installing ntfs-3g                                                    [##############################################] 100%
    (122/123) installing sudo                                                       [##############################################] 100%
    (123/123) installing nano                                                       [##############################################] 100%
    :: Running post-transaction hooks...
    ( 1/12) Creating system user accounts...
    ( 2/12) Updating journal message catalog...
    ( 3/12) Reloading system manager configuration...
      Skipped: Running in chroot.
    ( 4/12) Updating udev hardware database...
    ( 5/12) Applying kernel sysctl settings...
      Skipped: Running in chroot.
    ( 6/12) Creating temporary files...
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to open file "/sys/devices/system/cpu/microcode/reload": Read-only file system
    error: command failed to execute correctly
    ( 7/12) Reloading device manager configuration...
      Skipped: Running in chroot.
    ( 8/12) Arming ConditionNeedsUpdate...
    ( 9/12) Rebuilding certificate stores...
    (10/12) Updating module dependencies...
    (11/12) Updating linux initcpios...
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'default'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts.img
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'fallback'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts-fallback.img -S autodetect
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts-fallback.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'default'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux.img
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'fallback'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-fallback.img -S autodetect
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-fallback.img
    ==> Image generation successful
    (12/12) Reloading system bus configuration...
      Skipped: Running in chroot.

    Install booting related packages.

    [root@G5-openSUSE /]# pacstrap /mnt grub grub-btrfs os-prober efibootmgr

    The command with the output:

    [root@G5-openSUSE /]# pacstrap /mnt grub grub-btrfs os-prober efibootmgr                                                   
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community is up to date
    resolving dependencies...
    looking for conflicting packages...

    Packages (5) efivar-37-4  efibootmgr-17-2  grub-2:2.06-2  grub-btrfs-4.10.1-1  os-prober-1.79-1

    Total Download Size:    6.93 MiB
    Total Installed Size:  34.57 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...
     grub-2:2.06-2-x86_64                                 6.8 MiB   886 KiB/s 00:08 [##############################################] 100%
     grub-btrfs-4.10.1-1-any                             24.3 KiB   108 KiB/s 00:00 [##############################################] 100%
     os-prober-1.79-1-x86_64                             17.4 KiB   125 KiB/s 00:00 [##############################################] 100%
     efivar-37-4-x86_64                                 110.5 KiB   453 KiB/s 00:00 [##############################################] 100%
     efibootmgr-17-2-x86_64                              27.4 KiB   180 KiB/s 00:00 [##############################################] 100%
     Total (5/5)                                          6.9 MiB   754 KiB/s 00:09 [##############################################] 100%
    (5/5) checking keys in keyring                                                  [##############################################] 100%
    (5/5) checking package integrity                                                [##############################################] 100%
    (5/5) loading package files                                                     [##############################################] 100%
    (5/5) checking for file conflicts                                               [##############################################] 100%
    (5/5) checking available disk space                                             [##############################################] 100%
    :: Processing package changes...
    (1/5) installing grub                                                           [##############################################] 100%
    :: Generate your bootloader configuration with:
         grub-mkconfig -o /boot/grub/grub.cfg
    Optional dependencies for grub
        freetype2: For grub-mkfont usage
        fuse2: For grub-mount usage [installed]
        dosfstools: For grub-mkrescue FAT FS and EFI support
        efibootmgr: For grub-install EFI support [pending]
        libisoburn: Provides xorriso for generating grub rescue iso using grub-mkrescue
        os-prober: To detect other OSes when generating grub.cfg in BIOS systems [pending]
        mtools: For grub-mkrescue FAT FS support
    (2/5) installing grub-btrfs                                                     [##############################################] 100%
    Optional dependencies for grub-btrfs
        snapper: Snapper support
    (3/5) installing os-prober                                                      [##############################################] 100%
    (4/5) installing efivar                                                         [##############################################] 100%
    (5/5) installing efibootmgr                                                     [##############################################] 100%
    :: Running post-transaction hooks...
    (1/3) Reloading system manager configuration...
      Skipped: Running in chroot.
    (2/3) Arming ConditionNeedsUpdate...
    (3/3) Updating linux initcpios...
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'default'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts.img
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'fallback'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts-fallback.img -S autodetect
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts-fallback.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'default'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux.img
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'fallback'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-fallback.img -S autodetect
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-fallback.img
    ==> Image generation successful

    Install snapper and snap-pac, which automatically makes Snapper snapshots after package manager transactions. There will also be another package to install from the AUR, snap-pac-btrfs the second time we reboot into our installed system. Another snapper related program, snap-sync, for making backups to an external drive from a snapshot is also available from the regular Arch repositories for users who need its functionality.

    [root@G5-openSUSE /]# pacstrap /mnt snapper snap-pac

    The command with its output:

    [root@G5-openSUSE /]# pacstrap /mnt snapper snap-pac
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community is up to date
    resolving dependencies...
    looking for conflicting packages...

    Packages (6) boost-libs-1.76.0-1  gdbm-1.21-1  libnsl-2.0.0-1  python-3.9.7-1  snap-pac-3.0.1-1  snapper-0.9.1-1

    Total Download Size:   14.32 MiB
    Total Installed Size:  65.74 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...
     boost-libs-1.76.0-1-x86_64                           2.3 MiB  2.53 MiB/s 00:01 [##############################################] 100%
     snapper-0.9.1-1-x86_64                             760.3 KiB  2.61 MiB/s 00:00 [##############################################] 100%
     gdbm-1.21-1-x86_64                                 227.6 KiB   849 KiB/s 00:00 [##############################################] 100%
     libnsl-2.0.0-1-x86_64                               21.8 KiB  99.0 KiB/s 00:00 [##############################################] 100%
     python-3.9.7-1-x86_64                               11.0 MiB  10.6 MiB/s 00:01 [##############################################] 100%
     snap-pac-3.0.1-1-any                                18.6 KiB   133 KiB/s 00:00 [##############################################] 100%
     Total (6/6)                                         14.3 MiB  4.00 MiB/s 00:04 [##############################################] 100%
    (6/6) checking keys in keyring                                                  [##############################################] 100%
    (6/6) checking package integrity                                                [##############################################] 100%
    (6/6) loading package files                                                     [##############################################] 100%
    (6/6) checking for file conflicts                                               [##############################################] 100%
    (6/6) checking available disk space                                             [##############################################] 100%
    :: Processing package changes...
    (1/6) installing boost-libs                                                     [##############################################] 100%
    Optional dependencies for boost-libs
        openmpi: for mpi support
    (2/6) installing snapper                                                        [##############################################] 100%
    Optional dependencies for snapper
        pam: pam_snapper [installed]
    (3/6) installing gdbm                                                           [##############################################] 100%
    (4/6) installing libnsl                                                         [##############################################] 100%
    (5/6) installing python                                                         [##############################################] 100%
    Optional dependencies for python
        python-setuptools
        python-pip
        sqlite [installed]
        mpdecimal: for decimal
        xz: for lzma [installed]
        tk: for tkinter
    (6/6) installing snap-pac                                                       [##############################################] 100%
    :: Running post-transaction hooks...
    (1/4) Reloading system manager configuration...
      Skipped: Running in chroot.
    (2/4) Arming ConditionNeedsUpdate...
    (3/4) Reloading system bus configuration...
      Skipped: Running in chroot.
    (4/4) Performing snapper post snapshots for the following configurations...

    Install the X Window System and X Window System applications.

    [root@G5-openSUSE /]# pacstrap /mnt xorg-server xorg-apps

    [root@G5-openSUSE /]# pacstrap /mnt xorg-server xorg-apps
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community is up to date
    :: There are 35 members in group xorg-apps:
    :: Repository extra
       1) xorg-bdftopcf  2) xorg-iceauth  3) xorg-mkfontscale  4) xorg-sessreg  5) xorg-setxkbmap  6) xorg-smproxy  7) xorg-x11perf
       2) xorg-xauth  9) xorg-xbacklight  10) xorg-xcmsdb  11) xorg-xcursorgen  12) xorg-xdpyinfo  13) xorg-xdriinfo  14) xorg-xev
       3) xorg-xgamma  16) xorg-xhost  17) xorg-xinput  18) xorg-xkbcomp  19) xorg-xkbevd  20) xorg-xkbutils  21) xorg-xkill
       4) xorg-xlsatoms  23) xorg-xlsclients  24) xorg-xmodmap  25) xorg-xpr  26) xorg-xprop  27) xorg-xrandr  28) xorg-xrdb
       5) xorg-xrefresh  30) xorg-xset  31) xorg-xsetroot  32) xorg-xvinfo  33) xorg-xwd  34) xorg-xwininfo  35) xorg-xwud

    Enter a selection (default=all): 
    resolving dependencies...
    looking for conflicting packages...
    warning: dependency cycle detected:
    warning: harfbuzz will be installed before its freetype2 dependency
    warning: dependency cycle detected:
    warning: mesa will be installed before its libglvnd dependency

    Packages (93) fontconfig-2:2.13.94-1  freetype2-2.11.0-4  graphite-1:1.3.14-1  harfbuzz-3.0.0-1  libdrm-2.4.107-1
                  libedit-20210910_3.1-1  libepoxy-1.5.9-1  libevdev-1.11.0-1  libfontenc-1.1.4-3  libglvnd-1.3.4-1  libgudev-237-1
                  libice-1.0.10-3  libinput-1.19.1-1  libomxil-bellagio-0.9.3-3  libpciaccess-0.16-2  libpng-1.6.37-3  libsm-1.2.3-2
                  libunwind-1.5.0-1  libwacom-1.12-1  libx11-1.7.2-1  libxau-1.0.9-3  libxaw-1.0.14-1  libxcb-1.14-1
                  libxcomposite-0.4.5-3  libxcursor-1.2.0-2  libxdamage-1.1.5-3  libxdmcp-1.1.3-3  libxext-1.3.4-3  libxfixes-6.0.0-1
                  libxfont2-2.0.5-1  libxft-2.3.4-1  libxi-1.8-1  libxinerama-1.1.4-3  libxkbfile-1.1.0-2  libxmu-1.1.3-2
                  libxpm-3.5.13-2  libxrandr-1.5.2-3  libxrender-0.9.10-4  libxshmfence-1.3-2  libxt-1.2.1-1  libxtst-1.2.3-4
                  libxv-1.0.11-4  libxxf86vm-1.1.4-4  llvm-libs-12.0.1-4  lm_sensors-1:3.6.0.r41.g31d1f125-1  mesa-21.2.3-1
                  mtdev-1.1.6-1  pixman-0.40.0-1  vulkan-icd-loader-1.2.194-1  wayland-1.19.0-1  xcb-proto-1.14.1-3  xcb-util-0.4.0-3
                  xf86-input-libinput-1.2.0-1  xkeyboard-config-2.33-2  xorg-fonts-encodings-1.0.5-2  xorg-server-common-1.20.13-2
                  xorgproto-2021.5-1  xorg-bdftopcf-1.1-2  xorg-iceauth-1.0.8-2  xorg-mkfontscale-1.2.1-2  xorg-server-1.20.13-2
                  xorg-sessreg-1.1.2-2  xorg-setxkbmap-1.3.2-2  xorg-smproxy-1.0.6-3  xorg-x11perf-1.6.1-2  xorg-xauth-1.1-2
                  xorg-xbacklight-1.2.3-2  xorg-xcmsdb-1.0.5-3  xorg-xcursorgen-1.0.7-2  xorg-xdpyinfo-1.3.2-4  xorg-xdriinfo-1.0.6-2
                  xorg-xev-1.2.4-1  xorg-xgamma-1.0.6-3  xorg-xhost-1.0.8-2  xorg-xinput-1.6.3-2  xorg-xkbcomp-1.4.5-1
                  xorg-xkbevd-1.1.4-3  xorg-xkbutils-1.0.4-4  xorg-xkill-1.0.5-2  xorg-xlsatoms-1.1.3-2  xorg-xlsclients-1.1.4-2
                  xorg-xmodmap-1.0.10-2  xorg-xpr-1.0.5-2  xorg-xprop-1.2.5-1  xorg-xrandr-1.5.1-2  xorg-xrdb-1.2.1-1
                  xorg-xrefresh-1.0.6-2  xorg-xset-1.2.4-2  xorg-xsetroot-1.1.2-2  xorg-xvinfo-1.1.4-2  xorg-xwd-1.0.8-1
                  xorg-xwininfo-1.1.5-2  xorg-xwud-1.0.5-2

    Total Download Size:    54.65 MiB
    Total Installed Size:  256.41 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...
     libepoxy-1.5.9-1-x86_64                            436.1 KiB   886 KiB/s 00:00 [##############################################] 100%
     libpng-1.6.37-3-x86_64                             245.9 KiB  1118 KiB/s 00:00 [##############################################] 100%
     graphite-1:1.3.14-1-x86_64                         224.5 KiB   936 KiB/s 00:00 [##############################################] 100%
     harfbuzz-3.0.0-1-x86_64                            895.8 KiB  2.80 MiB/s 00:00 [##############################################] 100%
     freetype2-2.11.0-4-x86_64                          487.1 KiB  1903 KiB/s 00:00 [##############################################] 100%
     xorg-fonts-encodings-1.0.5-2-any                   567.0 KiB  2.10 MiB/s 00:00 [##############################################] 100%
     libfontenc-1.1.4-3-x86_64                           15.4 KiB  72.7 KiB/s 00:00 [##############################################] 100%
     libxfont2-2.0.5-1-x86_64                           116.1 KiB   528 KiB/s 00:00 [##############################################] 100%
     pixman-0.40.0-1-x86_64                             261.8 KiB  1235 KiB/s 00:00 [##############################################] 100%
     xkeyboard-config-2.33-2-any                        737.7 KiB  2.43 MiB/s 00:00 [##############################################] 100%
     xcb-proto-1.14.1-3-any                             108.0 KiB   500 KiB/s 00:00 [##############################################] 100%
     libxdmcp-1.1.3-3-x86_64                             27.4 KiB   114 KiB/s 00:00 [##############################################] 100%
     libxau-1.0.9-3-x86_64                               10.9 KiB  52.3 KiB/s 00:00 [##############################################] 100%
     libxcb-1.14-1-x86_64                               999.8 KiB  3.34 MiB/s 00:00 [##############################################] 100%
     xorgproto-2021.5-1-any                             240.3 KiB   985 KiB/s 00:00 [##############################################] 100%
     libx11-1.7.2-1-x86_64                                2.1 MiB  5.40 MiB/s 00:00 [##############################################] 100%
     libxkbfile-1.1.0-2-x86_64                           75.3 KiB   448 KiB/s 00:00 [##############################################] 100%
     xorg-xkbcomp-1.4.5-1-x86_64                         91.5 KiB   572 KiB/s 00:00 [##############################################] 100%
     xorg-setxkbmap-1.3.2-2-x86_64                       13.9 KiB  96.3 KiB/s 00:00 [##############################################] 100%
     xorg-server-common-1.20.13-2-x86_64                 27.7 KiB   193 KiB/s 00:00 [##############################################] 100%
     libunwind-1.5.0-1-x86_64                           111.1 KiB   534 KiB/s 00:00 [##############################################] 100%
     libxext-1.3.4-3-x86_64                             107.3 KiB   639 KiB/s 00:00 [##############################################] 100%
     libpciaccess-0.16-2-x86_64                          21.6 KiB   150 KiB/s 00:00 [##############################################] 100%
     libdrm-2.4.107-1-x86_64                            270.0 KiB  1324 KiB/s 00:00 [##############################################] 100%
     wayland-1.19.0-1-x86_64                            131.8 KiB   716 KiB/s 00:00 [##############################################] 100%
     libxxf86vm-1.1.4-4-x86_64                           15.7 KiB   106 KiB/s 00:00 [##############################################] 100%
     libxfixes-6.0.0-1-x86_64                            13.7 KiB  90.1 KiB/s 00:00 [##############################################] 100%
     libxdamage-1.1.5-3-x86_64                            7.1 KiB  37.8 KiB/s 00:00 [##############################################] 100%
     libxshmfence-1.3-2-x86_64                            5.7 KiB  39.9 KiB/s 00:00 [##############################################] 100%
     libomxil-bellagio-0.9.3-3-x86_64                   123.0 KiB   683 KiB/s 00:00 [##############################################] 100%
     libedit-20210910_3.1-1-x86_64                      110.0 KiB   550 KiB/s 00:00 [##############################################] 100%
     llvm-libs-12.0.1-4-x86_64                           24.4 MiB  12.6 MiB/s 00:02 [##############################################] 100%
     lm_sensors-1:3.6.0.r41.g31d1f125-1-x86_64          134.4 KiB   686 KiB/s 00:00 [##############################################] 100%
     vulkan-icd-loader-1.2.194-1-x86_64                 108.7 KiB   618 KiB/s 00:00 [##############################################] 100%
     mesa-21.2.3-1-x86_64                                16.9 MiB  10.7 MiB/s 00:02 [##############################################] 100%
     libglvnd-1.3.4-1-x86_64                            353.2 KiB  1299 KiB/s 00:00 [##############################################] 100%
     mtdev-1.1.6-1-x86_64                                17.1 KiB   129 KiB/s 00:00 [##############################################] 100%
     libevdev-1.11.0-1-x86_64                            63.8 KiB   326 KiB/s 00:00 [##############################################] 100%
     libgudev-237-1-x86_64                               44.0 KiB   262 KiB/s 00:00 [##############################################] 100%
     libwacom-1.12-1-x86_64                             102.8 KiB   547 KiB/s 00:00 [##############################################] 100%
     libinput-1.19.1-1-x86_64                           292.3 KiB  1491 KiB/s 00:00 [##############################################] 100%
     xf86-input-libinput-1.2.0-1-x86_64                  37.3 KiB   245 KiB/s 00:00 [##############################################] 100%
     xorg-server-1.20.13-2-x86_64                      1425.3 KiB  4.40 MiB/s 00:00 [##############################################] 100%
     xorg-bdftopcf-1.1-2-x86_64                          24.9 KiB   173 KiB/s 00:00 [##############################################] 100%
     libice-1.0.10-3-x86_64                              78.3 KiB   515 KiB/s 00:00 [##############################################] 100%
     xorg-iceauth-1.0.8-2-x86_64                         17.6 KiB   122 KiB/s 00:00 [##############################################] 100%
     xorg-mkfontscale-1.2.1-2-x86_64                     24.3 KiB   127 KiB/s 00:00 [##############################################] 100%
     xorg-sessreg-1.1.2-2-x86_64                          8.9 KiB  63.7 KiB/s 00:00 [##############################################] 100%
     libsm-1.2.3-2-x86_64                                45.5 KiB   247 KiB/s 00:00 [##############################################] 100%
     libxt-1.2.1-1-x86_64                               535.5 KiB  2.14 MiB/s 00:00 [##############################################] 100%
     libxmu-1.1.3-2-x86_64                               77.0 KiB   310 KiB/s 00:00 [##############################################] 100%
     xorg-smproxy-1.0.6-3-x86_64                         12.8 KiB  86.6 KiB/s 00:00 [##############################################] 100%
     libxrender-0.9.10-4-x86_64                          26.0 KiB   180 KiB/s 00:00 [##############################################] 100%
     fontconfig-2:2.13.94-1-x86_64                      319.4 KiB  1426 KiB/s 00:00 [##############################################] 100%
     libxft-2.3.4-1-x86_64                               48.4 KiB   327 KiB/s 00:00 [##############################################] 100%
     xorg-x11perf-1.6.1-2-x86_64                         68.6 KiB   429 KiB/s 00:00 [##############################################] 100%
     xorg-xauth-1.1-2-x86_64                             25.5 KiB   188 KiB/s 00:00 [##############################################] 100%
     xcb-util-0.4.0-3-x86_64                             12.5 KiB  89.3 KiB/s 00:00 [##############################################] 100%
     xorg-xbacklight-1.2.3-2-x86_64                       8.9 KiB  61.9 KiB/s 00:00 [##############################################] 100%
     xorg-xcmsdb-1.0.5-3-x86_64                          17.3 KiB   124 KiB/s 00:00 [##############################################] 100%
     libxcursor-1.2.0-2-x86_64                           28.5 KiB   165 KiB/s 00:00 [##############################################] 100%
     xorg-xcursorgen-1.0.7-2-x86_64                       9.5 KiB  60.7 KiB/s 00:00 [##############################################] 100%
     libxi-1.8-1-x86_64                                 150.9 KiB   726 KiB/s 00:00 [##############################################] 100%
     libxtst-1.2.3-4-x86_64                              29.5 KiB   180 KiB/s 00:00 [##############################################] 100%
     libxcomposite-0.4.5-3-x86_64                        11.4 KiB  81.6 KiB/s 00:00 [##############################################] 100%
     libxinerama-1.1.4-3-x86_64                          10.1 KiB  72.5 KiB/s 00:00 [##############################################] 100%
     xorg-xdpyinfo-1.3.2-4-x86_64                        16.0 KiB  70.4 KiB/s 00:00 [##############################################] 100%
     xorg-xdriinfo-1.0.6-2-x86_64                         7.0 KiB  47.1 KiB/s 00:00 [##############################################] 100%
     libxrandr-1.5.2-3-x86_64                            27.2 KiB   154 KiB/s 00:00 [##############################################] 100%
     xorg-xev-1.2.4-1-x86_64                             15.4 KiB  81.8 KiB/s 00:00 [##############################################] 100%
     xorg-xgamma-1.0.6-3-x86_64                           8.8 KiB  59.3 KiB/s 00:00 [##############################################] 100%
     xorg-xhost-1.0.8-2-x86_64                           11.7 KiB  73.1 KiB/s 00:00 [##############################################] 100%
     xorg-xrandr-1.5.1-2-x86_64                          36.8 KiB   214 KiB/s 00:00 [##############################################] 100%
     xorg-xinput-1.6.3-2-x86_64                          28.1 KiB   176 KiB/s 00:00 [##############################################] 100%
     xorg-xkbevd-1.1.4-3-x86_64                          18.6 KiB   126 KiB/s 00:00 [##############################################] 100%
     libxpm-3.5.13-2-x86_64                              54.2 KiB   331 KiB/s 00:00 [##############################################] 100%
     libxaw-1.0.14-1-x86_64                             361.0 KiB  1612 KiB/s 00:00 [##############################################] 100%
     xorg-xkbutils-1.0.4-4-x86_64                        19.2 KiB   133 KiB/s 00:00 [##############################################] 100%
     xorg-xkill-1.0.5-2-x86_64                            9.5 KiB  69.6 KiB/s 00:00 [##############################################] 100%
     xorg-xlsatoms-1.1.3-2-x86_64                         8.3 KiB  54.5 KiB/s 00:00 [##############################################] 100%
     xorg-xlsclients-1.1.4-2-x86_64                      10.3 KiB  73.8 KiB/s 00:00 [##############################################] 100%
     xorg-xmodmap-1.0.10-2-x86_64                        22.5 KiB   110 KiB/s 00:00 [##############################################] 100%
     xorg-xpr-1.0.5-2-x86_64                             33.2 KiB   189 KiB/s 00:00 [##############################################] 100%
     xorg-xprop-1.2.5-1-x86_64                           26.3 KiB   156 KiB/s 00:00 [##############################################] 100%
     xorg-xrdb-1.2.1-1-x86_64                            20.0 KiB  98.2 KiB/s 00:00 [##############################################] 100%
     xorg-xrefresh-1.0.6-2-x86_64                         8.9 KiB  63.8 KiB/s 00:00 [##############################################] 100%
     xorg-xset-1.2.4-2-x86_64                            19.4 KiB  69.4 KiB/s 00:00 [##############################################] 100%
     xorg-xsetroot-1.1.2-2-x86_64                        11.7 KiB  75.1 KiB/s 00:00 [##############################################] 100%
     libxv-1.0.11-4-x86_64                               35.4 KiB   181 KiB/s 00:00 [##############################################] 100%
     xorg-xvinfo-1.1.4-2-x86_64                           8.8 KiB  59.7 KiB/s 00:00 [##############################################] 100%
     xorg-xwd-1.0.8-1-x86_64                             18.5 KiB   118 KiB/s 00:00 [##############################################] 100%
     xorg-xwininfo-1.1.5-2-x86_64                        23.8 KiB   157 KiB/s 00:00 [##############################################] 100%
     xorg-xwud-1.0.5-2-x86_64                            16.2 KiB   109 KiB/s 00:00 [##############################################] 100%
     Total (93/93)                                       54.7 MiB  1833 KiB/s 00:31 [##############################################] 100%
    (93/93) checking keys in keyring                                                [##############################################] 100%
    (93/93) checking package integrity                                              [##############################################] 100%
    (93/93) loading package files                                                   [##############################################] 100%
    (93/93) checking for file conflicts                                             [##############################################] 100%
    (93/93) checking available disk space                                           [##############################################] 100%
    :: Running pre-transaction hooks...
    (1/1) Performing snapper pre snapshots for the following configurations...
    :: Processing package changes...
    ( 1/93) installing libepoxy                                                     [##############################################] 100%
    ( 2/93) installing libpng                                                       [##############################################] 100%
    ( 3/93) installing graphite                                                     [##############################################] 100%
    ( 4/93) installing harfbuzz                                                     [##############################################] 100%
    Optional dependencies for harfbuzz
        cairo: hb-view program
        chafa: hb-view program
    ( 5/93) installing freetype2                                                    [##############################################] 100%
    ( 6/93) installing xorg-fonts-encodings                                         [##############################################] 100%
    ( 7/93) installing libfontenc                                                   [##############################################] 100%
    ( 8/93) installing libxfont2                                                    [##############################################] 100%
    ( 9/93) installing pixman                                                       [##############################################] 100%
    (10/93) installing xkeyboard-config                                             [##############################################] 100%
    (11/93) installing xcb-proto                                                    [##############################################] 100%
    (12/93) installing libxdmcp                                                     [##############################################] 100%
    (13/93) installing libxau                                                       [##############################################] 100%
    (14/93) installing libxcb                                                       [##############################################] 100%
    (15/93) installing xorgproto                                                    [##############################################] 100%
    (16/93) installing libx11                                                       [##############################################] 100%
    (17/93) installing libxkbfile                                                   [##############################################] 100%
    (18/93) installing xorg-xkbcomp                                                 [##############################################] 100%
    (19/93) installing xorg-setxkbmap                                               [##############################################] 100%
    (20/93) installing xorg-server-common                                           [##############################################] 100%
    (21/93) installing libunwind                                                    [##############################################] 100%
    (22/93) installing libxext                                                      [##############################################] 100%
    (23/93) installing libpciaccess                                                 [##############################################] 100%
    (24/93) installing libdrm                                                       [##############################################] 100%
    (25/93) installing wayland                                                      [##############################################] 100%
    (26/93) installing libxxf86vm                                                   [##############################################] 100%
    (27/93) installing libxfixes                                                    [##############################################] 100%
    (28/93) installing libxdamage                                                   [##############################################] 100%
    (29/93) installing libxshmfence                                                 [##############################################] 100%
    (30/93) installing libomxil-bellagio                                            [##############################################] 100%
    (31/93) installing libedit                                                      [##############################################] 100%
    (32/93) installing llvm-libs                                                    [##############################################] 100%
    (33/93) installing lm_sensors                                                   [##############################################] 100%
    Optional dependencies for lm_sensors
        rrdtool: for logging with sensord
        perl: for sensor detection and configuration convert
    (34/93) installing vulkan-icd-loader                                            [##############################################] 100%
    Optional dependencies for vulkan-icd-loader
        vulkan-driver: packaged vulkan driver
    (35/93) installing mesa                                                         [##############################################] 100%
    Optional dependencies for mesa
        opengl-man-pages: for the OpenGL API man pages
        mesa-vdpau: for accelerated video playback
        libva-mesa-driver: for accelerated video playback
    (36/93) installing libglvnd                                                     [##############################################] 100%
    (37/93) installing mtdev                                                        [##############################################] 100%
    (38/93) installing libevdev                                                     [##############################################] 100%
    (39/93) installing libgudev                                                     [##############################################] 100%
    (40/93) installing libwacom                                                     [##############################################] 100%
    (41/93) installing libinput                                                     [##############################################] 100%
    Optional dependencies for libinput
        gtk3: libinput debug-gui
        python-pyudev: libinput measure
        python-libevdev: libinput measure
    (42/93) installing xf86-input-libinput                                          [##############################################] 100%
    (43/93) installing xorg-server                                                  [##############################################] 100%
    >>> xorg-server has now the ability to run without root rights with
        the help of systemd-logind. xserver will fail to run if not launched
        from the same virtual terminal as was used to log in.
        Without root rights, log files will be in ~/.local/share/xorg/ directory.

        Old behavior can be restored through Xorg.wrap config file.
        See Xorg.wrap man page (man xorg.wrap).
    (44/93) installing xorg-bdftopcf                                                [##############################################] 100%
    (45/93) installing libice                                                       [##############################################] 100%
    (46/93) installing xorg-iceauth                                                 [##############################################] 100%
    (47/93) installing xorg-mkfontscale                                             [##############################################] 100%
    Creating X fontdir indices... done.
    (48/93) installing xorg-sessreg                                                 [##############################################] 100%
    (49/93) installing libsm                                                        [##############################################] 100%
    (50/93) installing libxt                                                        [##############################################] 100%
    (51/93) installing libxmu                                                       [##############################################] 100%
    (52/93) installing xorg-smproxy                                                 [##############################################] 100%
    (53/93) installing libxrender                                                   [##############################################] 100%
    (54/93) installing fontconfig                                                   [##############################################] 100%
    Creating fontconfig configuration...
    Rebuilding fontconfig cache...
    (55/93) installing libxft                                                       [##############################################] 100%
    (56/93) installing xorg-x11perf                                                 [##############################################] 100%
    (57/93) installing xorg-xauth                                                   [##############################################] 100%
    (58/93) installing xcb-util                                                     [##############################################] 100%
    (59/93) installing xorg-xbacklight                                              [##############################################] 100%
    (60/93) installing xorg-xcmsdb                                                  [##############################################] 100%
    (61/93) installing libxcursor                                                   [##############################################] 100%
    Optional dependencies for libxcursor
        gnome-themes-standard: fallback icon theme
    (62/93) installing xorg-xcursorgen                                              [##############################################] 100%
    (63/93) installing libxi                                                        [##############################################] 100%
    (64/93) installing libxtst                                                      [##############################################] 100%
    (65/93) installing libxcomposite                                                [##############################################] 100%
    (66/93) installing libxinerama                                                  [##############################################] 100%
    (67/93) installing xorg-xdpyinfo                                                [##############################################] 100%
    (68/93) installing xorg-xdriinfo                                                [##############################################] 100%
    (69/93) installing libxrandr                                                    [##############################################] 100%
    (70/93) installing xorg-xev                                                     [##############################################] 100%
    (71/93) installing xorg-xgamma                                                  [##############################################] 100%
    (72/93) installing xorg-xhost                                                   [##############################################] 100%
    (73/93) installing xorg-xrandr                                                  [##############################################] 100%
    (74/93) installing xorg-xinput                                                  [##############################################] 100%
    (75/93) installing xorg-xkbevd                                                  [##############################################] 100%
    (76/93) installing libxpm                                                       [##############################################] 100%
    (77/93) installing libxaw                                                       [##############################################] 100%
    (78/93) installing xorg-xkbutils                                                [##############################################] 100%
    (79/93) installing xorg-xkill                                                   [##############################################] 100%
    (80/93) installing xorg-xlsatoms                                                [##############################################] 100%
    (81/93) installing xorg-xlsclients                                              [##############################################] 100%
    (82/93) installing xorg-xmodmap                                                 [##############################################] 100%
    (83/93) installing xorg-xpr                                                     [##############################################] 100%
    (84/93) installing xorg-xprop                                                   [##############################################] 100%
    (85/93) installing xorg-xrdb                                                    [##############################################] 100%
    Optional dependencies for xorg-xrdb
        gcc: for preprocessing
        mcpp: a lightweight alternative for preprocessing
    (86/93) installing xorg-xrefresh                                                [##############################################] 100%
    (87/93) installing xorg-xset                                                    [##############################################] 100%
    (88/93) installing xorg-xsetroot                                                [##############################################] 100%
    (89/93) installing libxv                                                        [##############################################] 100%
    (90/93) installing xorg-xvinfo                                                  [##############################################] 100%
    (91/93) installing xorg-xwd                                                     [##############################################] 100%
    (92/93) installing xorg-xwininfo                                                [##############################################] 100%
    (93/93) installing xorg-xwud                                                    [##############################################] 100%
    :: Running post-transaction hooks...
    (1/7) Reloading system manager configuration...
      Skipped: Running in chroot.
    (2/7) Updating udev hardware database...
    (3/7) Reloading device manager configuration...
      Skipped: Running in chroot.
    (4/7) Arming ConditionNeedsUpdate...
    (5/7) Updating fontconfig configuration...
    (6/7) Updating fontconfig cache...
    (7/7) Performing snapper post snapshots for the following configurations...

    Install Plasma and some KDE applications

    [root@G5-openSUSE /]# pacstrap /mnt plasma-meta kde-utilities kde-system dolphin-plugins kde-graphics

    The command with the output:

    [root@G5-openSUSE /]# pacstrap /mnt plasma-meta kde-utilities kde-system dolphin-plugins kde-graphics
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community                                            5.8 MiB   972 KiB/s 00:06 [##############################################] 100%
    :: There are 22 members in group kde-utilities:
    :: Repository extra
       6) ark  2) filelight  3) kate  4) kbackup  5) kcalc  6) kcharselect  7) kdebugsettings  8) kdf  9) kdialog  10) keditbookmarks
       7) kfind  12) kfloppy  13) kgpg  14) konsole  15) kteatime  16) ktimer  17) kwalletmanager  18) kwrite  19) markdownpart
       8) print-manager  21) sweeper  22) yakuake

    Enter a selection (default=all): 
    :: There are 5 members in group kde-system:
    :: Repository extra
       9) dolphin  2) kcron  3) khelpcenter  4) ksystemlog  5) partitionmanager

    Enter a selection (default=all): 
    :: There are 13 members in group kde-graphics:
    :: Repository extra
       10) gwenview  2) kamera  3) kcolorchooser  4) kdegraphics-mobipocket  5) kdegraphics-thumbnailers  6) kimagemapeditor
       11) kipi-plugins  8) kolourpaint  9) kruler  10) okular  11) skanlite  12) spectacle  13) svgpart

    Enter a selection (default=all): 
    resolving dependencies...
    :: There are 8 providers available for ttf-font:
    :: Repository extra
       12) gnu-free-fonts  2) noto-fonts  3) ttf-bitstream-vera  4) ttf-croscore  5) ttf-dejavu
    :: Repository community
       13) ttf-droid  7) ttf-ibm-plex  8) ttf-liberation

    Enter a number (default=1): 
    :: There are 2 providers available for phonon-qt5-backend:
    :: Repository extra
       14) phonon-qt5-gstreamer  2) phonon-qt5-vlc

    Enter a number (default=1): 
    :: There are 2 providers available for cron:
    :: Repository core
       15) cronie
    :: Repository community
       16) fcron

    Enter a number (default=1): 
    looking for conflicting packages...
    warning: dependency cycle detected:
    warning: usbmuxd will be installed before its libimobiledevice dependency
    warning: dependency cycle detected:
    warning: cifs-utils will be installed before its smbclient dependency
    warning: dependency cycle detected:
    warning: phonon-qt5-gstreamer will be installed before its phonon-qt5 dependency

    Packages (454) accounts-qml-module-0.7-4  accountsservice-0.6.55-3  adobe-source-code-pro-fonts-2.038ro+1.058it+1.018var-1
                   akonadi-contacts-21.08.1-1  alsa-card-profiles-1:0.3.38-1  alsa-lib-1.2.5.1-3  alsa-topology-conf-1.2.5.1-1
                   alsa-ucm-conf-1.2.5.1-1  aom-3.1.2-2  appstream-0.14.5-2  appstream-qt-0.14.5-2  archlinux-appstream-data-20210929-1
                   attica-5.86.0-2  avahi-0.8+22+gfd482a7-1  baloo-5.86.0-1  baloo-widgets-21.08.1-1  bluedevil-1:5.22.5-1  bluez-5.61-1
                   bluez-libs-5.61-1  bluez-qt-5.86.0-1  bolt-0.9.1-1  breeze-5.22.5-1  breeze-gtk-5.22.5-1  breeze-icons-5.86.0-1
                   cairo-1.17.4-5  cantarell-fonts-1:0.303-1  cdparanoia-10.2-8  celt-0.11.3-4  cfitsio-1:4.0.0-1  cifs-utils-6.14-1
                   convertlit-1.8-10  cronie-1.5.7-2  dav1d-0.9.2-1  db-5.3.28-5  dbus-glib-0.112-2  dconf-0.40.0-1  discount-2.2.7-1
                   discover-5.22.5-2  djvulibre-3.5.28-3  dmraid-1.0.0.rc16.3-13  docbook-xml-4.5-9  docbook-xsl-1.79.2-7
                   dosfstools-4.2-1  double-conversion-3.1.5-2  drkonqi-5.22.5-1  ebook-tools-0.2.2-7  editorconfig-core-c-0.12.5-1
                   exiv2-0.27.4-2  ffmpeg-2:4.4-4  flac-1.3.3-3  frameworkintegration-5.86.0-1  fribidi-1.0.10-1  fuse3-3.10.5-1
                   gc-8.0.4-4  gd-2.3.3-2  gdb-11.1-1  gdb-common-11.1-1  gdk-pixbuf2-2.42.6-2  ghostscript-9.55.0-2  giflib-5.2.1-2
                   glib-networking-1:2.70.0-1  glu-9.0.2-1  gnu-free-fonts-20120503-8  gpm-1.20.7.r38.ge82d1a6-4  gptfdisk-1.0.8-1
                   grantlee-5.2.0-3  grantleetheme-21.08.1-1  graphene-1.10.6-1  gsettings-desktop-schemas-41.0-1  gsm-1.0.19-1
                   gst-plugins-base-1.18.5-1  gst-plugins-base-libs-1.18.5-1  gstreamer-1.18.5-1  guile-2.2.7-1
                   hicolor-icon-theme-0.17-2  hidapi-0.11.0-1  http-parser-2.9.4-1  ijs-0.35-3  iso-codes-4.7.0-1  jack2-1.9.19-2
                   jansson-2.14-1  jasper-2.0.33-1  jbig2dec-0.19-1  js78-78.14.0-1  kaccounts-integration-21.08.1-1
                   kactivities-5.86.0-1  kactivities-stats-5.86.0-1  kactivitymanagerd-5.22.5-1  karchive-5.86.0-1  kauth-5.86.0-1
                   kbookmarks-5.86.0-1  kcmutils-5.86.0-1  kcodecs-5.86.0-1  kcolorpicker-0.1.6-1  kcompletion-5.86.0-1
                   kconfig-5.86.0-1  kconfigwidgets-5.86.0-1  kcontacts-1:5.86.0-1  kcoreaddons-5.86.0-1  kcrash-5.86.0-1
                   kdbusaddons-5.86.0-1  kde-cli-tools-5.22.5-1  kde-gtk-config-5.22.5-1  kdeclarative-5.86.0-1  kdecoration-5.22.5-1
                   kded-5.86.0-1  kdelibs4support-5.86.0-1  kdeplasma-addons-5.22.5-1  kdesu-5.86.0-1  kdnssd-5.86.0-1
                   kdoctools-5.86.0-1  kdsoap-2.0.0-1  kdsoap-ws-discovery-client-git20200927-2  kemoticons-5.86.0-1
                   kfilemetadata-5.86.0-1  kgamma5-5.22.5-1  kglobalaccel-5.86.0-1  kguiaddons-5.86.0-1  kholidays-1:5.86.0-1
                   khotkeys-5.22.5-1  khtml-5.86.0-1  ki18n-5.86.0-1  kiconthemes-5.86.0-1  kidletime-5.86.0-1  kimageannotator-0.5.2-1
                   kinfocenter-5.22.5-1  kio-5.86.0-1  kio-extras-21.08.1-2  kio-fuse-5.0.1-1  kirigami2-5.86.0-1  kitemmodels-5.86.0-1
                   kitemviews-5.86.0-1  kjobwidgets-5.86.0-1  kjs-5.86.0-1  kmenuedit-5.22.5-1  kmime-21.08.1-1  knewstuff-5.86.0-2
                   knotifications-5.86.0-1  knotifyconfig-5.86.0-1  kpackage-5.86.0-1  kparts-5.86.0-1  kpeople-5.86.0-1
                   kpimtextedit-21.08.1-1  kpmcore-21.08.1-1  kpty-5.86.0-1  kquickcharts-5.86.0-1  krunner-5.86.0-1  kscreen-5.22.5-1
                   kscreenlocker-5.22.5-1  kservice-5.86.0-1  ksshaskpass-5.22.5-1  ksystemstats-5.22.5-1  ktexteditor-5.86.0-2
                   ktextwidgets-5.86.0-1  kunitconversion-5.86.0-1  kuserfeedback-1.0.0-1  kwallet-5.86.0-1  kwallet-pam-5.22.5-1
                   kwayland-5.86.0-1  kwayland-integration-5.22.5-1  kwayland-server-5.22.5-1  kwidgetsaddons-5.86.0-1  kwin-5.22.5-1
                   kwindowsystem-5.86.0-1  kwrited-5.22.5-1  kxmlgui-5.86.0-1  l-smash-2.14.5-2  lame-3.100-3  layer-shell-qt-5.22.5-1
                   lcms2-2.12-1  ldb-2:2.4.0-1  libaccounts-glib-1.25-4  libaccounts-qt-1.16-3  libaio-0.3.112-2  libakonadi-21.08.1-1
                   libass-0.15.2-1  libasyncns-0.8+3+g68cd5af-3  libatasmart-0.19-5  libavc1394-0.5.4-4  libavif-0.9.2-1
                   libblockdev-2.26-1  libbluray-1.3.0-1  libbsd-0.11.3-1  libbytesize-2.6-1  libcanberra-0.30+2+gc0620e4-5
                   libcups-1:2.3.3op2-3  libdaemon-0.14-5  libdatrie-0.2.13-1  libdbusmenu-qt5-0.9.3+16.04.20160218-5  libde265-1.0.8-1
                   libdmtx-0.7.5-2  libevent-2.1.12-1  libexif-0.6.23-1  libfdk-aac-2.0.2-1  libfreeaptx-0.1.1-1  libgit2-1:1.2.0-1
                   libgphoto2-2.5.27-1  libheif-1.12.0-1  libibus-1.5.25-3  libical-3.0.10-1  libidn-1.38-1  libiec61883-1.2.0-6
                   libieee1284-0.2.11-11  libimobiledevice-1.3.0-3  libinih-53-1  libjpeg-turbo-2.1.1-1  libkdcraw-21.08.1-1
                   libkexiv2-21.08.1-1  libkipi-21.08.1-1  libkleo-21.08.1-1  libksane-21.08.1-1  libkscreen-5.22.5-1
                   libksysguard-5.22.5-1  libldac-2.0.2.3-1  libmbim-1.26.0-2  libmd-1.0.3-1  libmfx-21.3.2-1  libmm-glib-1.18.2-1
                   libmodplug-0.8.9.0-3  libmtp-1.1.19-1  libndp-1.8-1  libnewt-0.52.21-5  libnm-1.32.12-1  libnotify-0.7.9-2
                   libogg-1.3.5-1  libpaper-1.1.28-1  libpgm-5.3.128-1  libplist-2.2.0-3  libproxy-0.4.17-2  libpulse-15.0-1
                   libqaccessibilityclient-0.4.1-2  libqalculate-3.20.1-1  libqmi-1.30.2-1  libqrtr-glib-1.0.0-1  libraw-0.20.2-1
                   libraw1394-2.1.2-3  librsvg-2:2.52.0-1  libsamplerate-0.2.2-1  libsndfile-1.0.31-1  libsodium-1.0.18-2
                   libsoup-2.74.0-3  libsoxr-0.1.3-2  libspectre-0.2.9-2  libssh-0.9.6-1  libstemmer-2.1.0-1  libteam-1.31-3
                   libthai-0.1.28-2  libtheora-1.1.1-5  libtiff-4.3.0-1  libtommath-1.2.0-3  libtool-2.4.6+42+gb88cebd5-16
                   libusb-1.0.24-2  libusbmuxd-2.0.2-1  libutempter-1.2.1-1  libva-2.13.0-1  libvdpau-1.4-1  libvisual-0.4.0-8
                   libvorbis-1.3.7-2  libvpx-1.10.0-1  libwebp-1.2.1-2  libxkbcommon-1.3.1-1  libxkbcommon-x11-1.3.1-1  libxres-1.2.1-1
                   libxslt-1.1.34-6  libxss-1.2.3-3  libyaml-0.2.5-1  libyuv-r2212+dfaf7534-2  libzip-1.8.0-1  lmdb-0.9.29-1
                   lvm2-2.03.13-1  md4c-0.4.8-1  mdadm-4.1-2  media-player-info-24-2  milou-5.22.5-1  minizip-1:1.2.11-4
                   mobile-broadband-provider-info-20210805-1  modemmanager-1.18.2-1  modemmanager-qt-5.86.0-1  ndctl-71.1-1
                   net-snmp-5.9.1-1  networkmanager-1.32.12-1  networkmanager-qt-5.86.0-1  noto-fonts-20201226-2  nspr-4.32-1
                   nss-3.70-1  openal-1.21.1-1  opencore-amr-0.1.5-5  openjpeg2-2.4.0-1  opus-1.3.1-2  orc-0.4.32-1  oxygen-5.22.5-1
                   pango-1:1.48.10-1  parted-3.4-2  perl-5.34.0-2  phonon-qt5-4.11.1-2  phonon-qt5-gstreamer-4.10.0-2
                   pipewire-1:0.3.38-1  pipewire-media-session-1:0.3.38-1  plasma-browser-integration-5.22.5-1  plasma-desktop-5.22.5-1
                   plasma-disks-5.22.5-1  plasma-firewall-5.22.5-1  plasma-framework-5.86.0-1  plasma-integration-5.22.5-1
                   plasma-nm-5.22.5-1  plasma-pa-5.22.5-1  plasma-sdk-5.22.5-1  plasma-systemmonitor-5.22.5-1
                   plasma-thunderbolt-5.22.5-1  plasma-vault-5.22.5-1  plasma-workspace-5.22.5-2  plasma-workspace-wallpapers-5.22.5-1
                   polkit-0.119-1  polkit-kde-agent-5.22.5-1  polkit-qt5-0.114.0-1  poppler-21.09.0-1  poppler-glib-21.09.0-1
                   poppler-qt5-21.09.0-1  powerdevil-5.22.5-1  ppp-2.4.9-1  prison-5.86.0-1  pulseaudio-15.0-1  purpose-5.86.0-1
                   python-dnspython-1:2.1.0-1  python-markdown-3.3.4-1  qca-qt5-2.3.4-1  qgpgme-1.16.0-1  qqc2-desktop-style-5.86.0-1
                   qrencode-4.1.1-1  qt5-base-5.15.2+kde+r228-1  qt5-declarative-5.15.2+kde+r31-1  qt5-graphicaleffects-5.15.2-1
                   qt5-location-5.15.2-3  qt5-multimedia-5.15.2-1  qt5-quickcontrols-5.15.2-1  qt5-quickcontrols2-5.15.2+kde+r8-1
                   qt5-sensors-5.15.2-1  qt5-speech-5.15.2-1  qt5-svg-5.15.2+kde+r7-1  qt5-tools-5.15.2+kde+r17-3
                   qt5-wayland-5.15.2+kde+r33-1  qt5-webchannel-5.15.2-1  qt5-webengine-5.15.6-2  qt5-x11extras-5.15.2-1  rav1e-0.4.1-1
                   re2-1:20210901-1  rtkit-0.13-1  run-parts-5.5-1  sane-1.0.32-3  sbc-1.5-2  sddm-0.19.0-7  sddm-kcm-5.22.5-1
                   sdl2-2.0.16-3  shared-mime-info-2.0+57+gc1d1c70-1  signon-kwallet-extension-21.08.1-1  signon-plugin-oauth2-0.25-1
                   signon-ui-0.17+20171022-2  signond-8.60-3  slang-2.3.2-2  smartmontools-7.2-1  smbclient-4.15.0-1  snappy-1.1.9-2
                   socat-1.7.4.1-1  solid-5.86.0-1  sonnet-5.86.0-1  sound-theme-freedesktop-0.8-4  source-highlight-3.1.9-6
                   speex-1.2.0-3  speexdsp-1.2.0-2  srt-1.4.3-1  svt-av1-0.8.7-1  syndication-5.86.0-1  syntax-highlighting-5.86.0-1
                   sysfsutils-2.1.1-1  systemsettings-5.22.5-1  taglib-1.12-1  talloc-2.3.3-1  tdb-1.4.5-1  tevent-1:0.11.0-1
                   texinfo-6.8-2  thin-provisioning-tools-0.9.0-1  threadweaver-5.86.0-1  tslib-1.22-1  ttf-hack-3.003-3
                   udisks2-2.9.4-1  upower-0.99.13-1  usbmuxd-1.1.1-1  v4l-utils-1.20.0-1  vid.stab-1.1-3  vmaf-1.5.3-1
                   volume_key-0.3.12-5  webrtc-audio-processing-0.3.1-3  which-2.21-5  wpa_supplicant-2:2.9-8
                   x264-3:0.161.r3039.544c61f-1  x265-3.5-1  xapian-core-1:1.4.18-1  xcb-util-cursor-0.1.3-3  xcb-util-image-0.4.0-3
                   xcb-util-keysyms-0.4.0-3  xcb-util-renderutil-0.3.9-3  xcb-util-wm-0.4.1-3  xdg-desktop-portal-kde-5.22.5-1
                   xdg-user-dirs-0.17-3  xdg-utils-1.1.3+19+g9816ebb-1  xfsprogs-5.13.0-1  xorg-xmessage-1.0.5-2  xvidcore-1.3.7-2
                   zeromq-4.3.4-2  zimg-3.0.3-1  zita-alsa-pcmi-0.3.2-3  zita-resampler-1.8.0-1  ark-21.08.1-1  dolphin-21.08.1-1
                   dolphin-plugins-21.08.1-1  filelight-21.08.1-1  gwenview-21.08.1-1  kamera-21.08.1-1  kate-21.08.1-1
                   kbackup-21.08.1-1  kcalc-21.08.1-1  kcharselect-21.08.1-1  kcolorchooser-21.08.1-1  kcron-21.08.1-1
                   kdebugsettings-21.08.1-1  kdegraphics-mobipocket-21.08.1-1  kdegraphics-thumbnailers-21.08.1-1  kdf-21.08.1-1
                   kdialog-21.08.1-1  keditbookmarks-21.08.1-1  kfind-21.08.1-1  kfloppy-21.08.1-1  kgpg-21.08.1-1
                   khelpcenter-21.08.1-1  kimagemapeditor-21.08.1-1  kipi-plugins-21.08.1-1  kolourpaint-21.08.1-1  konsole-21.08.1-1
                   kruler-21.08.1-1  ksystemlog-21.08.1-1  kteatime-21.08.1-1  ktimer-21.08.1-1  kwalletmanager-21.08.1-1
                   kwrite-21.08.1-1  markdownpart-21.08.1-1  okular-21.08.1-1  partitionmanager-21.08.1-1  plasma-meta-5.22-1
                   print-manager-21.08.1-1  skanlite-21.08.1-1  spectacle-21.08.1-1  svgpart-21.08.1-1  sweeper-21.08.1-1
                   yakuake-21.08.1-1

    Total Download Size:    450.58 MiB
    Total Installed Size:  2157.68 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...

    ... truncated ...

    Total (277/277)                                    450.6 MiB  2001 KiB/s 03:51 [##############################################] 100%
    (454/454) checking keys in keyring                                              [##############################################] 100%
    (454/454) checking package integrity                                            [##############################################] 100%
    (454/454) loading package files                                                 [##############################################] 100%
    (454/454) checking for file conflicts                                           [##############################################] 100%
    (454/454) checking available disk space                                         [##############################################] 100%
    :: Running pre-transaction hooks...
    (1/1) Performing snapper pre snapshots for the following configurations...
    :: Processing package changes...
    (  1/454) installing libjpeg-turbo                                              [##############################################] 100%
    Optional dependencies for libjpeg-turbo
        java-runtime>11: for TurboJPEG Java wrapper
    (  2/454) installing xcb-util-keysyms                                           [##############################################] 100%
    (  3/454) installing xcb-util-renderutil                                        [##############################################] 100%
    (  4/454) installing which                                                      [##############################################] 100%
    (  5/454) installing xdg-utils                                                  [##############################################] 100%
    Optional dependencies for xdg-utils
        kde-cli-tools: for KDE Plasma5 support in xdg-open [pending]
        exo: for Xfce support in xdg-open
        pcmanfm: for LXDE support in xdg-open
        perl-file-mimeinfo: for generic support in xdg-open
        perl-net-dbus: Perl extension to dbus used in xdg-screensaver
        perl-x11-protocol: Perl X11 protocol used in xdg-screensaver
    (  6/454) installing shared-mime-info                                           [##############################################] 100%
    (  7/454) installing xcb-util-wm                                                [##############################################] 100%
    (  8/454) installing xcb-util-image                                             [##############################################] 100%
    (  9/454) installing tslib                                                      [##############################################] 100%
    ( 10/454) installing libxkbcommon                                               [##############################################] 100%
    Optional dependencies for libxkbcommon
        libxkbcommon-x11: xkbcli interactive-x11 [pending]
        wayland: xkbcli interactive-wayland [installed]
    ( 11/454) installing libxkbcommon-x11                                           [##############################################] 100%
    ( 12/454) installing libproxy                                                   [##############################################] 100%
    Optional dependencies for libproxy
        networkmanager: NetworkManager configuration module [pending]
        perl: Perl bindings [pending]
        python2: Python 2.x bindings
        python: Python 3.x bindings [installed]
        libproxy-webkit: PAC proxy support (via WebKit)
    ( 13/454) installing libtiff                                                    [##############################################] 100%
    Optional dependencies for libtiff
        freeglut: for using tiffgt
    ( 14/454) installing libdaemon                                                  [##############################################] 100%
    ( 15/454) installing avahi                                                      [##############################################] 100%
    Optional dependencies for avahi
        gtk3: avahi-discover, avahi-discover-standalone, bshell, bssh, bvnc
        qt5-base: qt5 bindings [pending]
        libevent: libevent bindings [pending]
        nss-mdns: NSS support for mDNS
        python-twisted: avahi-bookmarks
        python-gobject: avahi-bookmarks, avahi-discover
        python-dbus: avahi-bookmarks, avahi-discover
    ( 16/454) installing libusb                                                     [##############################################] 100%
    ( 17/454) installing libcups                                                    [##############################################] 100%
    ( 18/454) installing double-conversion                                          [##############################################] 100%
    ( 19/454) installing md4c                                                       [##############################################] 100%
    ( 20/454) installing qt5-base                                                   [##############################################] 100%
    Optional dependencies for qt5-base
        qt5-svg: to use SVG icon themes [pending]
        qt5-wayland: to run Qt applications in a Wayland session [pending]
        qt5-translations: for some native UI translations
        postgresql-libs: PostgreSQL driver
        mariadb-libs: MariaDB driver
        unixodbc: ODBC driver
        libfbclient: Firebird/iBase driver
        freetds: MS SQL driver
        gtk3: GTK platform plugin
        perl: for fixqt4headers and syncqt [pending]
    ( 21/454) installing db                                                         [##############################################] 100%
    ( 22/454) installing libical                                                    [##############################################] 100%
    ( 23/454) installing alsa-topology-conf                                         [##############################################] 100%
    ( 24/454) installing alsa-ucm-conf                                              [##############################################] 100%
    ( 25/454) installing alsa-lib                                                   [##############################################] 100%
    ( 26/454) installing bluez                                                      [##############################################] 100%
    ( 27/454) installing bluez-qt                                                   [##############################################] 100%
    Optional dependencies for bluez-qt
        qt5-declarative: QML bindings [pending]
    ( 28/454) installing media-player-info                                          [##############################################] 100%
    ( 29/454) installing libplist                                                   [##############################################] 100%
    ( 30/454) installing libusbmuxd                                                 [##############################################] 100%
    ( 31/454) installing usbmuxd                                                    [##############################################] 100%
    ( 32/454) installing libimobiledevice                                           [##############################################] 100%
    ( 33/454) installing upower                                                     [##############################################] 100%
    ( 34/454) installing js78                                                       [##############################################] 100%
    ( 35/454) installing polkit                                                     [##############################################] 100%
    ( 36/454) installing libatasmart                                                [##############################################] 100%
    ( 37/454) installing dosfstools                                                 [##############################################] 100%
    ( 38/454) installing dmraid                                                     [##############################################] 100%
    ( 39/454) installing gptfdisk                                                   [##############################################] 100%
    ( 40/454) installing libbytesize                                                [##############################################] 100%
    Optional dependencies for libbytesize
        python: for bscalc command [installed]
    ( 41/454) installing libaio                                                     [##############################################] 100%
    ( 42/454) installing thin-provisioning-tools                                    [##############################################] 100%
    ( 43/454) installing lvm2                                                       [##############################################] 100%
    ( 44/454) installing mdadm                                                      [##############################################] 100%
    ( 45/454) installing ndctl                                                      [##############################################] 100%
    ( 46/454) installing parted                                                     [##############################################] 100%
    ( 47/454) installing nspr                                                       [##############################################] 100%
    ( 48/454) installing nss                                                        [##############################################] 100%
    ( 49/454) installing volume_key                                                 [##############################################] 100%
    Optional dependencies for volume_key
        python: for python bindings [installed]
    ( 50/454) installing libinih                                                    [##############################################] 100%
    ( 51/454) installing xfsprogs                                                   [##############################################] 100%
    Optional dependencies for xfsprogs
        python: for xfs_scrub_all script [installed]
        smtp-forwarder: for xfs_scrub_fail script
    ( 52/454) installing libyaml                                                    [##############################################] 100%
    ( 53/454) installing libblockdev                                                [##############################################] 100%
    ( 54/454) installing udisks2                                                    [##############################################] 100%
    Optional dependencies for udisks2
        gptfdisk: GUID partition table support [installed]
        ntfs-3g: NTFS filesystem management support [installed]
        dosfstools: VFAT filesystem management support [installed]
    ( 55/454) installing solid                                                      [##############################################] 100%
    Optional dependencies for solid
        qt5-declarative: QML bindings [pending]
    ( 56/454) installing kcoreaddons                                                [##############################################] 100%
    Optional dependencies for kcoreaddons
        python-pyqt5: for the Python bindings
    ( 57/454) installing kwidgetsaddons                                             [##############################################] 100%
    Optional dependencies for kwidgetsaddons
        python-pyqt5: for the Python bindings
    ( 58/454) installing qt5-x11extras                                              [##############################################] 100%
    ( 59/454) installing kjobwidgets                                                [##############################################] 100%
    Optional dependencies for kjobwidgets
        python-pyqt5: for the Python bindings
    ( 60/454) installing kdbusaddons                                                [##############################################] 100%
    Optional dependencies for kdbusaddons
        python-pyqt5: for the Python bindings
    ( 61/454) installing kconfig                                                    [##############################################] 100%
    Optional dependencies for kconfig
        python-pyqt5: for the Python bindings
    ( 62/454) installing kwindowsystem                                              [##############################################] 100%
    ( 63/454) installing kcrash                                                     [##############################################] 100%
    Optional dependencies for kcrash
        drkonqi: KDE crash handler application [pending]
    ( 64/454) installing kglobalaccel                                               [##############################################] 100%
    ( 65/454) installing qt5-svg                                                    [##############################################] 100%
    ( 66/454) installing polkit-qt5                                                 [##############################################] 100%
    ( 67/454) installing kauth                                                      [##############################################] 100%
    Optional dependencies for kauth
        python-pyqt5: for the Python bindings
    ( 68/454) installing kcodecs                                                    [##############################################] 100%
    Optional dependencies for kcodecs
        python-pyqt5: for the Python bindings
    ( 69/454) installing qt5-declarative                                            [##############################################] 100%
    ( 70/454) installing qt5-wayland                                                [##############################################] 100%
    ( 71/454) installing kguiaddons                                                 [##############################################] 100%
    Optional dependencies for kguiaddons
        python-pyqt5: for the Python bindings
    ( 72/454) installing ki18n                                                      [##############################################] 100%
    Optional dependencies for ki18n
        python-pyqt5: for the Python bindings
        python: to compile .ts files [installed]
    ( 73/454) installing kconfigwidgets                                             [##############################################] 100%
    Optional dependencies for kconfigwidgets
        python-pyqt5: for the Python bindings
        perl: for preparetips5 [pending]
    ( 74/454) installing kitemviews                                                 [##############################################] 100%
    Optional dependencies for kitemviews
        python-pyqt5: for the Python bindings
    ( 75/454) installing karchive                                                   [##############################################] 100%
    ( 76/454) installing kiconthemes                                                [##############################################] 100%
    Optional dependencies for kiconthemes
        breeze-icons: fallback icon theme [pending]
    ( 77/454) installing kxmlgui                                                    [##############################################] 100%
    ( 78/454) installing kbookmarks                                                 [##############################################] 100%
    ( 79/454) installing libxslt                                                    [##############################################] 100%
    ( 80/454) installing libogg                                                     [##############################################] 100%
    ( 81/454) installing libvorbis                                                  [##############################################] 100%
    ( 82/454) installing libtool                                                    [##############################################] 100%
    ( 83/454) installing libasyncns                                                 [##############################################] 100%
    ( 84/454) installing opus                                                       [##############################################] 100%
    ( 85/454) installing speexdsp                                                   [##############################################] 100%
    ( 86/454) installing speex                                                      [##############################################] 100%
    ( 87/454) installing flac                                                       [##############################################] 100%
    ( 88/454) installing libsndfile                                                 [##############################################] 100%
    Optional dependencies for libsndfile
        alsa-lib: for sndfile-play [installed]
    ( 89/454) installing libpulse                                                   [##############################################] 100%
    Optional dependencies for libpulse
        glib2: mainloop integration [installed]
    ( 90/454) installing tdb                                                        [##############################################] 100%
    Optional dependencies for tdb
        python: for python bindings [installed]
    ( 91/454) installing sound-theme-freedesktop                                    [##############################################] 100%
    ( 92/454) installing libcanberra                                                [##############################################] 100%
    ( 93/454) installing libdbusmenu-qt5                                            [##############################################] 100%
    ( 94/454) installing gstreamer                                                  [##############################################] 100%
    ( 95/454) installing orc                                                        [##############################################] 100%
    ( 96/454) installing iso-codes                                                  [##############################################] 100%
    ( 97/454) installing gst-plugins-base-libs                                      [##############################################] 100%
    ( 98/454) installing cdparanoia                                                 [##############################################] 100%
    ( 99/454) installing libvisual                                                  [##############################################] 100%
    (100/454) installing libtheora                                                  [##############################################] 100%
    (101/454) installing libdatrie                                                  [##############################################] 100%
    (102/454) installing libthai                                                    [##############################################] 100%
    (103/454) installing cairo                                                      [##############################################] 100%
    (104/454) installing fribidi                                                    [##############################################] 100%
    (105/454) installing pango                                                      [##############################################] 100%
    (106/454) installing graphene                                                   [##############################################] 100%
    (107/454) installing gst-plugins-base                                           [##############################################] 100%
    (108/454) installing openal                                                     [##############################################] 100%
    Optional dependencies for openal
        qt5-base: alsoft-config GUI Configurator [installed]
        fluidsynth: MIDI rendering
        libmysofa: makemhr tool
    (109/454) installing qt5-multimedia                                             [##############################################] 100%
    Optional dependencies for qt5-multimedia
        qt5-declarative: QML bindings [installed]
        gst-plugins-good: camera support, additional plugins
        gst-plugins-bad: camera support, additional plugins
        gst-plugins-ugly: additional plugins
        gst-libav: ffmpeg plugin
    (110/454) installing qt5-speech                                                 [##############################################] 100%
    Optional dependencies for qt5-speech
        flite: flite TTS backend
        speech-dispatcher: speech-dispatcher TTS backend
    (111/454) installing knotifications                                             [##############################################] 100%
    (112/454) installing kservice                                                   [##############################################] 100%
    (113/454) installing kwallet                                                    [##############################################] 100%
    Optional dependencies for kwallet
        kwalletmanager: Configuration GUI [pending]
    (114/454) installing kcompletion                                                [##############################################] 100%
    Optional dependencies for kcompletion
        python-pyqt5: for the Python bindings
    (115/454) installing sonnet                                                     [##############################################] 100%
    Optional dependencies for sonnet
        hunspell: spell checking via hunspell
        aspell: spell checking via aspell
        hspell: spell checking for Hebrew
        libvoikko: Finnish support via Voikko
    (116/454) installing ktextwidgets                                               [##############################################] 100%
    (117/454) installing kded                                                       [##############################################] 100%
    (118/454) installing kio                                                        [##############################################] 100%
    Optional dependencies for kio
        kio-extras: extra protocols support (sftp, fish and more) [pending]
        kdoctools: for the help kioslave [pending]
        kio-fuse: to mount remote filesystems via FUSE [pending]
    (119/454) installing kpackage                                                   [##############################################] 100%
    (120/454) installing kdeclarative                                               [##############################################] 100%
    (121/454) installing bluedevil                                                  [##############################################] 100%
    Optional dependencies for bluedevil
        pulseaudio-bluetooth: to connect to A2DP profile
    (122/454) installing libxss                                                     [##############################################] 100%
    (123/454) installing kidletime                                                  [##############################################] 100%
    (124/454) installing syntax-highlighting                                        [##############################################] 100%
    (125/454) installing source-highlight                                           [##############################################] 100%
    Optional dependencies for source-highlight
        lesspipe: src-hilite-lesspipe.sh
    (126/454) installing perl                                                       [##############################################] 100%
    (127/454) installing texinfo                                                    [##############################################] 100%
    (128/454) installing gc                                                         [##############################################] 100%
    (129/454) installing guile                                                      [##############################################] 100%
    (130/454) installing gdb-common                                                 [##############################################] 100%
    (131/454) installing gdb                                                        [##############################################] 100%
    (132/454) installing drkonqi                                                    [##############################################] 100%
    (133/454) installing kdecoration                                                [##############################################] 100%
    (134/454) installing kde-gtk-config                                             [##############################################] 100%
    Optional dependencies for kde-gtk-config
        gtk2: GTK2 apps support
        gtk3: GTK3 apps support
        xsettingsd: apply settings to GTK applications on the fly
    (135/454) installing knotifyconfig                                              [##############################################] 100%
    (136/454) installing libxres                                                    [##############################################] 100%
    (137/454) installing qt5-webchannel                                             [##############################################] 100%
    (138/454) installing qt5-location                                               [##############################################] 100%
    (139/454) installing libevent                                                   [##############################################] 100%
    Optional dependencies for libevent
        python: to use event_rpcgen.py [installed]
    (140/454) installing snappy                                                     [##############################################] 100%
    (141/454) installing minizip                                                    [##############################################] 100%
    (142/454) installing aom                                                        [##############################################] 100%
    (143/454) installing gsm                                                        [##############################################] 100%
    (144/454) installing celt                                                       [##############################################] 100%
    (145/454) installing libsamplerate                                              [##############################################] 100%
    (146/454) installing zita-alsa-pcmi                                             [##############################################] 100%
    (147/454) installing zita-resampler                                             [##############################################] 100%
    Optional dependencies for zita-resampler
        libsndfile: for zresample and zretune [installed]
    (148/454) installing jack2                                                      [##############################################] 100%
    Optional dependencies for jack2
        a2jmidid: for ALSA MIDI to JACK MIDI bridging
        libffado: for firewire support using FFADO
        jack2-dbus: for dbus integration
        realtime-privileges: for realtime privileges
        zita-ajbridge: for using multiple ALSA devices
    (149/454) installing lame                                                       [##############################################] 100%
    (150/454) installing libass                                                     [##############################################] 100%
    (151/454) installing libraw1394                                                 [##############################################] 100%
    (152/454) installing libavc1394                                                 [##############################################] 100%
    (153/454) installing libbluray                                                  [##############################################] 100%
    Optional dependencies for libbluray
        java-runtime: BD-J library
    (154/454) installing dav1d                                                      [##############################################] 100%
    Optional dependencies for dav1d
        dav1d-doc: HTML documentation
    (155/454) installing libiec61883                                                [##############################################] 100%
    (156/454) installing libmfx                                                     [##############################################] 100%
    (157/454) installing libmodplug                                                 [##############################################] 100%
    (158/454) installing rav1e                                                      [##############################################] 100%
    (159/454) installing gdk-pixbuf2                                                [##############################################] 100%
    Optional dependencies for gdk-pixbuf2
        libwmf: Load .wmf and .apm
        libopenraw: Load .dng, .cr2, .crw, .nef, .orf, .pef, .arw, .erf, .mrw, and .raf
        libavif: Load .avif [pending]
        libheif: Load .heif, .heic, and .avif [pending]
        librsvg: Load .svg, .svgz, and .svg.gz [pending]
        webp-pixbuf-loader: Load .webp
    (160/454) installing librsvg                                                    [##############################################] 100%
    (161/454) installing libsoxr                                                    [##############################################] 100%
    (162/454) installing libssh                                                     [##############################################] 100%
    (163/454) installing libva                                                      [##############################################] 100%
    Optional dependencies for libva
        intel-media-driver: backend for Intel GPUs (>= Broadwell)
        libva-vdpau-driver: backend for Nvidia and AMD GPUs
        libva-intel-driver: backend for Intel GPUs (<= Haswell)
    (164/454) installing libvdpau                                                   [##############################################] 100%
    (165/454) installing vid.stab                                                   [##############################################] 100%
    (166/454) installing libvpx                                                     [##############################################] 100%
    (167/454) installing giflib                                                     [##############################################] 100%
    (168/454) installing libwebp                                                    [##############################################] 100%
    Optional dependencies for libwebp
        freeglut: vwebp viewer
    (169/454) installing l-smash                                                    [##############################################] 100%
    (170/454) installing x264                                                       [##############################################] 100%
    (171/454) installing x265                                                       [##############################################] 100%
    (172/454) installing xvidcore                                                   [##############################################] 100%
    (173/454) installing zimg                                                       [##############################################] 100%
    (174/454) installing opencore-amr                                               [##############################################] 100%
    (175/454) installing lcms2                                                      [##############################################] 100%
    (176/454) installing openjpeg2                                                  [##############################################] 100%
    (177/454) installing libibus                                                    [##############################################] 100%
    (178/454) installing hidapi                                                     [##############################################] 100%
    Optional dependencies for hidapi
        libusb: for the libusb backend -- hidapi-libusb.so [installed]
        libudev.so: for the hidraw backend -- hidapi-hidraw.so [installed]
    (179/454) installing sdl2                                                       [##############################################] 100%
    Optional dependencies for sdl2
        alsa-lib: ALSA audio driver [installed]
        libpulse: PulseAudio audio driver [installed]
        jack: JACK audio driver [installed]
        pipewire: PipeWire audio driver [pending]
        libdecor: Wayland client decorations
    (180/454) installing srt                                                        [##############################################] 100%
    (181/454) installing svt-av1                                                    [##############################################] 100%
    (182/454) installing hicolor-icon-theme                                         [##############################################] 100%
    (183/454) installing sysfsutils                                                 [##############################################] 100%
    (184/454) installing v4l-utils                                                  [##############################################] 100%
    Optional dependencies for v4l-utils
        qt5-base: for qv4l2 [installed]
        alsa-lib: for qv4l2 [installed]
    (185/454) installing vmaf                                                       [##############################################] 100%
    (186/454) installing ffmpeg                                                     [##############################################] 100%
    Optional dependencies for ffmpeg
        avisynthplus: AviSynthPlus support
        intel-media-sdk: Intel QuickSync support
        ladspa: LADSPA filters
        nvidia-utils: Nvidia NVDEC/NVENC support
    (187/454) installing re2                                                        [##############################################] 100%
    (188/454) installing gnu-free-fonts                                             [##############################################] 100%
    (189/454) installing noto-fonts                                                 [##############################################] 100%
    Optional dependencies for noto-fonts
        noto-fonts-cjk: CJK characters
        noto-fonts-emoji: Emoji characters
        noto-fonts-extra: additional variants (condensed, semi-bold, extra-light)
    (190/454) installing qt5-webengine                                              [##############################################] 100%
    Optional dependencies for qt5-webengine
        libpipewire02: WebRTC desktop sharing under Wayland
    (191/454) installing attica                                                     [##############################################] 100%
    (192/454) installing syndication                                                [##############################################] 100%
    (193/454) installing knewstuff                                                  [##############################################] 100%
    Optional dependencies for knewstuff
        kirigami2: QML components [pending]
    (194/454) installing libksysguard                                               [##############################################] 100%
    (195/454) installing ksystemstats                                               [##############################################] 100%
    Optional dependencies for ksystemstats
        networkmanager-qt: network usage monitor [pending]
    (196/454) installing kparts                                                     [##############################################] 100%
    (197/454) installing http-parser                                                [##############################################] 100%
    (198/454) installing libgit2                                                    [##############################################] 100%
    (199/454) installing editorconfig-core-c                                        [##############################################] 100%
    (200/454) installing ktexteditor                                                [##############################################] 100%
    (201/454) installing libqalculate                                               [##############################################] 100%
    Optional dependencies for libqalculate
        gnuplot: for plotting support
    (202/454) installing libutempter                                                [##############################################] 100%
    (203/454) installing kpty                                                       [##############################################] 100%
    (204/454) installing kdesu                                                      [##############################################] 100%
    (205/454) installing kcmutils                                                   [##############################################] 100%
    (206/454) installing kactivities                                                [##############################################] 100%
    Optional dependencies for kactivities
        qt5-declarative: QML bindings [installed]
    (207/454) installing kde-cli-tools                                              [##############################################] 100%
    Optional dependencies for kde-cli-tools
        plasma-workspace: for kcmshell5 [pending]
    (208/454) installing libstemmer                                                 [##############################################] 100%
    (209/454) installing lmdb                                                       [##############################################] 100%
    (210/454) installing dconf                                                      [##############################################] 100%
    (211/454) installing cantarell-fonts                                            [##############################################] 100%
    (212/454) installing adobe-source-code-pro-fonts                                [##############################################] 100%
    (213/454) installing gsettings-desktop-schemas                                  [##############################################] 100%
    (214/454) installing glib-networking                                            [##############################################] 100%
    (215/454) installing libsoup                                                    [##############################################] 100%
    Optional dependencies for libsoup
        samba: Windows Domain SSO
    (216/454) installing appstream                                                  [##############################################] 100%
    (217/454) installing appstream-qt                                               [##############################################] 100%
    (218/454) installing kactivitymanagerd                                          [##############################################] 100%
    (219/454) installing kholidays                                                  [##############################################] 100%
    Optional dependencies for kholidays
        qt5-declarative: QML bindings [installed]
    (220/454) installing xorg-xmessage                                              [##############################################] 100%
    (221/454) installing kwayland                                                   [##############################################] 100%
    (222/454) installing qt5-quickcontrols                                          [##############################################] 100%
    (223/454) installing qt5-quickcontrols2                                         [##############################################] 100%
    Optional dependencies for qt5-quickcontrols2
        qt5-graphicaleffects: for the Material style [pending]
    (224/454) installing qt5-graphicaleffects                                       [##############################################] 100%
    (225/454) installing kirigami2                                                  [##############################################] 100%
    (226/454) installing plasma-framework                                           [##############################################] 100%
    (227/454) installing threadweaver                                               [##############################################] 100%
    (228/454) installing krunner                                                    [##############################################] 100%
    (229/454) installing kitemmodels                                                [##############################################] 100%
    Optional dependencies for kitemmodels
        python-pyqt5: for the Python bindings
        qt5-declarative: QML bindings [installed]
    (230/454) installing milou                                                      [##############################################] 100%
    (231/454) installing libdmtx                                                    [##############################################] 100%
    (232/454) installing qrencode                                                   [##############################################] 100%
    (233/454) installing prison                                                     [##############################################] 100%
    Optional dependencies for prison
        qt5-declarative: QML bindings [installed]
    (234/454) installing layer-shell-qt                                             [##############################################] 100%
    (235/454) installing kscreenlocker                                              [##############################################] 100%
    Optional dependencies for kscreenlocker
        kcmutils: configuration module [installed]
    (236/454) installing xcb-util-cursor                                            [##############################################] 100%
    (237/454) installing kwayland-server                                            [##############################################] 100%
    (238/454) installing frameworkintegration                                       [##############################################] 100%
    Optional dependencies for frameworkintegration
        appstream-qt: dependency resolving via AppStream [installed]
        packagekit-qt5: dependency resolving via AppStream
    (239/454) installing breeze-icons                                               [##############################################] 100%
    (240/454) installing breeze                                                     [##############################################] 100%
    Optional dependencies for breeze
        breeze-gtk: Breeze widget style for GTK applications [pending]
        kcmutils: for breeze-settings [installed]
    (241/454) installing rtkit                                                      [##############################################] 100%
    (242/454) installing alsa-card-profiles                                         [##############################################] 100%
    (243/454) installing bluez-libs                                                 [##############################################] 100%
    (244/454) installing sbc                                                        [##############################################] 100%
    (245/454) installing libldac                                                    [##############################################] 100%
    (246/454) installing libfreeaptx                                                [##############################################] 100%
    (247/454) installing libfdk-aac                                                 [##############################################] 100%
    (248/454) installing webrtc-audio-processing                                    [##############################################] 100%
    (249/454) installing pipewire                                                   [##############################################] 100%
    Created symlink /etc/systemd/user/sockets.target.wants/pipewire.socket → /usr/lib/systemd/user/pipewire.socket.
    Optional dependencies for pipewire
        pipewire-docs: Documentation
        pipewire-media-session: Default session manager [pending]
        pipewire-alsa: ALSA configuration
        pipewire-jack: JACK support
        pipewire-pulse: PulseAudio replacement
        gst-plugin-pipewire: GStreamer support
        pipewire-zeroconf: Zeroconf support
    (250/454) installing pipewire-media-session                                     [##############################################] 100%
    Created symlink /etc/systemd/user/pipewire-session-manager.service → /usr/lib/systemd/user/pipewire-media-session.service.
    Created symlink /etc/systemd/user/pipewire.service.wants/pipewire-media-session.service → /usr/lib/systemd/user/pipewire-media-session.service.
    (251/454) installing libqaccessibilityclient                                    [##############################################] 100%
    (252/454) installing kwin                                                       [##############################################] 100%
    Optional dependencies for kwin
        maliit-keyboard: virtual keyboard for kwin-wayland
    (253/454) installing ttf-hack                                                   [##############################################] 100%
    (254/454) installing qqc2-desktop-style                                         [##############################################] 100%
    (255/454) installing plasma-integration                                         [##############################################] 100%
    (256/454) installing kpeople                                                    [##############################################] 100%
    Optional dependencies for kpeople
        qt5-declarative: QML bindings [installed]
    (257/454) installing kactivities-stats                                          [##############################################] 100%
    (258/454) installing libkscreen                                                 [##############################################] 100%
    (259/454) installing kquickcharts                                               [##############################################] 100%
    (260/454) installing kuserfeedback                                              [##############################################] 100%
    Optional dependencies for kuserfeedback
        qt5-declarative: QML bindings [installed]
        qt5-charts: User Feedback console
        qt5-svg: User Feedback console [installed]
    (261/454) installing kdnssd                                                     [##############################################] 100%
    (262/454) installing talloc                                                     [##############################################] 100%
    Optional dependencies for talloc
        python: for python bindings [installed]
    (263/454) installing cifs-utils                                                 [##############################################] 100%
    (264/454) installing tevent                                                     [##############################################] 100%
    Optional dependencies for tevent
        python: for python bindings [installed]
    (265/454) installing ldb                                                        [##############################################] 100%
    Optional dependencies for ldb
        python: for python bindings [installed]
    (266/454) installing python-markdown                                            [##############################################] 100%
    (267/454) installing python-dnspython                                           [##############################################] 100%
    Optional dependencies for python-dnspython
        python-cryptography: DNSSEC support
        python-requests-toolbelt: DoH support
        python-idna: support for updated IDNA 2008
        python-curio: async support
        python-trio: async support
        python-sniffio: async support
    (268/454) installing libmd                                                      [##############################################] 100%
    (269/454) installing libbsd                                                     [##############################################] 100%
    (270/454) installing jansson                                                    [##############################################] 100%
    (271/454) installing smbclient                                                  [##############################################] 100%
    Optional dependencies for smbclient
        python-dnspython: samba_dnsupdate and samba_upgradedns in AD setup [installed]
    (272/454) installing libmtp                                                     [##############################################] 100%
    (273/454) installing phonon-qt5-gstreamer                                       [##############################################] 100%
    Optional dependencies for phonon-qt5-gstreamer
        pulseaudio: PulseAudio support [pending]
        gst-plugins-good: PulseAudio support and good codecs
        gst-plugins-bad: additional codecs
        gst-plugins-ugly: additional codecs
        gst-libav: libav codec
    (274/454) installing phonon-qt5                                                 [##############################################] 100%
    Optional dependencies for phonon-qt5
        pulseaudio: PulseAudio support [pending]
        qt5-tools: Designer plugin [pending]
    (275/454) installing kdsoap                                                     [##############################################] 100%
    (276/454) installing kdsoap-ws-discovery-client                                 [##############################################] 100%
    (277/454) installing kio-extras                                                 [##############################################] 100%
    Optional dependencies for kio-extras
        qt5-imageformats: thumbnails for additional image formats
        perl: info kioslave [installed]
        kimageformats: thumbnails for additional image formats
        taglib: audio file thumbnails [pending]
        libappimage: AppImage thumbnails
        icoutils: Windows executable thumbnails
        openexr: EXR format thumbnails
        kactivities-stats: recently used kioslave [installed]
    (278/454) installing fuse3                                                      [##############################################] 100%
    (279/454) installing kio-fuse                                                   [##############################################] 100%
    (280/454) installing plasma-workspace                                           [##############################################] 100%
    Optional dependencies for plasma-workspace
        plasma-workspace-wallpapers: additional wallpapers [pending]
        gpsd: GPS based geolocation
        networkmanager-qt: IP based geolocation [pending]
        kdepim-addons: displaying PIM events in the calendar
        appmenu-gtk-module: global menu support for GTK2 and some GTK3 applications
        baloo: Baloo search runner [pending]
        discover: manage applications installation from the launcher [pending]
    (281/454) installing kunitconversion                                            [##############################################] 100%
    (282/454) installing kdeplasma-addons                                           [##############################################] 100%
    Optional dependencies for kdeplasma-addons
        kross: comic applet
        purpose: Quickshare applet [pending]
        quota-tools: disk quota applet
        qt5-webengine: dictionary and webbrowser applets [installed]
    (283/454) installing kemoticons                                                 [##############################################] 100%
    (284/454) installing kdelibs4support                                            [##############################################] 100%
    (285/454) installing khotkeys                                                   [##############################################] 100%
    (286/454) installing systemsettings                                             [##############################################] 100%
    (287/454) installing glu                                                        [##############################################] 100%
    (288/454) installing kinfocenter                                                [##############################################] 100%
    (289/454) installing qt5-sensors                                                [##############################################] 100%
    Optional dependencies for qt5-sensors
        qt5-declarative: QML bindings [installed]
    (290/454) installing kscreen                                                    [##############################################] 100%
    (291/454) installing ksshaskpass                                                [##############################################] 100%
    (292/454) installing kwrited                                                    [##############################################] 100%
    (293/454) installing oxygen                                                     [##############################################] 100%
    Optional dependencies for oxygen
        kcmutils: for oxygen-settings5 [installed]
    (294/454) installing signond                                                    [##############################################] 100%
    (295/454) installing signon-kwallet-extension                                   [##############################################] 100%
    (296/454) installing signon-plugin-oauth2                                       [##############################################] 100%
    (297/454) installing dbus-glib                                                  [##############################################] 100%
    (298/454) installing libaccounts-glib                                           [##############################################] 100%
    (299/454) installing libaccounts-qt                                             [##############################################] 100%
    (300/454) installing libnotify                                                  [##############################################] 100%
    (301/454) installing signon-ui                                                  [##############################################] 100%
    (302/454) installing kaccounts-integration                                      [##############################################] 100%
    (303/454) installing accounts-qml-module                                        [##############################################] 100%
    (304/454) installing purpose                                                    [##############################################] 100%
    Optional dependencies for purpose
        kdeconnect: sharing to smartphone via KDE Connect
        telegram-desktop: sharing via Telegram
        bluedevil: sharing via Bluetooth [installed]
    (305/454) installing exiv2                                                      [##############################################] 100%
    (306/454) installing poppler                                                    [##############################################] 100%
    Optional dependencies for poppler
        poppler-data: highly recommended encoding data to display PDF documents with certain encodings and characters
    (307/454) installing poppler-qt5                                                [##############################################] 100%
    (308/454) installing taglib                                                     [##############################################] 100%
    (309/454) installing libzip                                                     [##############################################] 100%
    (310/454) installing libtommath                                                 [##############################################] 100%
    (311/454) installing convertlit                                                 [##############################################] 100%
    (312/454) installing ebook-tools                                                [##############################################] 100%
    (313/454) installing kfilemetadata                                              [##############################################] 100%
    Optional dependencies for kfilemetadata
        libappimage: AppImage extractor
    (314/454) installing plasma-browser-integration                                 [##############################################] 100%
    (315/454) installing polkit-kde-agent                                           [##############################################] 100%
    (316/454) installing kmenuedit                                                  [##############################################] 100%
    (317/454) installing baloo                                                      [##############################################] 100%
    Optional dependencies for baloo
        qt5-declarative: QML bindings [installed]
    (318/454) installing accountsservice                                            [##############################################] 100%
    (319/454) installing xdg-user-dirs                                              [##############################################] 100%
    Created symlink /etc/systemd/user/default.target.wants/xdg-user-dirs-update.service → /usr/lib/systemd/user/xdg-user-dirs-update.service.
    (320/454) installing plasma-desktop                                             [##############################################] 100%
    Optional dependencies for plasma-desktop
        plasma-nm: Network manager applet [pending]
        powerdevil: power management, suspend and hibernate support [pending]
        kscreen: screen management [installed]
        ibus: kimpanel IBUS support
        scim: kimpanel SCIM support
        kaccounts-integration: OpenDesktop integration plugin [installed]
        packagekit-qt5: to install new krunner plugins
    (321/454) installing smartmontools                                              [##############################################] 100%
    Optional dependencies for smartmontools
        s-nail: to get mail alerts to work
    (322/454) installing plasma-disks                                               [##############################################] 100%
    (323/454) installing plasma-firewall                                            [##############################################] 100%
    Optional dependencies for plasma-firewall
        iproute2: netstat backend [installed]
        firewalld: firewalld backend
        ufw: ufw backend
    (324/454) installing ppp                                                        [##############################################] 100%
    (325/454) installing libmbim                                                    [##############################################] 100%
    (326/454) installing libqrtr-glib                                               [##############################################] 100%
    (327/454) installing libqmi                                                     [##############################################] 100%
    (328/454) installing mobile-broadband-provider-info                             [##############################################] 100%
    (329/454) installing libmm-glib                                                 [##############################################] 100%
    (330/454) installing modemmanager                                               [##############################################] 100%
    Optional dependencies for modemmanager
        usb_modeswitch: install if your modem shows up as a storage drive
    (331/454) installing modemmanager-qt                                            [##############################################] 100%
    (332/454) installing libnm                                                      [##############################################] 100%
    (333/454) installing wpa_supplicant                                             [##############################################] 100%
    (334/454) installing gpm                                                        [##############################################] 100%
    (335/454) installing slang                                                      [##############################################] 100%
    (336/454) installing libnewt                                                    [##############################################] 100%
    Optional dependencies for libnewt
        tcl: whiptcl support
        python: libnewt support with the _snack module [installed]
        python2: libnewt support with the _snack module
    (337/454) installing libndp                                                     [##############################################] 100%
    (338/454) installing libsodium                                                  [##############################################] 100%
    (339/454) installing libpgm                                                     [##############################################] 100%
    (340/454) installing zeromq                                                     [##############################################] 100%
    (341/454) installing libteam                                                    [##############################################] 100%
    (342/454) installing networkmanager                                             [##############################################] 100%
    Optional dependencies for networkmanager
        dnsmasq: connection sharing
        nftables: connection sharing
        iptables: connection sharing [installed]
        bluez: Bluetooth support [installed]
        ppp: dialup connection support [installed]
        modemmanager: cellular network support [installed]
        iwd: wpa_supplicant alternative
        dhclient: alternative DHCP client
        dhcpcd: alternative DHCP client
        openresolv: alternative resolv.conf manager
        firewalld: firewall support
    (343/454) installing networkmanager-qt                                          [##############################################] 100%
    (344/454) installing qca-qt5                                                    [##############################################] 100%
    Optional dependencies for qca-qt5
        pkcs11-helper: PKCS-11 plugin
        botan: botan plugin
    (345/454) installing plasma-nm                                                  [##############################################] 100%
    Optional dependencies for plasma-nm
        openconnect: Cisco AnyConnect VPN plugin
    (346/454) installing plasma-workspace-wallpapers                                [##############################################] 100%
    (347/454) installing pulseaudio                                                 [##############################################] 100%
    Created symlink /etc/systemd/user/sockets.target.wants/pulseaudio.socket → /usr/lib/systemd/user/pulseaudio.socket.
    Optional dependencies for pulseaudio
        pulseaudio-alsa: ALSA configuration (recommended)
        pulseaudio-zeroconf: Zeroconf support
        pulseaudio-lirc: IR (lirc) support
        pulseaudio-jack: Jack support
        pulseaudio-bluetooth: Bluetooth support
        pulseaudio-equalizer: Graphical equalizer
        pulseaudio-rtp: RTP and RAOP support
    (348/454) installing plasma-pa                                                  [##############################################] 100%
    (349/454) installing plasma-sdk                                                 [##############################################] 100%
    (350/454) installing plasma-systemmonitor                                       [##############################################] 100%
    (351/454) installing bolt                                                       [##############################################] 100%
    (352/454) installing plasma-thunderbolt                                         [##############################################] 100%
    (353/454) installing plasma-vault                                               [##############################################] 100%
    Optional dependencies for plasma-vault
        encfs: to use encFS for encryption
        cryfs: to use cryFS for encryption
        gocryptfs: to use gocryptfs for encryption
    (354/454) installing kwayland-integration                                       [##############################################] 100%
    (355/454) installing socat                                                      [##############################################] 100%
    (356/454) installing kwallet-pam                                                [##############################################] 100%
    (357/454) installing kgamma5                                                    [##############################################] 100%
    (358/454) installing sddm                                                       [##############################################] 100%
    (359/454) installing sddm-kcm                                                   [##############################################] 100%
    (360/454) installing breeze-gtk                                                 [##############################################] 100%
    (361/454) installing powerdevil                                                 [##############################################] 100%
    Optional dependencies for powerdevil
        kinfocenter: for the Energy Information KCM [installed]
    (362/454) installing archlinux-appstream-data                                   [##############################################] 100%
    (363/454) installing discount                                                   [##############################################] 100%
    (364/454) installing discover                                                   [##############################################] 100%
    Optional dependencies for discover
        packagekit-qt5: to manage packages from Arch Linux repositories
        flatpak: Flatpak packages support
        fwupd: firmware update support
    (365/454) installing xdg-desktop-portal-kde                                     [##############################################] 100%
    (366/454) installing plasma-meta                                                [##############################################] 100%
    Optional dependencies for plasma-meta
        breeze-grub: Breeze theme for GRUB
    (367/454) installing ark                                                        [##############################################] 100%
    Optional dependencies for ark
        p7zip: 7Z format support
        unrar: RAR decompression support
        unarchiver: RAR format support
        lzop: LZO format support
        lrzip: LRZ format support
    (368/454) installing filelight                                                  [##############################################] 100%
    (369/454) installing kate                                                       [##############################################] 100%
    Optional dependencies for kate
        konsole: open a terminal in Kate [pending]
        clang: C and C++ LSP support
        python-language-server: Python LSP support
        texlab: LaTeX LSP support
        rust: Rust LSP support
        git: git-blame plugin
    (370/454) installing kbackup                                                    [##############################################] 100%
    (371/454) installing kcalc                                                      [##############################################] 100%
    (372/454) installing kcharselect                                                [##############################################] 100%
    (373/454) installing kdebugsettings                                             [##############################################] 100%
    (374/454) installing kdf                                                        [##############################################] 100%
    (375/454) installing kdialog                                                    [##############################################] 100%
    (376/454) installing keditbookmarks                                             [##############################################] 100%
    (377/454) installing kfind                                                      [##############################################] 100%
    Optional dependencies for kfind
        mlocate: search using mlocate index
    (378/454) installing kfloppy                                                    [##############################################] 100%
    (379/454) installing libakonadi                                                 [##############################################] 100%
    (380/454) installing kcontacts                                                  [##############################################] 100%
    (381/454) installing kmime                                                      [##############################################] 100%
    (382/454) installing grantlee                                                   [##############################################] 100%
    (383/454) installing grantleetheme                                              [##############################################] 100%
    (384/454) installing qgpgme                                                     [##############################################] 100%
    (385/454) installing kpimtextedit                                               [##############################################] 100%
    (386/454) installing libkleo                                                    [##############################################] 100%
    (387/454) installing akonadi-contacts                                           [##############################################] 100%
    (388/454) installing kgpg                                                       [##############################################] 100%
    (389/454) installing konsole                                                    [##############################################] 100%
    Optional dependencies for konsole
        keditbookmarks: to manage bookmarks [installed]
    (390/454) installing kteatime                                                   [##############################################] 100%
    (391/454) installing ktimer                                                     [##############################################] 100%
    (392/454) installing kwalletmanager                                             [##############################################] 100%
    (393/454) installing kwrite                                                     [##############################################] 100%
    (394/454) installing markdownpart                                               [##############################################] 100%
    (395/454) installing print-manager                                              [##############################################] 100%
    Optional dependencies for print-manager
        system-config-printer: auto-detect the printer driver
    (396/454) installing sweeper                                                    [##############################################] 100%
    (397/454) installing yakuake                                                    [##############################################] 100%
    (398/454) installing baloo-widgets                                              [##############################################] 100%
    (399/454) installing dolphin                                                    [##############################################] 100%
    Optional dependencies for dolphin
        kde-cli-tools: for editing file type options [installed]
        ffmpegthumbs: video thumbnails
        kdegraphics-thumbnailers: PDF and PS thumbnails [pending]
        konsole: terminal panel [installed]
        purpose: share context menu [installed]
    (400/454) installing run-parts                                                  [##############################################] 100%
    (401/454) installing cronie                                                     [##############################################] 100%
    Optional dependencies for cronie
        smtp-server: send job output via email
        smtp-forwarder: forward job output to email server
    (402/454) installing kcron                                                      [##############################################] 100%
    (403/454) installing kjs                                                        [##############################################] 100%
    (404/454) installing khtml                                                      [##############################################] 100%
    (405/454) installing docbook-xml                                                [##############################################] 100%
    (406/454) installing docbook-xsl                                                [##############################################] 100%
    (407/454) installing kdoctools                                                  [##############################################] 100%
    (408/454) installing xapian-core                                                [##############################################] 100%
    (409/454) installing khelpcenter                                                [##############################################] 100%
    (410/454) installing ksystemlog                                                 [##############################################] 100%
    (411/454) installing kpmcore                                                    [##############################################] 100%
    Optional dependencies for kpmcore
        e2fsprogs: ext2/3/4 support [installed]
        xfsprogs: XFS support [installed]
        jfsutils: JFS support
        reiserfsprogs: Reiser support
        ntfs-3g: NTFS support [installed]
        dosfstools: FAT32 support [installed]
        fatresize: FAT resize support
        f2fs-tools: F2FS support
        exfat-utils: exFAT support
        exfatprogs: exFAT support (alternative to exfat-utils)
        nilfs-utils: nilfs support
        udftools: UDF support
    (412/454) installing partitionmanager                                           [##############################################] 100%
    (413/454) installing dolphin-plugins                                            [##############################################] 100%
    Optional dependencies for dolphin-plugins
        ktexteditor: Mercurial plugin [installed]
    (414/454) installing libkipi                                                    [##############################################] 100%
    (415/454) installing jasper                                                     [##############################################] 100%
    Optional dependencies for jasper
        jasper-doc: documentation
        freeglut: jiv support
        glu: jiv support [installed]
    (416/454) installing libraw                                                     [##############################################] 100%
    (417/454) installing libkdcraw                                                  [##############################################] 100%
    (418/454) installing cfitsio                                                    [##############################################] 100%
    (419/454) installing gwenview                                                   [##############################################] 100%
    Optional dependencies for gwenview
        qt5-imageformats: support for tiff, webp, and more image formats
        kimageformats: support for dds, xcf, exr, psd, and more image formats
        kipi-plugins: export to various online services [pending]
        kamera: import pictures from gphoto2 cameras [pending]
    (420/454) installing libexif                                                    [##############################################] 100%
    (421/454) installing libyuv                                                     [##############################################] 100%
    (422/454) installing libavif                                                    [##############################################] 100%
    (423/454) installing libde265                                                   [##############################################] 100%
    Optional dependencies for libde265
        ffmpeg: for sherlock265 [installed]
        qt5-base: for sherlock265 [installed]
        sdl: dec265 YUV overlay output
    (424/454) installing libheif                                                    [##############################################] 100%
    Optional dependencies for libheif
        libjpeg: for heif-convert and heif-enc [installed]
        libpng: for heif-convert and heif-enc [installed]
    (425/454) installing gd                                                         [##############################################] 100%
    Optional dependencies for gd
        perl: bdftogd script [installed]
    (426/454) installing libgphoto2                                                 [##############################################] 100%
    (427/454) installing kamera                                                     [##############################################] 100%
    (428/454) installing kcolorchooser                                              [##############################################] 100%
    (429/454) installing kdegraphics-mobipocket                                     [##############################################] 100%
    (430/454) installing libkexiv2                                                  [##############################################] 100%
    (431/454) installing jbig2dec                                                   [##############################################] 100%
    (432/454) installing libpaper                                                   [##############################################] 100%
    (433/454) installing ijs                                                        [##############################################] 100%
    (434/454) installing libidn                                                     [##############################################] 100%
    (435/454) installing ghostscript                                                [##############################################] 100%
    Optional dependencies for ghostscript
        texlive-core: needed for dvipdf
        gtk3: needed for gsx
    (436/454) installing kdegraphics-thumbnailers                                   [##############################################] 100%
    (437/454) installing kimagemapeditor                                            [##############################################] 100%
    (438/454) installing kipi-plugins                                               [##############################################] 100%
    Optional dependencies for kipi-plugins
        libmediawiki: MediaWiki Export plugin
        libkvkontakte: VKontakte.ru Exporter plugin
        qt5-xmlpatterns: rajce.net plugin
    (439/454) installing libieee1284                                                [##############################################] 100%
    Optional dependencies for libieee1284
        python: for python module [installed]
    (440/454) installing net-snmp                                                   [##############################################] 100%
    Optional dependencies for net-snmp
        perl-term-readkey: for snmpcheck application
        perl-tk: for snmpcheck and tkmib applications
        python: for the python modules [installed]
    (441/454) installing poppler-glib                                               [##############################################] 100%
    (442/454) installing sane                                                       [##############################################] 100%
    (443/454) installing libksane                                                   [##############################################] 100%
    (444/454) installing kolourpaint                                                [##############################################] 100%
    (445/454) installing kruler                                                     [##############################################] 100%
    (446/454) installing djvulibre                                                  [##############################################] 100%
    (447/454) installing libspectre                                                 [##############################################] 100%
    (448/454) installing okular                                                     [##############################################] 100%
    Optional dependencies for okular
        ebook-tools: mobi and epub support [installed]
        kdegraphics-mobipocket: mobi support [installed]
        libzip: CHM support [installed]
        khtml: CHM support [installed]
        chmlib: CHM support
        calligra: ODT and ODP support
        unrar: Comic Book Archive support
        unarchiver: Comic Book Archive support (alternative)
    (449/454) installing skanlite                                                   [##############################################] 100%
    (450/454) installing qt5-tools                                                  [##############################################] 100%
    Optional dependencies for qt5-tools
        clang: for qdoc
        qt5-webkit: for Qt Assistant
    (451/454) installing kcolorpicker                                               [##############################################] 100%
    (452/454) installing kimageannotator                                            [##############################################] 100%
    (453/454) installing spectacle                                                  [##############################################] 100%
    Optional dependencies for spectacle
        kipi-plugins: export to various online services [installed]
    (454/454) installing svgpart                                                    [##############################################] 100%
    :: Running post-transaction hooks...
    ( 1/19) Creating system user accounts...
    Creating group saned with gid 991.
    Creating user saned (SANE daemon user) with uid 991 and gid 991.
    Creating group usbmux with gid 140.
    ( 2/19) Reloading system manager configuration...
      Skipped: Running in chroot.
    ( 3/19) Updating udev hardware database...
    ( 4/19) Creating temporary files...
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to open file "/sys/devices/system/cpu/microcode/reload": Read-only file system
    error: command failed to execute correctly
    ( 5/19) Reloading device manager configuration...
      Skipped: Running in chroot.
    ( 6/19) Arming ConditionNeedsUpdate...
    ( 7/19) Updating fontconfig configuration...
    ( 8/19) Updating linux initcpios...
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'default'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts.img
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux-lts.preset: 'fallback'
      -> -k /boot/vmlinuz-linux-lts -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-lts-fallback.img -S autodetect
    ==> Starting build: 5.10.70-1-lts
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-lts-fallback.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'default'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux.img
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [autodetect]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux.img
    ==> Image generation successful
    ==> Building image from preset: /etc/mkinitcpio.d/linux.preset: 'fallback'
      -> -k /boot/vmlinuz-linux -c /etc/mkinitcpio.conf -g /boot/initramfs-linux-fallback.img -S autodetect
    ==> Starting build: 5.14.8-arch1-1
      -> Running build hook: [base]
      -> Running build hook: [udev]
      -> Running build hook: [modconf]
      -> Running build hook: [block]
    ==> WARNING: Possibly missing firmware for module: aic94xx
    ==> WARNING: Possibly missing firmware for module: wd719x
    ==> WARNING: Possibly missing firmware for module: xhci_pci
      -> Running build hook: [filesystems]
      -> Running build hook: [keyboard]
      -> Running build hook: [fsck]
    ==> Generating module dependencies
    ==> Creating zstd-compressed initcpio image: /boot/initramfs-linux-fallback.img
    ==> Image generation successful
    ( 9/19) Reloading system bus configuration...
      Skipped: Running in chroot.
    (10/19) Warn about old perl modules
    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
            LANGUAGE = "",
            LC_ALL = (unset),
            LANG = "en_US.UTF-8"
        are supported and installed on your system.
    perl: warning: Falling back to the standard locale ("C").
    (11/19) Updating fontconfig cache...
    (12/19) Probing GDK-Pixbuf loader modules...
    (13/19) Updating GIO module cache...
    (14/19) Compiling GSettings XML schema files...
    (15/19) Updating the info directory file...
    (16/19) Updating the appstream cache...
    AppStream system cache refresh failed. Turn on verbose mode to get detailed issue information.
    error: command failed to execute correctly
    (17/19) Updating the MIME type database...
    (18/19) Updating X fontdir indices...
    (19/19) Performing snapper post snapshots for the following configurations...

    Again note the optional dependencies and install those desired later to realize the functionality they. For example the packges indicated as
    sonnet dependencies are necessary for spell checking functionality.
    Install other desired programs.

    [root@G5-openSUSE /]# pacstrap /mnt fish starship neofetch reflector git

    Two of the packages I install at this point are
    reflector, useful for easily sorting package repository mirrors, and git, needed for installing packages manually from the AUR. I installed the base-devel group -- required by pacman wrappers for AUR support -- later after booting into the installed system with a working desktop environment, but now would be a good time to intall it. The command with the output:

    [root@G5-openSUSE /]# pacstrap /mnt fish starship neofetch reflector git
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community is up to date
    resolving dependencies...
    looking for conflicting packages...

    Packages (9) perl-error-0.17029-3  perl-mailtools-2.21-5  perl-timedate-2.33-3  ttf-iosevka-nerd-2.1.0-1  fish-3.3.1-1  git-2.33.0-1
                 neofetch-7.1.0-2  reflector-2021.7.8-1  starship-0.58.0-2

    Total Download Size:    14.64 MiB
    Total Installed Size:  158.62 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...
     fish-3.3.1-1-x86_64                                  2.8 MiB  2.11 MiB/s 00:01 [##############################################] 100%
     ttf-iosevka-nerd-2.1.0-1-any                         4.2 MiB  3.50 MiB/s 00:01 [##############################################] 100%
     starship-0.58.0-2-x86_64                          1619.7 KiB  5.49 MiB/s 00:00 [##############################################] 100%
     neofetch-7.1.0-2-any                                83.1 KiB   561 KiB/s 00:00 [##############################################] 100%
     reflector-2021.7.8-1-any                            26.7 KiB   163 KiB/s 00:00 [##############################################] 100%
     perl-error-0.17029-3-any                            21.8 KiB   127 KiB/s 00:00 [##############################################] 100%
     perl-timedate-2.33-3-any                            35.8 KiB   229 KiB/s 00:00 [##############################################] 100%
     perl-mailtools-2.21-5-any                           62.2 KiB   379 KiB/s 00:00 [##############################################] 100%
     git-2.33.0-1-x86_64                                  5.9 MiB  3.75 MiB/s 00:02 [##############################################] 100%
     Total (9/9)                                         14.6 MiB  2.29 MiB/s 00:06 [##############################################] 100%
    (9/9) checking keys in keyring                                                  [##############################################] 100%
    (9/9) checking package integrity                                                [##############################################] 100%
    (9/9) loading package files                                                     [##############################################] 100%
    (9/9) checking for file conflicts                                               [##############################################] 100%
    (9/9) checking available disk space                                             [##############################################] 100%
    :: Running pre-transaction hooks...
    (1/1) Performing snapper pre snapshots for the following configurations...
    :: Processing package changes...
    (1/9) installing fish                                                           [##############################################] 100%
    Optional dependencies for fish
        python: man page completion parser / web config tool [installed]
        pkgfile: command-not-found hook
    (2/9) installing ttf-iosevka-nerd                                               [##############################################] 100%
    (3/9) installing starship                                                       [##############################################] 100%
    (4/9) installing neofetch                                                       [##############################################] 100%
    Optional dependencies for neofetch
        catimg: Display Images
        chafa: Image to text support
        feh: Wallpaper Display
        imagemagick: Image cropping / Thumbnail creation / Take a screenshot
        jp2a: Display Images
        libcaca: Display Images
        nitrogen: Wallpaper Display
        w3m: Display Images
        xdotool: See https://github.com/dylanaraps/neofetch/wiki/Images-in-the-terminal
        xorg-xdpyinfo: Resolution detection (Single Monitor) [installed]
        xorg-xprop: Desktop Environment and Window Manager [installed]
        xorg-xrandr: Resolution detection (Multi Monitor + Refresh rates) [installed]
        xorg-xwininfo: See https://github.com/dylanaraps/neofetch/wiki/Images-in-the-terminal [installed]
    (5/9) installing reflector                                                      [##############################################] 100%
    Optional dependencies for reflector
        rsync: rate rsync mirrors
    (6/9) installing perl-error                                                     [##############################################] 100%
    (7/9) installing perl-timedate                                                  [##############################################] 100%
    (8/9) installing perl-mailtools                                                 [##############################################] 100%
    (9/9) installing git                                                            [##############################################] 100%
    Optional dependencies for git
        tk: gitk and git gui
        perl-libwww: git svn
        perl-term-readkey: git svn and interactive.singlekey setting
        perl-io-socket-ssl: git send-email TLS support
        perl-authen-sasl: git send-email TLS support
        perl-mediawiki-api: git mediawiki support
        perl-datetime-format-iso8601: git mediawiki support
        perl-lwp-protocol-https: git mediawiki https support
        perl-cgi: gitweb (web interface) support
        python: git svn & git p4 [installed]
        subversion: git svn
        org.freedesktop.secrets: keyring credential helper
        libsecret: libsecret credential helper [installed]
    :: Running post-transaction hooks...
    (1/7) Creating system user accounts...
    Creating group git with gid 990.
    Creating user git (git daemon user) with uid 990 and gid 990.
    (2/7) Reloading system manager configuration...
      Skipped: Running in chroot.
    (3/7) Arming ConditionNeedsUpdate...
    (4/7) Warn about old perl modules
    perl: warning: Setting locale failed.
    perl: warning: Please check that your locale settings:
            LANGUAGE = "",
            LC_ALL = (unset),
            LANG = "en_US.UTF-8"
        are supported and installed on your system.
    perl: warning: Falling back to the standard locale ("C").
    (5/7) Updating fontconfig cache...
    (6/7) Updating X fontdir indices...
    (7/7) Performing snapper post snapshots for the following configurations...

    Install packages for accessing man and texinfo pages.

    [root@G5-openSUSE /]# pacstrap /mnt man-db man-pages texinfo

    The command with the output:

    [root@G5-openSUSE /]# pacstrap /mnt man-db man-pages texinfo
    ==> Creating install root at /mnt
    ==> Installing packages to /mnt
    :: Synchronizing package databases...
     core is up to date
     extra is up to date
     community is up to date
    warning: texinfo-6.8-2 is up to date -- reinstalling
    resolving dependencies...
    looking for conflicting packages...

    Packages (5) groff-1.22.4-6  libpipeline-1.5.3-1  man-db-2.9.4-2  man-pages-5.13-1  texinfo-6.8-2

    Total Download Size:    9.08 MiB
    Total Installed Size:  26.53 MiB
    Net Upgrade Size:      17.79 MiB

    :: Proceed with installation? [Y/n] 
    :: Retrieving packages...
     groff-1.22.4-6-x86_64                                2.1 MiB  2.59 MiB/s 00:01 [##############################################] 100%
     libpipeline-1.5.3-1-x86_64                          39.8 KiB   184 KiB/s 00:00 [##############################################] 100%
     man-db-2.9.4-2-x86_64                             1059.0 KiB  3.92 MiB/s 00:00 [##############################################] 100%
     man-pages-5.13-1-any                                 5.9 MiB  2.48 MiB/s 00:02 [##############################################] 100%
     Total (4/4)                                          9.1 MiB  2.09 MiB/s 00:04 [##############################################] 100%
    (5/5) checking keys in keyring                                                  [##############################################] 100%
    (5/5) checking package integrity                                                [##############################################] 100%
    (5/5) loading package files                                                     [##############################################] 100%
    (5/5) checking for file conflicts                                               [##############################################] 100%
    (5/5) checking available disk space                                             [##############################################] 100%
    :: Running pre-transaction hooks...
    (1/1) Performing snapper pre snapshots for the following configurations...
    :: Processing package changes...
    (1/5) installing groff                                                          [##############################################] 100%
    Optional dependencies for groff
        netpbm: for use together with man -H command interaction in browsers
        psutils: for use together with man -H command interaction in browsers
        libxaw: for gxditview [installed]
        perl-file-homedir: for use with glilypond
    (2/5) installing libpipeline                                                    [##############################################] 100%
    (3/5) installing man-db                                                         [##############################################] 100%
    Optional dependencies for man-db
        gzip [installed]
    (4/5) installing man-pages                                                      [##############################################] 100%
    (5/5) reinstalling texinfo                                                      [##############################################] 100%
    :: Running post-transaction hooks...
    (1/5) Reloading system manager configuration...
      Skipped: Running in chroot.
    (2/5) Creating temporary files...
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to parse ACL "d:group::r-x,d:group:adm:r-x,d:group:wheel:r-x,group::r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "d:group:adm:r-x,d:group:wheel:r-x,group:adm:r-x,group:wheel:r-x": Invalid argument. Ignoring
    Failed to parse ACL "group:adm:r--,group:wheel:r--": Invalid argument. Ignoring
    Failed to open file "/sys/devices/system/cpu/microcode/reload": Read-only file system
    error: command failed to execute correctly
    (3/5) Arming ConditionNeedsUpdate...
    (4/5) Updating the info directory file...
    (5/5) Performing snapper post snapshots for the following configurations...

    Generate fstab. The -U option ensures that partitions are identified by UUIDs instead of kernel assigned block names (e.g. /dev/sda5/dev/nvme0n1p7).

    [root@G5-openSUSE /]# genfstab -U /mnt >> /mnt/etc/fstab

    Edit the generated fstab. Remove the subvolume identification from the subvolume mounted to /, specifically, remove "subvolid=258,subvol=/@/.snapshots/1/snapshot". Otherwise, the booted system will use this subvolume target as the filesystem root even if we want to boot into a snapshot to initiate a rollback or to a new default subvolume if it is changed by Snapper or by some other means.

    nano /mnt/etc/fstab

    The following images show
    /etc/fstab before and after the necessary changes.
    The Siduction website home.
    Edit /etc/fstab
    Edit /etc/fstab ,accessed as /mnt/etc/fstab from the chroot environment, to remove the subvolume identifier mount options (subvolid=258,subvol=/@/.snapshots/1/snapshot) on the line where / is the target.
    Edit the files /etc/grub.d/10_linux and /etc/grub.d/20_linux_xen, accessed from within our chroot environment as /mnt/etc/grub.d/10_linux and /mnt/etc/grub.d/20_linux_xen. In both files remove

    rootflags=subvol=${rootsubvol}

    with our editor

    [root@G5-openSUSE /]# nano /mnt/etc/grub.d/10_linux

    and

    [root@G5-openSUSE /]# nano /mnt/etc/grub.d/20_linux_xen

    The following images show these files before and after making the necessary changes. Edit
    /etc/fstab ,accessed as /mnt/etc/fstab from the chroot environment, to remove the subvolume identifier mount options (subvolid=258,subvol=/@/.snapshots/1/snapshot) on the line where / is the target.
    The Siduction website home.
    Modifying /etc/grub.d/10_linux and /etc/grub.d/20_linux_xen
    Image 1: Text to replace in /etc/grub.d/10_linux shown in nano
    Image 2: Replacing with "" (no characters in field) in nano
    Image 3: The text to replace highlighted.
    Image 4: The file with the text removed.
    Image 5: Text to replace in /etc/grub.d/20_linux_xen shown in nano
    Image 6: Replacing with "" (no characters in field) in nano
    Image 7: Text to be removed highlighted.
    Image 8: The file with the text removed.


    This modification is necessary because otherwise GRUB will always look for the kernel in /@/boot instead of /@/.snapshots/1/snapshot/boot, or the appropriate location if another future snapshot becomes the default subvolume after a rollback, e.g., /@/.snapshots/34/snapshot/boot if the 34th snapshot becomes the default.

Installation Part II: Basic System Configuration
Chroot into newly Installed Arch
Basic System Configuration

Our last chroot was from our host computer to the Arch bootstrap environment. We now chroot from the Arch bootstrap environment to the new system.

[root@G5-openSUSE /]# arch-chroot /mnt

The prompt does not change. The command with the output:

[root@G5-openSUSE /]# arch-chroot /mnt
[root@G5-openSUSE /]# 

    Set the timezone by making a symbolic link to a file in /usr/share/zoneinfo/Region/City to /etc/localtime. In my case Region is America and City is New_York. Other available time zones can be determined by browsing the subdirectories of /usr/share/zoneinfo, e.g.

    [root@G5-openSUSE /]# ls -l /usr/share/zoneinfo/Europe/

    Make the symbolic link to the appropriate zone file:

    [root@G5-openSUSE /]# ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime

    Generate /etc/adjtime to synchronize the system time to the hardware clock with the hwclock command.

    [root@G5-openSUSE /]# hwclock --systohc

    The option used is different depending on whether the hardware clock is set to UTC or local time. The option used here
    --systohc is used if the hardware clock is set to UTC as is recommended when multiple OSes are installed on the computer and all are aware of this.
    Set the locale. First edit /etc/locale.gen to uncomment needed locales.

    [root@G5-openSUSE /]# nano /etc/locale.gen

    . In my case I uncommented
    en_US.UTF-8 UTF-8. Then run the locale-gen

    [root@G5-openSUSE /]# locale-gen

    The command with the output:

    [root@G5-openSUSE /]# locale-gen
    Generating locales...
      en_US.UTF-8... done
    Generation complete.

    Create the /etc/locale.conf file.

    [root@G5-openSUSE /]# touch /etc/locale.conf

    Set the LANG environment variable in /etc/locale.conf.

    [root@G5-openSUSE /]# nano /etc/locale.conf

    I need the
    en_US.UTF-8 locale so I entered the following in /etc/locale.conf. The entered string must be in the first column of the file.

    LANG=en_US.UTF-8

    Other environment variables are also available to refine the locale, but if they are not set the value set for
    LANG is used for the behavior they would define. (see man locale.7)
    Configure the virtual console -- the terminal available before the graphical environment starts and those available by pressing the Ctrl + Alt + Fn keys. Some of the options available are the keyboard layout and the font. The defaults are appropriate for me, so I didn't need to do anything. Users who want a different or optional keymap will need to add appropriate entries to the /etc/vconsole.conf file. (See man vconsole.conf.)

    nano /etc/vconsole.conf

    Set the hostname by editing the file /etc/hostname.

    [root@G5-openSUSE /]# nano /etc/hostname

    and entering the desired hostname. I chose
    G5-ARCH-B.
    Configure hosts by editing /etc/hosts

    [root@G5-openSUSE /]# nano /etc/hosts

    I entered

    127.0.0.1	localhost
    ::1		localhost
    127.0.1.1	G5-ARCH-B.localdomain	G5-ARCH-B

    Note that the hostname entered in the previous step is also entered here as the first part of a fully qualified domain name and as the hostname in the second and third elements, respectively, of the third line. This configuration is appropriate for a host with a dynamic IP address assigned by DHCP and not for a host with a fixed IP address, in which case
    127.0.1.1 is replaced with the fixed IP address and if an actual domain is configured for the host, localdomain would be replaced with the domain.
    Optionally create a new initramfs with

    mkinitcpio -p linux

    after editing
    /etc/mkinitcpio.conf. I opted not to do this during installation, but instead to optimize the initramfs configuration at /etc/mkinitcpio.conf later after installation and then generate the initramfs image.

Initialize Snapper

Next we make some adjustments so that Snapper will work in its default manner with our Arch system. Before we do that, it is beneficial to view the subvolume layout and how the subvolumes are mounted. We can view the mounted subvolumes with the mount command.

[root@G5-openSUSE /]# mount
/dev/nvme0n1p7 on / type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=258,subvol=/@/.snapshots/1/snapshot)
/dev/nvme0n1p7 on /.snapshots type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=257,subvol=/@/.snapshots)
/dev/nvme0n1p7 on /boot/grub type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=259,subvol=/@/boot/grub)
/dev/nvme0n1p7 on /opt type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=260,subvol=/@/opt)
/dev/nvme0n1p7 on /root type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=261,subvol=/@/root)
/dev/nvme0n1p7 on /tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=263,subvol=/@/tmp)
/dev/nvme0n1p7 on /srv type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=262,subvol=/@/srv)
/dev/nvme0n1p7 on /usr/local type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=264,subvol=/@/usr/local)
/dev/nvme0n1p7 on /var/cache type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=265,subvol=/@/var/cache)
/dev/nvme0n1p7 on /var/log type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=266,subvol=/@/var/log)
/dev/nvme0n1p7 on /var/spool type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=267,subvol=/@/var/spool)
/dev/nvme0n1p7 on /var/tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=268,subvol=/@/var/tmp)
/dev/nvme0n1p1 on /efi type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
/dev/sda5 on /home type ext4 (rw,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
sys on /sys type sysfs (ro,nosuid,nodev,noexec,relatime)
efivarfs on /sys/firmware/efi/efivars type efivarfs (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=12134028k,nr_inodes=3033507,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
shm on /dev/shm type tmpfs (rw,nosuid,nodev,relatime,inode64)
tmpfs on /run type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)
tmp on /tmp type tmpfs (rw,nosuid,nodev,inode64)
tmpfs on /etc/resolv.conf type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)

And the subvolumes are:

[root@G5-openSUSE /]# btrfs subvolume list /
ID 256 gen 30 top level 5 path @
ID 257 gen 45 top level 256 path @/.snapshots
ID 258 gen 104 top level 257 path @/.snapshots/1/snapshot
ID 259 gen 45 top level 256 path @/boot/grub
ID 260 gen 45 top level 256 path @/opt
ID 261 gen 84 top level 256 path @/root
ID 262 gen 54 top level 256 path @/srv
ID 263 gen 45 top level 256 path @/tmp
ID 264 gen 54 top level 256 path @/usr/local
ID 265 gen 85 top level 256 path @/var/cache
ID 266 gen 85 top level 256 path @/var/log
ID 267 gen 54 top level 256 path @/var/spool
ID 268 gen 58 top level 256 path @/var/tmp
ID 271 gen 56 top level 258 path var/lib/portables
ID 273 gen 57 top level 258 path var/lib/machines

Note that the installation target, the initial snapshot subvolid=258,subvol=/@/.snapshots/1/snapshot is mounted at /. (This same subvolume will be mounted at / in the final installed system but without the subvolume identifier for reasons discussed previously.) And the subvolid=257,subvol=/@/.snapshots subvolume is mounted at /.snapshots. Because the initial snapshot subvolid=258 is mounted at / and the snapshots subvolume (subvolid=257) is mounted at /.snapshots, this means the snapshots subvolume is under the subvolume mounted at /. This is a requirement of Snapper for its upstream default normal operation.

Snapper requires an initialization command to begin creating snapshots of a subvolume. To initialize the creation of snapshots of the subvolume mounted at / the command

snapper -c root create-config /

is executed. This creates a configuration file named /etc/snapper/configs/root for snapshotting the subvolume mounted at / by copying /etc/snapper/config-templates/default. It also creates a subvolume named .snapshots in the subvolume mounted at / and makes the directory /.snapshots to which to mount it. For the command to complete successfully, thus enabling snapshotting of the subvolume mounted at /,

    there must not be a subvolume with the same name as the subvolume Snapper wants to create inside the subvolume mounted at root.
    the directory /.snapshots must not exist
    the configuration file has to be created at the approproate location, which requires conditoins #1 and #2 to be satisfied

So we must perform the actions below to "trick" Snapper into making the configuration file, by first unmounting the /@/.snapshots subvolume, deleting its mountpoint (/.snapshots), running the Snapper configuraiton command, deleting the subvolume created by Snapper, remaking the mountpoint, and finally remounting our original /@/snapshots subvolume. For those wondering why the configuration file can't be copied manually, and the name of the configuration added to /etc/conf.d/snapper, it is possible, but Snapper commands result in errors.

    Unount the @/.snapshots subvolume from /.snapshots.

    [root@G5-openSUSE /]# umount /.snapshots

    Remove the directory that was the mountpoint of the @/.snapshots subvolume. Removing the directory does not delete the subvolume since it was unmounted in the last command.

    [root@G5-openSUSE /]# rm -r /.snapshots

    The subvolume
    @/.snapshots/1/snapshot is also unaffected since its ancestor snapshot was unmounted above. The btrfs subvolume list command shows that they are still there:

    [root@G5-openSUSE /]# btrfs subvolume list /
    ID 256 gen 28 top level 5 path @
    ID 257 gen 23 top level 256 path @/.snapshots
    ID 258 gen 100 top level 257 path @/.snapshots/1/snapshot
    ID 259 gen 12 top level 256 path @/boot/grub
    ID 260 gen 13 top level 256 path @/opt
    ID 261 gen 73 top level 256 path @/root
    ID 262 gen 40 top level 256 path @/srv
    ID 263 gen 16 top level 256 path @/tmp
    ID 264 gen 40 top level 256 path @/usr/local
    ID 265 gen 75 top level 256 path @/var/cache
    ID 266 gen 76 top level 256 path @/var/log
    ID 267 gen 71 top level 256 path @/var/spool
    ID 268 gen 44 top level 256 path @/var/tmp
    ID 272 gen 42 top level 258 path var/lib/portables
    ID 273 gen 43 top level 258 path var/lib/machines

    But using the mount command again, we see that the subvolume
    /@/.snapshots is not mounted anymore:

    [root@G5-openSUSE /]# mount
    /dev/nvme0n1p7 on / type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=258,subvol=/@/.snapshots/1/snapshot)
    /dev/nvme0n1p7 on /boot/grub type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=259,subvol=/@/boot/grub)
    /dev/nvme0n1p7 on /opt type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=260,subvol=/@/opt)
    /dev/nvme0n1p7 on /root type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=261,subvol=/@/root)
    /dev/nvme0n1p7 on /srv type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=262,subvol=/@/srv)
    /dev/nvme0n1p7 on /tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=263,subvol=/@/tmp)
    /dev/nvme0n1p7 on /usr/local type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=264,subvol=/@/usr/local)
    /dev/nvme0n1p7 on /var/cache type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=265,subvol=/@/var/cache)
    /dev/nvme0n1p7 on /var/log type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=266,subvol=/@/var/log)
    /dev/nvme0n1p7 on /var/spool type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=267,subvol=/@/var/spool)
    /dev/nvme0n1p7 on /var/tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=268,subvol=/@/var/tmp)
    /dev/nvme0n1p1 on /efi type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
    /dev/sda5 on /home type ext4 (rw,relatime)
    proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
    sys on /sys type sysfs (ro,nosuid,nodev,noexec,relatime)
    efivarfs on /sys/firmware/efi/efivars type efivarfs (rw,nosuid,nodev,noexec,relatime)
    udev on /dev type devtmpfs (rw,nosuid,relatime,size=12134028k,nr_inodes=3033507,mode=755,inode64)
    devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
    shm on /dev/shm type tmpfs (rw,nosuid,nodev,relatime,inode64)
    tmpfs on /run type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)
    tmp on /tmp type tmpfs (rw,nosuid,nodev,inode64)
    tmpfs on /etc/resolv.conf type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)

    Issue the Snapper command to initialize a configuration named root for a subvolume mounted at /.

    [root@G5-openSUSE /]# snapper --no-dbus -c root create-config /

    The
    --no-dbus option is required because we are running in a chroot environemnt to the system and not the actual system. This command, as previously mentioned, necessary for Snapper's operation, creates a default configuration named root in /etc/snapper/configs for the subvolume mounted at /. It also creates a subvolume named .snapshots in whatever subvolume is currently mounted at /. The command would not complete successfully without our previous actions of unmounting the existing @/.snapshots subvolume which had been mounted at /.snapshots and removing the .snapshots directory. The new subvolume created by Snapper (subvolid=276) is shown in the following listing.

    [root@G5-openSUSE /]# btrfs subvolume list /
    ID 256 gen 28 top level 5 path @
    ID 257 gen 23 top level 256 path @/.snapshots
    ID 258 gen 102 top level 257 path @/.snapshots/1/snapshot
    ID 259 gen 12 top level 256 path @/boot/grub
    ID 260 gen 13 top level 256 path @/opt
    ID 261 gen 73 top level 256 path @/root
    ID 262 gen 40 top level 256 path @/srv
    ID 263 gen 16 top level 256 path @/tmp
    ID 264 gen 40 top level 256 path @/usr/local
    ID 265 gen 75 top level 256 path @/var/cache
    ID 266 gen 76 top level 256 path @/var/log
    ID 267 gen 71 top level 256 path @/var/spool
    ID 268 gen 44 top level 256 path @/var/tmp
    ID 272 gen 42 top level 258 path var/lib/portables
    ID 273 gen 43 top level 258 path var/lib/machines
    ID 276 gen 102 top level 258 path .snapshots

    Delete the subvolume automatically created by snapper.

    [root@G5-openSUSE /]# btrfs subvolume delete /.snapshots

    The command with its output:

    [root@G5-openSUSE /]# btrfs subvolume delete /.snapshots
    Delete subvolume (no-commit): '//.snapshots'

    After this command the subvolume created by
    Snapper is gone. The subvolumes are now:

    [root@G5-openSUSE /]# btrfs subvolume list /
    ID 256 gen 28 top level 5 path @
    ID 257 gen 23 top level 256 path @/.snapshots
    ID 258 gen 104 top level 257 path @/.snapshots/1/snapshot
    ID 259 gen 12 top level 256 path @/boot/grub
    ID 260 gen 13 top level 256 path @/opt
    ID 261 gen 73 top level 256 path @/root
    ID 262 gen 40 top level 256 path @/srv
    ID 263 gen 16 top level 256 path @/tmp
    ID 264 gen 40 top level 256 path @/usr/local
    ID 265 gen 75 top level 256 path @/var/cache
    ID 266 gen 76 top level 256 path @/var/log
    ID 267 gen 71 top level 256 path @/var/spool
    ID 268 gen 44 top level 256 path @/var/tmp
    ID 272 gen 42 top level 258 path var/lib/portables
    ID 273 gen 43 top level 258 path var/lib/machines

    Remake the directory for mounting our snapshots subvolume.

    [root@G5-openSUSE /]# mkdir /.snapshots

    Remount our snapshots subvolume with mount -a which remounts all filesystems specified in /etc/fstab

    [root@G5-openSUSE /]# mount -a

    After this command, our original
    @/.snapshots subvolume is back (last item in list):

    [root@G5-openSUSE /]# mount
    /dev/nvme0n1p7 on / type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=258,subvol=/@/.snapshots/1/snapshot)
    /dev/nvme0n1p7 on /boot/grub type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=259,subvol=/@/boot/grub)
    /dev/nvme0n1p7 on /opt type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=260,subvol=/@/opt)
    /dev/nvme0n1p7 on /root type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=261,subvol=/@/root)
    /dev/nvme0n1p7 on /srv type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=262,subvol=/@/srv)
    /dev/nvme0n1p7 on /tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=263,subvol=/@/tmp)
    /dev/nvme0n1p7 on /usr/local type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=264,subvol=/@/usr/local)
    /dev/nvme0n1p7 on /var/cache type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=265,subvol=/@/var/cache)
    /dev/nvme0n1p7 on /var/log type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=266,subvol=/@/var/log)
    /dev/nvme0n1p7 on /var/spool type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=267,subvol=/@/var/spool)
    /dev/nvme0n1p7 on /var/tmp type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=268,subvol=/@/var/tmp)
    /dev/nvme0n1p1 on /efi type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
    /dev/sda5 on /home type ext4 (rw,relatime)
    proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
    sys on /sys type sysfs (ro,nosuid,nodev,noexec,relatime)
    efivarfs on /sys/firmware/efi/efivars type efivarfs (rw,nosuid,nodev,noexec,relatime)
    udev on /dev type devtmpfs (rw,nosuid,relatime,size=12134028k,nr_inodes=3033507,mode=755,inode64)
    devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
    shm on /dev/shm type tmpfs (rw,nosuid,nodev,relatime,inode64)
    tmpfs on /run type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)
    tmp on /tmp type tmpfs (rw,nosuid,nodev,inode64)
    tmpfs on /etc/resolv.conf type tmpfs (rw,nosuid,nodev,size=4859024k,nr_inodes=819200,mode=755,inode64)
    /dev/nvme0n1p7 on /.snapshots type btrfs (rw,relatime,compress=zstd:3,ssd,space_cache,subvolid=257,subvol=/@/.snapshots)

    Adjust permissions of /.snapshots

    [root@G5-openSUSE /]# chmod 750 /.snapshots

Install Bootloader

Next we install the firmware part of GRUB.

    Install the GRUB firmware bootloader on the ESP.

    [root@G5-openSUSE /]# grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=ARCH-B --modules="normal test efi_gop efi_uga search echo linux all_video gfxmenu gfxterm_background gfxterm_menu gfxterm loadenv configfile gzio part_gpt btrfs"

    The option
    --bootloader-id specifies what will be displayed in the UEFI interface when choosing the bootloader to start. The command with the output:

    [root@G5-openSUSE /]# grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=ARCH-B --modules="normal test efi_gop efi_uga search echo linux all_video gfxmenu gfxterm_background gfxterm_menu gfxterm loadenv configfile gzio part_gpt btrfs"
    Installing for x86_64-efi platform.
    Installation finished. No error reported.

    Update the GRUB configuration.

    [root@G5-openSUSE /]# grub-mkconfig -o /boot/grub/grub.cfg

    The command with the output:

    [root@G5-openSUSE /]# grub-mkconfig -o /boot/grub/grub.cfg
    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-linux-lts
    Found initrd image: /boot/intel-ucode.img /boot/initramfs-linux-lts.img
    Found fallback initrd image(s) in /boot:  intel-ucode.img initramfs-linux-lts-fallback.img
    Found linux image: /boot/vmlinuz-linux
    Found initrd image: /boot/intel-ucode.img /boot/initramfs-linux.img
    Found fallback initrd image(s) in /boot:  intel-ucode.img initramfs-linux-fallback.img
    Warning: os-prober will not be executed to detect other bootable partitions.
    Systems on them will not be added to the GRUB boot configuration.
    Check GRUB_DISABLE_OS_PROBER documentation entry.
    Adding boot menu entry for UEFI Firmware Settings ...
    Detecting snapshots ...
    Info: Separate boot partition not detected 
    Info: snapper detected, using config: root
    No snapshots found.
    If you think an error has occurred , please file a bug report at " https://github.com/Antynea/grub-btrfs "
    Unmount /tmp/grub-btrfs.UaWtzHmGoX .. Success
    done

    Create the root user's password.

    [root@G5-openSUSE /]# passwd

    The command with the output.

    [root@G5-openSUSE /]# passwd
    New password: 
    Retype new password: 
    passwd: password updated successfully

    Allow newly created users that are added to the wheel user group as a supplementary user group during creation to use sudo to execute commands with elevated privileges. We will do this by editing /etc/sudoers file with visudo to enable sudo for all members of the wheel group.

    [root@G5-openSUSE /]# EDITOR=nano visudo

    Edit the file to remove the "
    #" in the line

    # %wheel ALL=(ALL) ALL

    so it becomes

    %wheel ALL=(ALL) ALL

    Exit out of the chroot into installed system back to the Arch bootstrap environment.

    [root@G5-openSUSE /]# exit

    [root@G5-openSUSE /]# exit
    exit

    Exit out of the bootstrap environment back to the host system.

    [root@G5-openSUSE /]# exit

    The command with the output:

    [root@G5-openSUSE /]# exit
    exit

Installation Part III: Create Users and Enable systemd Services

Reboot the host, the Arch installed GRUB firmware bootloader should take control on next boot. Since the Plasma's display manager, SDDM, has not been enabled, the graphical environment will not be activated and we will be presented with a login prompt in a virtual console. After logging in as root we will perform final installation steps.

    Login at the prompt as root, at the password prompt supplying the root password created during the last phase of the installationat.
    Create a regular user.

    [root@G5-ARCH-B ~]# useradd -m -G wheel -s /bin/bash username

    This will create a user named
    username that is a member of the wheel supplementary user group. Optionally include -c "FUll Name" in the above command.

    [root@G5-ARCH-B ~]# useradd -m -G wheel -c "FUll Name" -s /bin/bash username

    Assign a passwd to the newly created user.

    [root@G5-ARCH-B ~]# password username

    Enter the desired password for the user each time it is prompted.
    Enable NetworkManager.

    [root@G5-ARCH-B ~]# systemctl enable NetworkManager.service

    Enable bluetooth.

    [root@G5-ARCH-B ~]# systemctl enable bluetooth.service

    Enable SDDM, the Plasma desktop environments display manager.

    [root@G5-ARCH-B ~]# systemctl enable sddm

At this point we have a fully operational Arch system installed on a Btrfs filesystem with a configured non-root user and a full desktop environment. Snapper has been initialized and snapshots are automatically created before and after package manager transactions. The only remaining tasks are

    to enable the creation of GRUB menu items for each snapshot
    to enable systemd units to optimize SSD use
    to enable systemd services for maintaining the Btrfs filesystem
    to enable the timeline type of automatic Snapper snapshots and to optimize Snapper's cleanup of snapshots by editing its configuration file
    enabling the necessary systemd unit files for the timeline Snapper snapshots and the cleanup algorithms

We will perform these tasks in the installed system after rebooting into it.
Installation Part IV: Configuring AUR and Snapper Integration with GRUB and Pacman

Now that we are in the graphical Arch system, we will perform the remaining tasks listed above. But first, lets see what our Btrfs system looks like at this stage. The image below shows a Konsole window with the outputs of the commands

btrfs subvolume list /

snapper list

and

snapper list-configs

These show, respectively, the subvolumes that exist on the system, the snapshots that exist on the system, and the Snapper configurations that exist on the system.
The Initial State of the System
Konsole window with the outputs of commands that show the subvolumes on the system, the snapshots that initially exist, and the Snapper configuration we created during the installation. Image 3 shows /etc/fstab and how the subvolumes are mounted to the filesystem hierarchy.
Click on any of the thumbnails to view a larger image.

Enable AUR Helper

Since we will need AUR helpers eventually to install other AUR packages, including snap-pac-grub, which provides pacman hooks to include snapshots in the GRUB menu, we will enable a pacman wrapper that supports the AUR -- paru.

    Search for the package on the AUR home page, and open the page for paru or paru-bin. Open the resulting page and copy the Git CLone URL.
    Create a directory for AUR packages.

    [brook@G5-ARCH-B ~]$ mkdir AUR

    Change to the new directory.

    [brook@G5-ARCH-B ~]$ cd AUR

    Clone the repository.

    [brook@G5-ARCH-B AUR]$ git clone https://aur.archlinux.org/paru.git

    Change to the created directory.

    [brook@G5-ARCH-B AUR]$ cd paru

    Use makepkg to build the package and install it.

    [brook@G5-ARCH-B paru]$ makepkg -sic

Enable Snapshots in GRUB Menu

At this point the Btrfs system and Snapper are fully operational. We can make snapshots manually, and thanks to the installation of snap-pac in the previous phases of the installation process, any pacman operations will cause snapshots to be created before and after the transactions, Snapper's pre and post snapshot types as in openSUSE. We can also manually create these types of snapshots as well as the single snapshot type, but none of these snapshots will be added to the GRUB menu allowing us to easily boot into a snapshot, to execute the snapper rollback command. But to have these snapshots automatically added to the GRUB menu as in openSUSE, we need to install snap-pac-grub from the AUR using paru.

The AUR package snap-pac-grub provides pacman hooks that will add snapshots as sub-menu items under a new GRUB menu item,Arch Linux Snapshots. Since this is an AUR package, and Arch does not include AUR helpers in its repositories, we need to install it manually using Arch supported methods.

Install snap-pac-grub with

paru -Sa snap-pac-grub

The Siduction website home.
Installing snap-pac-grub

The second image in the above set shows the pacman transaction that installs snap-pac-grub. At this point only snap-pac is active and its effect on pacman transactions is visible. In the pre-transaction hooks phase the pre transaction Snapper snapshot (numbered "2") is created. Later, in the post-transaction hooks phase, another snapshot (numbered "3") is created.

:: Proceed with installation? [Y/n]
(1/1) checking keys in keyring                                                                                   [####################################################################] 100%
(1/1) checking package integrity                                                                                 [####################################################################] 100%
(1/1) loading package files                                                                                      [####################################################################] 100%
(1/1) checking for file conflicts                                                                                [####################################################################] 100%
(1/1) checking available disk space                                                                              [####################################################################] 100%
:: Running pre-transaction hooks...
(1/1) Performing snapper pre snapshots for the following configurations...
==> root: 22
:: Processing package changes...
(1/1) installing snap-pac-grub                                                                                   [####################################################################] 100%
:: Running post-transaction hooks...
(1/3) Arming ConditionNeedsUpdate...
(2/3) Performing snapper post snapshots for the following configurations...
==> root: 23
(3/3) Generate GRUB config to let grub-btrfs detect new snapshots
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-linux-lts
Found initrd image: /boot/intel-ucode.img /boot/initramfs-linux-lts.img
Found fallback initrd image(s) in /boot:  intel-ucode.img initramfs-linux-lts-fallback.img
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/intel-ucode.img /boot/initramfs-linux.img
Found fallback initrd image(s) in /boot:  intel-ucode.img initramfs-linux-fallback.img
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
Detecting snapshots ...
Info: Separate boot partition not detected
Info: snapper detected, using config: root
Found snapshot: 2021-10-03 18:11:31 | @/.snapshots/23/snapshot | post | snap-pac-grub
Found snapshot: 2021-10-03 18:11:31 | @/.snapshots/22/snapshot | pre  | pacman --upgrade --noconfirm -- /home/brook/.cache/paru/clone/snap-pac-g
Found snapshot: 2021-10-03 17:31:06 | @/.snapshots/21/snapshot | post | qogir-icon-theme-git
Found snapshot: 2021-10-03 17:30:59 | @/.snapshots/20/snapshot | pre  | pacman --upgrade --noconfirm -- /home/brook/.cache/paru/clone/qogir-icon
Found snapshot: 2021-10-03 16:49:33 | @/.snapshots/19/snapshot | post | qogir-gtk-theme-git
Found snapshot: 2021-10-03 16:49:33 | @/.snapshots/18/snapshot | pre  | pacman --upgrade --noconfirm -- /home/brook/.cache/paru/clone/qogir-gtk-
Found snapshot: 2021-10-03 16:48:49 | @/.snapshots/17/snapshot | post | gtk-engine-murrine gtk-engines gtk2
Found snapshot: 2021-10-03 16:48:49 | @/.snapshots/16/snapshot | pre  | pacman --sync -- extra/gtk2 community/gtk-engine-murrine extra/gtk-engin
Found snapshot: 2021-10-03 16:48:00 | @/.snapshots/15/snapshot | post | kvantum-qt5
Found snapshot: 2021-10-03 16:48:00 | @/.snapshots/14/snapshot | pre  | pacman --sync -- kvantum-qt5
Found snapshot: 2021-10-03 16:47:12 | @/.snapshots/13/snapshot | post | qogir-kde-theme-git
Found snapshot: 2021-10-03 16:47:12 | @/.snapshots/12/snapshot | pre  | pacman --upgrade --noconfirm -- /home/brook/.cache/paru/clone/qogir-kde-
Found snapshot: 2021-10-03 16:00:00 | @/.snapshots/11/snapshot | post | adwaita-icon-theme at-spi2-atk at-spi2-core atk desktop-file-utils firef
Found snapshot: 2021-10-03 15:59:58 | @/.snapshots/10/snapshot | pre  | pacman -S firefox firefox-developer-edition
Found snapshot: 2021-10-03 15:55:59 | @/.snapshots/9/snapshot  | post | paru
Found snapshot: 2021-10-03 15:55:59 | @/.snapshots/8/snapshot  | pre  | /usr/bin/pacman -U /home/brook/AUR/paru/paru-1.8.2-1-x86_64.pkg.tar.zst
Found snapshot: 2021-10-03 15:35:56 | @/.snapshots/7/snapshot  | post | rust
Found snapshot: 2021-10-03 15:35:55 | @/.snapshots/6/snapshot  | pre  | /usr/bin/pacman -S --asdeps cargo
Found snapshot: 2021-10-03 15:31:27 | @/.snapshots/5/snapshot  | post | ed
Found snapshot: 2021-10-03 15:31:27 | @/.snapshots/4/snapshot  | pre  | pacman -S ed
Found snapshot: 2021-10-03 15:30:52 | @/.snapshots/3/snapshot  | post | autoconf automake binutils bison elfutils fakeroot file findutils flex g
Found snapshot: 2021-10-03 15:30:50 | @/.snapshots/2/snapshot  | pre  | pacman -S base-devel
Found 22 snapshot(s)
Unmount /tmp/grub-btrfs.CgH1nVo4U7 .. Success
done

Now that snap-pac-grub has been installed, all future transactions will have an additional hook provided by the package that will generate additional GRUB menu items for each item. This is shown in the next listing

:: Running pre-transaction hooks...
(1/1) Performing snapper pre snapshots for the following configurations...
==> root: 32
:: Processing package changes...

... truncated ...

:: Running post-transaction hooks...
( 1/10) Creating system user accounts...
Creating group mpd with gid 45.
Creating user mpd (n/a) with uid 45 and gid 45.
( 2/10) Reloading system manager configuration...
( 3/10) Creating temporary files...
( 4/10) Arming ConditionNeedsUpdate...
( 5/10) Refreshing PackageKit...
( 6/10) Updating icon theme caches...
( 7/10) Updating the info directory file...
( 8/10) Updating the desktop file MIME type cache...
( 9/10) Performing snapper post snapshots for the following configurations...
==> root: 33
(10/10) Generate GRUB config to let grub-btrfs detect new snapshots
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-linux
Found initrd image: /boot/intel-ucode.img /boot/initramfs-linux.img
Found fallback initrd image(s) in /boot: intel-ucode.img initramfs-linux-fallback.img
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
Detecting snapshots ...
Info: Separate boot partition not detected
Info: snapper detected, using config: root
Found snapshot: 2021-09-29 15:20:22 | @/.snapshots/33/snapshot | post | audiofile cantata chromaprint faad2 fluidsynth
libao libcddb libcdio lib
Found snapshot: 2021-09-29 15:20:21 | @/.snapshots/32/snapshot | pre | pacman -S mpd cantata
Found snapshot: 2021-09-28 19:31:02 | @/.snapshots/31/snapshot | post | vim vim-runtime
Found snapshot: 2021-09-28 19:31:01 | @/.snapshots/30/snapshot | pre | pacman -S vim
Found snapshot: 2021-09-28 19:14:24 | @/.snapshots/29/snapshot | post | firefox
Found snapshot: 2021-09-28 19:14:23 | @/.snapshots/28/snapshot | pre | pacman -S firefox
Found snapshot: 2021-09-28 18:48:55 | @/.snapshots/27/snapshot | post | imagemagick liblqr libraqm
Found snapshot: 2021-09-28 18:48:53 | @/.snapshots/26/snapshot | pre | pacman -S imagemagick
Found snapshot: 2021-09-27 20:41:54 | @/.snapshots/25/snapshot | post | appmenu-gtk-module libdbusmenu-glib
Found snapshot: 2021-09-27 20:41:53 | @/.snapshots/24/snapshot | pre | pacman -S appmenu-gtk-module libdbusmenu-glib
Found snapshot: 2021-09-27 20:30:26 | @/.snapshots/23/snapshot | post | sdl virtualbox virtualbox-host-modules-arch
Found snapshot: 2021-09-27 20:30:17 | @/.snapshots/22/snapshot | pre | pacman -S virtualbox
Found snapshot: 2021-09-27 15:43:05 | @/.snapshots/21/snapshot | post | coreutils discover grub-btrfs iana-etc
ktexteditor libass libgit2 libmtp
Found snapshot: 2021-09-27 15:42:54 | @/.snapshots/20/snapshot | pre | pacman -Syu
Found snapshot: 2021-09-27 15:40:11 | @/.snapshots/19/snapshot | post | reflector
Found snapshot: 2021-09-27 15:40:11 | @/.snapshots/18/snapshot | pre | pacman -S reflector
Found snapshot: 2021-09-27 15:34:48 | @/.snapshots/17/snapshot | post | tlp
Found snapshot: 2021-09-27 15:34:47 | @/.snapshots/16/snapshot | pre | pacman -S tlp
Found snapshot: 2021-09-27 15:22:05 | @/.snapshots/15/snapshot | post | hdparm tlp usbutils
Found snapshot: 2021-09-27 15:22:04 | @/.snapshots/14/snapshot | pre | pacman -S tlp
Found snapshot: 2021-09-27 15:00:26 | @/.snapshots/13/snapshot | post | kvantum-qt5
Found snapshot: 2021-09-27 15:00:26 | @/.snapshots/12/snapshot | pre | pacman -S kvantum-qt5
Found snapshot: 2021-09-27 14:50:09 | @/.snapshots/11/snapshot | post | confuse flashrom fwupd fwupd-efi gcab
gobject-introspection-runtime libf
Found snapshot: 2021-09-27 14:50:08 | @/.snapshots/10/snapshot | pre | pacman -S packagekit-qt5 fwupd
Found snapshot: 2021-09-27 14:27:49 | @/.snapshots/9/snapshot | post | plasma5-applets-active-window-control
plasma5-applets-thermal-monitor pl
Found snapshot: 2021-09-27 14:27:49 | @/.snapshots/8/snapshot | pre | pacman -S plasma5-applets-active-window-control
plasma5-applets-thermal-
Found snapshot: 2021-09-27 14:07:32 | @/.snapshots/7/snapshot | post | kleopatra kmime kpimtextedit kwalletmanager
libkleo qgpgme
Found snapshot: 2021-09-27 14:07:31 | @/.snapshots/6/snapshot | pre | pacman -S kwalletmanager kleopatra
Found snapshot: 2021-09-27 13:44:10 | @/.snapshots/5/snapshot | post | visual-studio-code-bin
Found snapshot: 2021-09-27 13:44:09 | @/.snapshots/4/snapshot | pre | pacman --upgrade --noconfirm --
/home/brook/.cache/paru/clone/visual-stu
Found snapshot: 2021-09-27 13:32:59 | @/.snapshots/3/snapshot | post | snap-pac-grub
Found snapshot: 2021-09-27 13:32:59 | @/.snapshots/2/snapshot | pre | pacman --upgrade --noconfirm --
/home/brook/.cache/paru/clone/snap-pac-g
Found 32 snapshot(s)
Unmount /tmp/grub-btrfs.ikBeKYXZ5j .. Success
done

Installation Part V: Final Configuration

We will now perform the remaining post installation configuration, including setting up automated maintenance of the filesystem and snapahots.
Enable Periodic Execution of fstrim

We will enable periodic TRIM commands using the systemd service and timer unit files fstrim.service and fstrim.timer. Enabling the timer will activate the service once a week, which in turn executes the fstrim command.

 100%  16:47:48  USER: brook HOST: G5-ARCH-B   
 ~  ❯$ sudo systemctl enable fstrim.timer

The command with the output:

 100%  16:47:48  USER: brook HOST: G5-ARCH-B   
 ~  ❯$ sudo systemctl enable fstrim.timer
Created symlink /etc/systemd/system/timers.target.wants/fstrim.timer → /usr/lib/systemd/system/fstrim.timer.

Enable Periodic Execution of btrfs scrub

According to man btrfs-scrub, the btrfs scrub command, included with btrfs-progs

    is used to scrub a mounted btrfs filesystem, which will read all data and metadata blocks from all devices and verify checksums. Automatically repair corrupted blocks if there's a correct copy available. 

As indicated in the man page, this command is not as capable as, and is not a replacement for btrfs-check, which will actually check for and repair structural errors on a -- preferrably -- unmounted Btrfs filesystem.

We will enable periodic execution of the command using the systemd unit file btrfs-scrub@.timer which starts btrfs-scrub@.service. Because these units are templates we need to enable the actual unit files by first using systemd-escape to determine the actual names of the unit files, and then enable them.

     100%  16:51:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemd-escape --template btrfs-scrub@.timer --path /dev/disk/by-uuid/eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b

    The command with the output:

     100%  16:51:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ systemd-escape --template btrfs-scrub@.timer --path /dev/disk/by-uuid/eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b
    btrfs-scrub@dev-disk-by\x2duuid-eb5f654e\x2dbfaf\x2d4d89\x2db3e4\x2d3d6dc3bd224b.timer

    Enable the timer using the unit file name as determined above:

     100%  16:52:05  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl enable btrfs-scrub@dev-disk-by\x2duuid-eb5f654e\x2dbfaf\x2d4d89\x2db3e4\x2d3d6dc3bd224b.timer

    This also creates the actual
    .service unit file. The command with the output:

     100%  16:52:05  USER: brook HOST: G5-ARCH-B  
     ~  ❯$ sudo systemctl enable btrfs-scrub@dev-disk-by\x2duuid-eb5f654e\x2dbfaf\x2d4d89\x2db3e4\x2d3d6dc3bd224b.timer
    Created symlink /etc/systemd/system/timers.target.wants/btrfs-scrub@dev-disk-by-uuid-eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b.timer → /usr/lib/systemd/system/btrfs-scrub@.timer.

    Start the timer.

     100%  17:15:50  USER: brook HOST: G5-ARCH-B   
    PCD: 3s ~  ❯$ sudo systemctl start btrfs-scrub@dev-disk-by\x2duuid-eb5f654e\x2dbfaf\x2d4d89\x2db3e4\x2d3d6dc3bd224b.timer

Checking the status of the timer, as shown below, the timer will activate the service in 1 month and 4 days, in turn activating the command

btrfs scrub start

on the Btrfs filesystem on the specified device.

 100%  17:18:38  USER: brook HOST: G5-ARCH-B   
PCD: 1m5s ~  ❯$ sudo systemctl status btrfs-scrub@dev-disk-by\x2duuid-eb5f654e\x2dbfaf\x2d4d89\x2db3e4\x2d3d6dc3bd224b.timer
● btrfs-scrub@dev-disk-by-uuid-eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b.timer - Monthly Btrfs scrub on /dev/disk/by/uuid/eb5f654e/bfaf/4d89/b3e4/3d6dc3bd224b
     Loaded: loaded (/usr/lib/systemd/system/btrfs-scrub@.timer; enabled; vendor preset: disabled)
     Active: active (waiting) since Sun 2021-10-10 17:16:03 EDT; 2min 37s ago
    Trigger: Thu 2021-11-04 04:38:00 EDT; 3 weeks 3 days left
   Triggers: ● btrfs-scrub@dev-disk-by-uuid-eb5f654e-bfaf-4d89-b3e4-3d6dc3bd224b.service

Oct 10 17:16:03 G5-ARCH-B systemd[1]: Started Monthly Btrfs scrub on /dev/disk/by/uuid/eb5f654e/bfaf/4d89/b3e4/3d6dc3bd224b.

The device paths specified when using systemd-escape were by UUID. Alternatively, the kernel device names could have been used to create the unit file names, as in

 100%  17:16:03  USER: brook HOST: G5-ARCH-B   
 ~  ❯$ systemd-escape --template btrfs-scrub@.timer --path /dev/nvme0n1p7 btrfs-scrub@dev-nvme0n1p7.timer

Edit Snapper Configuration

The Snapper configuration file that was created during the installation phase after changing root from the Arch bootstrap environment to the installed system -- /etc/snapper/configs/root -- allows modification of the number of automatic snapshots of a certain type that are created by Snapper, as well as modification of the number of snapshots that are kept by the various snapshot cleanup algorithms. The variables and values that can be specified in this file are documented in man snapper-configs.

    Under "# btrfs qgroup for space aware cleanup algorithms", change

    QGROUP=""

    to

    QGROUP="1/0"

    if Btrfs quota groups were enabled early in the installation process.
    Under "# limit for number cleanup", change

    NUMBER_LIMIT="50"

    to

    NUMBER_LIMIT="10-35"

    A range value is required for this variable if the
    SPACE_LIMIT and FREE_LIMIT variables are set in the file -- as they are by default, enabling cleanup based on the amount of storage space used by snapshots and the amount of free space specified as required.
    Under "# limit for number cleanup", change

    NUMBER_LIMIT_IMPORTANT="50"

    to

    NUMBER_LIMIT_IMPORTANT="15-25"

    Under "# limits for timeline cleanup" change

    TIMELINE_LIMIT_HOURLY="10"

    to

    TIMELINE_LIMIT_HOURLY="5"

    Change

    TIMELINE_LIMIT_DAILY="10"

    to

    TIMELINE_LIMIT_DAILY="5"

    Change

    TIMELINE_LIMIT_WEEKLY="0"

    to

    TIMELINE_LIMIT_WEEKLY="2"

    Change

    TIMELINE_LIMIT_MONTHLY="10"

    to

    TIMELINE_LIMIT_MONTHLY="3"

    Change

    TIMELINE_LIMIT_YEARLY="10"

    to

    TIMELINE_LIMIT_YEARLY="0"

The snapshot creation and cleanup algorithims and their parameters depend on a cron daemon or a systemd timer to periodically activate the cleanup. The cron daemon is not enabled by default in Arch, and we will not enable it, instead using the systemd services and timers provided by the snapper package that activate them. If you want to use the cron daemon for some other function, the systemd timers need not be activated (duplicate actions will be taken by the timers), the algorithims will work automatically with cron.

    Enable timeline snapshots timer.

     100%  17:40:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl enable snapper-timeline.timer

    The command with the output.

     100%  17:40:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl enable snapper-timeline.timer
    Created symlink /etc/systemd/system/timers.target.wants/snapper-timeline.timer → /usr/lib/systemd/system/snapper-timeline.timer.

    Start the timeline snapshots timer.

     100%  17:40:12  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl start snapper-timeline.timer

    Enable the timeline cleanup algorithm based on the "TIMELINE_LIMIT_XXXX" settings in the configuration.

     100%  17:41:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl enable snapper-cleanup.timer

    The command with the output:

     100%  17:41:01  USER: brook HOST: G5-ARCH-B   
     ~  ❯$ sudo systemctl enable snapper-cleanup.timer
    Created symlink /etc/systemd/system/timers.target.wants/snapper-cleanup.timer → /usr/lib/systemd/system/snapper-cleanup.timer.

    Start the timeline cleanup timer.

     100%  17:46:53  USER: brook HOST: G5-ARCH-B   
    PCD: 5s ~  ❯$ sudo systemctl start snapper-cleanup.timer

Whenever a pacman transaction occurs, the snapshots created by the timer, that have not been cleaned up, will be found by the pacman hooks provided by grub-btrfs and added to the GRUB menu by the hooks provided by snap-pac-btrfs.
Delete Subvolumes Created by systemd for VMs and Containers

After the initial pacstrap installation, two new subvolumes appeared, which we had not created. These are the subvolumes var/lib/machines and var/lib/portables within the initial installation subvolume @/.snapshots/1/snapshot. The subvolumes are created by systemd as part of its container and virtual machine snapshot management services which integrate with btrfs when available. Their existance is ensured by the tmpfiles mechanism with the files /usr/lib/tmpfiles.d/systemd-nspawn.conf and /usr/lib/tmpfiles.d/portables.conf.

/usr/lib/tmpfiles.d/systemd-nspawn.conf and /usr/lib/tmpfiles.d/portables.conf contain -- respectively -- the lines

Q /var/lib/machines 0700 - - -

and

Q /var/lib/portables 0700

The Q indicates that the subvolume or directory in the next field should be created if it does not exist. (See tmpfiles.d(5).)

If systemd management of container and VM snapshots is desired, the subvolumes should be created under the @ subvolume similar to @var/cache, @var/log, etc. For this installation process this is not an objective so these were not done in this case.

However, leanivng these subvolumes systemd is a problem in that it prevents the

snapper cleanup number

which according to man snapper(8) "deletes old snapshots when a certain number of snapshots is reached" from deleting snapshots between snapshot number 1 and the limit used by the command after consulting its configuration, for example at /etc/snapper/configs/root. The failure when attempting to delete snapshot 1 because it contains other subvolumes var/lib/machines and var/lib/portables prevents the snapper cleanup tool from continuing to delete the other snapshots that would be deleted up to the limit of the number of allowed snapshots. This also causes the systemd unit automating snapper number cleanup, snapper-cleanup.service started by snapper-cleanup.timer which we set up above, to also fail as shown in the following listing

󰁹 100%  15:11:31  USER: brook HOST: ARCH-16ITH6   
PCD: 24s ~  ❯$ sudo systemctl status snapper-cleanup
× snapper-cleanup.service - Daily Cleanup of Snapper Snapshots
     Loaded: loaded (/usr/lib/systemd/system/snapper-cleanup.service; static)
     Active: failed (Result: exit-code) since Thu 2023-09-14 13:06:45 EDT; 2h 6min ago
   Duration: 794ms
TriggeredBy: ● snapper-cleanup.timer
       Docs: man:snapper(8)
             man:snapper-configs(5)
    Process: 4078 ExecStart=/usr/lib/snapper/systemd-helper --cleanup (code=exited, status=1/FAILURE)
   Main PID: 4078 (code=exited, status=1/FAILURE)
        CPU: 340ms

Sep 14 13:06:44 ARCH-16ITH6 systemd[1]: Started Daily Cleanup of Snapper Snapshots.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: running cleanup for 'root'.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: running number cleanup for 'root'.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: Deleting snapshot failed.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: number cleanup for 'root' failed.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: running timeline cleanup for 'root'.
Sep 14 13:06:44 ARCH-16ITH6 systemd-helper[4078]: running empty-pre-post cleanup for 'root'.
Sep 14 13:06:45 ARCH-16ITH6 systemd[1]: snapper-cleanup.service: Main process exited, code=exited, status=1/FAILURE
Sep 14 13:06:45 ARCH-16ITH6 systemd[1]: snapper-cleanup.service: Failed with result 'exit-code'.

The failure of the snapper cleanup number service, over time will cause the filesystem to be exhausted by snapshots without manual intervention. So these subvolumes must be deleted with:

sudo btrfs subvolume delete /.snapshots/1/snapshot/var/lib/portables

and

sudo btrfs subvolume delete /.snapshots/1/snapshot/var/lib/machines

The command with the output of the first of these is:

󰁹 100%  16:44:09  USER: brook HOST: ARCH-16ITH6   
PCD: 12s ~  ❯$ sudo btrfs subvolume delete /.snapshots/1/snapshot/var/lib/portables
Delete subvolume (no-commit): '/.snapshots/1/snapshot/var/lib/portables'

Upon reboot the directories /var/lib/portables and /var/lib/machines directories will exist. They will be included in snaphshots of the subvolume mounted at / as any other directory under / which is not a mount point for one of the excluded subvolumes such as @/opt or one of the @/var subvolumes.

Unlike the

snapper cleanup number

command, the

snapper cleanup timeline

and its associated service work without problems if even leaving the above subvolumes intact as created by systemd during the pacstrap installation.
