# JetBrains IDEs

The `/opt` path should be writable, as we use that location to install JetBrains products.

```bash
sudo mkdir /opt/jetbrains
sudo chmod $USER:$USER /opt/jetbrains
```

I'm on the IntelliJ camp. Since I likely have more than one JetBrains IDE installed, I manage them with [Jetbrains Toolbox](https://www.jetbrains.com/toolbox-app). Follow the link to download the `tar.gz` package.

Currently the JetBrains Toolbox does not work on Wayland. So the first bit should be performed from an X11 session.

```bash
cp jetbrains-toolbox-*.tar.gz /opt/jetbrains
cd /opt/jetbrains
tar xvzf jetbrains-toolbox-*.tar.gz
rm jetbrains-toolbox-*.tar.gz```bash
sudo zypper install libgthread-2_0-0
./jetbrains-toolbox-*/jetbrains-toolbox
```

In JetBrains Toolbox set the install folder to `/opt/jetbrains` (click gear icon -> Settings -> Tools -> Tools installation location).

Also disable autostart on startup for the Toolbox in (click gear icon -> Settings -> Appearance and Behavior -> Launch Toolbox app at system startup).

Now you may install the JetBrains products you want using the Toolbox app.


<!--

```bash
cd /usr/local/bin
wget -cO jetbrains-toolbox.tar.gz "https://data.services.jetbrains.com/products/download?platform=linux&code=TBA"
tar -xzf jetbrains-toolbox.tar.gz
DIR=$(find . -maxdepth 1 -type d -name jetbrains-toolbox-\* -print | head -n1)
mv $DIR/jetbrains-toolbox .
rmdir $DIR
./jetbrains-toolbox
```

When it runs click the bolt-shaped icon and choose `Settings` to:

* Disable "Launch Toolbox App at system startup", to conserve valuable resources
* Improve privacy by disabling "Send anonymous usage statistics to JetBrains"
* Set the "Tools install location" (I rather not have 3rd party binaries in my home dir) to: `/usr/local/share/JetBrains/Toolbox` and click `Apply`
* Set the "Shell scrips location" to: `/usr/local/bin` and click `Apply`

And install you IDE of choice. I do *IntellJ IDEA Community Edition*.

When done downloading close the app by clicking the bolt-shaped icon and choosing `Quit` or `CTRL-Q`.

-->

### Alternative: Download and install from `tar.gz` packages

[Download IntelliJ IDEA Commmunity Edition](https://www.jetbrains.com/idea/download/?section=linux), it is the second download on the page.

Then install it (and an OS dependency for it) with:

```bash
tar xvzf ideaIC-20*.tar.gz -C /usr/local/bin
```

Right-click on the "start menu" to add an item for it with `Edit Applications...` (point it to `/usr/local/bin/idea-IC-?????????/bin/idea`).


### Wayland native more for JetBrains products

And to make it use Wayland add `-Dawt.toolkit.name=WLToolkit` to `Help` > `Edit Custom VM Options...`
