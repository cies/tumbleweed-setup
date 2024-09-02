Dell Latitude 7420
==================

This file contains instructions to install Linux that are specific to this device.


## BIOS settings

Get to the BIOS settings by repeatedly pressing F2 and/or F12 while booting the device.

1. Disable secure boot in Boot Configuration
2. Add extra boot option for USB:
    * Go to the "Boot Configuration" page
    * Click the plus button Add Boot Option
    * Click `Browse for file...` button 
    * Select the top entry that has "USB" as part of it's name
    * Then follow to EFI > boot > bootx64.efi
    * Hit Ok
    * Name it "usb1"

Now restart into bios menu an pick the "usb1" boot option.

Possibly you also need to disable fast boot in Windows:

1. Open Control Panel
2. Go to Power Options, click on “System and Security”, then, click on “Power Options”
3. Scroll down to the “Shutdown settings” section
4. Uncheck the box next to “Turn on fast startup (recommended)”.
5. Uncheck "Hibernate"


## Fingerprint login

So far I've **not** gotten this to work. Should be something like:

```bash
sudo zypper install fprintd fprintd-pam
fprintd-enroll
sudo pam-config --update fprintd
```

But using `lsusb` I cannot see the fingerprint reader device. So I give up.

Sources:
* https://en.opensuse.org/SDB:Using_fingerprint_authentication
* https://askubuntu.com/questions/1406999/i-cant-use-fingerprint-sensor-in-ubuntu-22-04
* https://fprint.freedesktop.org/supported-devices.html
* https://wiki.archlinux.org/title/Fprint


## Powermanagent

There seems to be a problem (according to reports) that power management on this device is not working well.
I've been able to confirm this.

A respected colleague came up with this "easy yet pertial fix" for OpenSUSE:

```bash
sudo zypper install thermald
sudo systemctl enable thermald.service
```

But there's more performance to be squeezed out of this machine... The story continues...

Some pointers here:

* https://github.com/intel/thermal_daemon/issues/341
* https://unix.stackexchange.com/questions/630535/thermal-throttling-on-i7-1185g7
* https://www.dell.com/community/en/conversations/latitude/latitude-542074207520-cpu-throttling-issue-on-linux/647f94baf4ccf8a8de71684e





