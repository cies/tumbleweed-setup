Lenovo Yoga 7 (14ARE05)
=======================

This file contains instructions to install Linux that are specific to this device.

## Touchpad not working after waking up

It seems the touchpad did not work after waking up from sleep/hibernate, this fixes it when using KDE.

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
