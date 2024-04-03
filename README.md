# My OpenSUSE Tumbleweed setup

After enjoying Kubuntu and KDE for a while, I got fed up with the problematic distribution version upgrades.
I wanted to go with a rolling release model, and somethign that does not tries to shove `snap` or `flatpak` down my throat.

I picked SUSE Tumbleweed as it is quite KDE focussed, has speady enough updates, follows a rolling release model, comes with a solid package manager and is overall pretty stable. Arch, Neon and Manjaro were also considered.

This time around I wanted to use `btrfs` with snapshots on my root and `/home` partitions. Also I like the fact that  root and `/home` can share one partition in `btrfs` with its subvolumes feature. Obviously I will use disk encryption on that partition.

This repo contains per-topic guides (by means of a `README.md` file each in a directory of its own), and some generic setup stuff (in the `README.md` file you are reading now). I've tried lots of dotfile managers, yet they never really stuck (the dont really help for tree of configs and installing OS dependencies). Hence this repo simply contains Markdown files with copy-pastable bits and dot/config files. Maybe one day I manage to get some of my KDE settings automated, who knows.

This repro is to scratch my own itch, but feel free to copy from it. If you have tips or otherwise want to reach out (maybe you found a life saving productivity hack in here) feel free to file a issue even if you just want to say hi. There's no issue template. No code of conduct. Go for it.

**NOTE**: This guide assumes a recent version of SUSE Tumbleweed.


## Download the ISO and prepare a USB drive

First [download a recent OpenSUSE Tumbleweed](https://get.opensuse.org/tumbleweed/#download) in `/tmp`.

Now find the block device of the USB drive with:

    lsblk  # repeat this with and without the USB drive inserted

Make sure the USB drive is **not** mounted.

We assume the USB drive is available as `/dev/sdb` and the image is `kubuntu-21.10-desktop-amd64.iso`.

```bash
sudo dd if=/tmp/openSUSE-Tumbleweed-DVD-*-Media.iso of=/dev/sdb bs=4M status=progress && sync
```

When this is finished the USB drive is ready to be used as installation medium.

To add an aditional partition to the USB drive to use for storage run:

```bash
sudo cfdisk /dev/sda
```

Create an additional partition that uses all empty space, set the type to `W95 FAT32 LBA` for interoperability with other OSes (set it to `EXT4` when only using it from Linux) and write the table to disk.
Then format the partition with:

```bash
sudo mkfs.vfat /dev/sdaX
```

Or (when going with EXT4):

```bash
sudo mkfs.ext4 /dev/sdaX
```

Where `X` is the number of the just created extra partition (use `lsblk` to find it).


## Running the installer

When installing (by booting from the just created USB drive) choose these options:

* Keyboard layout variant: US
* What to install: Plasma

### Partitioning

Previous version of this guide contianed bad advise, the partition/subvolume scheme "Guided setup" suggested by the installer is quite good.
Make sure to "Enable Disk Encryption" and (not quite sure about this) "Enable Logical Volumen Management (LVM)", as explained [in this article](https://en.opensuse.org/SDB:Encrypted_root_file_system).

Check the box for "Propose Separate Home LVM logincal Volume" and choose `Ext4` as a filesystem.

I had to [read this document](https://en.opensuse.org/SDB:BTRFS) in order to find out how to setup Btrfs properly (basically to understand that the suggested layour by the "Guided setup" is really good).

Leave "Propose Separate Swap" checked (if you want to be able to suspend/hibernate you can check than option as well).

This setup put the home folder on a separate partion which makes keeping your home files while reinstalling your system really easy.
Putting you home folder on a subvolume of the Btrfs partition has the advantage that the disk space is then shared between all the Btrfs partition's subvolumes and the supervolume, at the expence of making reinstalling-while-keeping-the-home-folder a bit more tricky (you need to wipe the non-home subvolumes and snapshot by hand before reinstalling).

When you encrypt multiple partitions (what is not needed when using the layout suggested by "Guided setup", as that uses LVM), using the same passphrase will ensure you only need to provide the passphrase once on boot (it's automatically tried for all encrypted partitions).

**NOTE**: Encrypting the `/boot` and `<swap>` partitions will reduce several other attack vectors, where one installs tainted kernel/initramfs (on `/boot`) or tainted memory image (which is saved to `<swap>` when hibernating) by which valuable data can be stolen.
These attacks are far more involved (expensive) and thus far less likely. Encrypting the `/boot` and `<swap>` partitions comes at a cost: you may need to provide your disk encryption passphrase several times at boot, or add some complex and brittle passphrase memoization to the boot process (which is [not trivial](https://en.opensuse.org/SDB:Encrypted_root_file_system)).


## Generic setup

These steps are fundamental to the per topic READMEs and/or my sanity.


### Install some packages

I'm blind without these, and the rest of the READMEs may require them.

```bash
sudo zypper install git tig htop iotop ripgrep xinput libinput-tools make zsh
```

**NOTE**: `libinput-tools` can do for Wayland what `xinput` did for Xorg.

### Get `/usr/local/{bin,share,src}` ready

Since I like to use that `/usr/local` for installing software as a normal user that is not managed by the package manager and should be available system-wide.

```bash
sudo chown -R $USER:$USER /usr/local/bin /usr/local/share /usr/local/src
```

## Current laptop's issue: touchpad not working after waking up

Using a *Lenovo Yoga Slim 7 14ARE05* my touchpad does not work after sleep/hibernate, this fixes it when using KDE.

It also fixes [a bug in KDE](https://bugs.kde.org/show_bug.cgi?id=435113) by which mouse settings get lost when unplugging (use `CTRL-L` to activate the lock screen).

```bash
cat > /usr/local/bin/restart-touchpad.sh << EOF
#!/bin/sh
export TOUCHPAD_NAME="\$(xinput list --name-only | grep Touchpad)"
xinput disable "\$TOUCHPAD_NAME" && xinput enable "\$TOUCHPAD_NAME"
export USB_MOUSE_NAME="\$(xinput list --name-only | grep 'USB Optical Mouse')"
xinput set-prop "\$USB_MOUSE_NAME" 287 1
xinput set-prop "\$USB_MOUSE_NAME" 298 0.2
EOF
chmod +x /usr/local/bin/restart-touchpad.sh
cat > ~/.config/ksmserver.notifyrc << EOF
[Event/unlocked]
Action=Execute
Execute=/usr/local/bin/restart-touchpad.sh
Logfile=
Sound=
TTS=
EOF
```

**NOTE**: Finding a non-KDE-specific solution is not easy as requires the use of systemd's system-wide services (only those have access to the suspend and hibernate targets), while the system-wide services cannot access Xorg since the `XAUTHORITY` env var is not set (and on modern machines it can no longer be expected to be at `~/.Xauthority`.


## That's it...

See the on topic READMEs in the sub directories.

Usually I start with `kde`, `zsh` and `firefox` to avoid annoyances getting the others done.

