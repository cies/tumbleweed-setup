# IDEs

**NOTE**: The `/usr/local/bin` and `/usr/local/share` path should be writable for the user as per instruction in the top level README.

I'm on the IntelliJ camp. Since I likely have more than one JetBrains IDE installed, I manage them with [Jetbrains Toolbox](https://www.jetbrains.com/toolbox-app), installed with:

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
