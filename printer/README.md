# Printer

The *Bonjour/Dejavu* protocol is considered a security thread (it advertises your machine in the local network), so it is blocked by the firewall in OpenSUSE.

To test if we can find *Bonjour* services on the local network we need the `avahi` tools installed:

```bash
sudo zypper install -y avahi-utils
```

Using `avahi-browse -at` you can list any *Bonjour* sevices that can be found.

To temporarily open the firewall, first find the zone you are in with:

```bash
sudo firewall-cmd --list-all-zones
```

The zone you are in has an real ethernet interface specified for it (no virtual interfaces like bridges), also it has the tag `(active)` after its name.
On my system the current zone was `public`.
I opened the firewall for *Bonjour* with:

```bash
sudo firewall-cmd --zone=public --add-service=mdns
```

Running `avahi-browse -at` again then showed several sevices.

Then go to KDE's *System Settings > Printers*. It should show the printer(s) that are advertised with Bonjour on the local network.
Pick the one you want to install, click the "Select default driver" button.

Once the printer is installed, you may try to print a test page on that printer.

If printing the test page went well, close the firewall for *Bonjour* with:

```bash
sudo firewall-cmd --zone=public --remove-service=mdns
```

By installing printer drivers you may have installed `hplip` which sets up the *HP Device Manager* to autostart.
I do not like that and remove it with:

```bash
sudo zypper remove -y hplip
```
