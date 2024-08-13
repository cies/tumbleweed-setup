# KDE

Some tools that are not installed by default:

```
sudo zypper install kompare ktorrent kolourpaint kturtle filelight
```

* `kompare` for a visual diff tool (IntelliJ has a good one, but sometimes you are nto in IntelliJ)
* `ktorrent` as torrents are awesome
* `kolourpaint` is the simple tool for image manipulation (think the good old MSPaint)
* `kturtle` educaitonal software akin to LOGO I wrote
* `filelight` visualizes usage of disk space


## Setup the keyboard mapping

This makes CAPS an extra Ctrl, AltGr the compose key (the way to type special characters on Linux; e.g. try "AltGr then `=` then `c` for the `€` symbol), and right-Ctrl an additional Win/Super key.

```bash
cat > ~/.config/kxkbrc << EOF 
[Layout]
Options=caps:ctrl_modifier,compose:ralt,ctrl:swap_rwin_rctl
ResetOldOptions=true
EOF
```


## Disable KWallet

I keep my passwords/phrases in BitWarden (personal) and 1Password (work), so I have no need for KWallet.

```bash
cat > ~/.config/kwalletrc << EOF
[Wallet]
Enabled=false
First Use=false
EOF
```

After this the NetworkManager will save wifi passwords unencrypted in `/etc/NetworkManager/system-connections` by default.
This makes it easy to [migrate wifi passwords to another system](../wifi-passwords/README.md).


## Changing the wallpaper

Find one that fits your screen (so it looks crisp), put it in `~/Pictures/wallpaper` and use both for:

* Display and lock screen backgrounds: right click the the image > `Set as Wallpaper` > `Both`
* Any other backgrounds for displays you may have plugged in
* And in `System Settings` > `Global Theme` > `Logion Screen (SDDM)` > pick "Breeze" > click the "Change Background" icon > `Load From File...`


## Set up the panel with the *System Load Viewer* widget

**NOTE**: This widget is not yet ported to Plasma 6.

Switch the start menu to "Application menu" (right-click on the SUSE lizzard logo in bottom-left > `Show Alternatives...`).

Add the following widget/applets/plasmoids to the panel though `Add Widgets...` > `Get New Widgets...` > `Download New Plasma Widgets`: *System Load Viewer*.

From left to right:
* Application Menu
* NOT a `Pager` (remove it, easiest by reducing the number of virtual desktops to 1)
* Task Manager
* System Load Viewer (love this)
* System Tray
* Clock
* NOT the `Peek at Desktop` button

Now adjust the height of the pannel so that the icons look best (not too much out of line), for that is ~32 pixels.

Configure the *System Load Viewer* (right-click the widget and choose `Configure System Load Viewer...`) as follows:
* Set compact bar (first!)
* Select all but cache in "Show:"
* Colors: Set colors manually and tweak the colors
* WISH: it'd also show network (aggregated in/out) and storage (aggregated i/o)

Configure the *Digital Clock* (right-click the widget and choose "Configure Digital Clock...") as follows:
* Time display: 24 hour
* Date format: Custom, namely: `ddd yyyy-MM-dd`
* Font style: `Manual` > `Choose Style...` > select "Bold"


## System Settings

### Input & Output > Mouse & Touchpad

Set both to mouse and touchpad inverted "natural" scrolling (scrolling with mouse and touchpad in the same direction as a swipe scroll) and increase the pointer speed to `0.2`.

Disable all configurations for Screen Edges.

### Colors & Themes > Global Theme

* Colors
  * Pick `Breeze Dark`
* Plasma Style
  * Pick `Breeze`
* Windows Decorations
  * Edit ("pen" icon) the `Breeze` theme
    * On the `Shadows and Outline` tab
      * Set shadow size to `Very Large`
      * Set the shadow color to `#00aaff`
      * Set outline intensity to `Medium`
* Cursors
  * Click button `Configure Launch Feedback...`
    * Set sursor feedback to `None`
* Splash Screen
  * Pick `None`
* Login Screen (SDDM)
  * You should have already picked `Breeze` and set the backgroun to the same image as the wallpaper and the lock screen.

### Apps & Windows > Window Management

* Task Switcher
  * On the `Main` tab pick `Compact` (instead of `Thumbnail Grid`)
* Desktop Effect
  * Enable `Dim Inactive` and `Dim Screen for Administrator Mode`
* Virtual Desktops
  * Keep only one

### Language & Time > Region & Language

Set `Time`, `Measurements`, `Paper Size` and `Phone Numbers` to `C` (instead of `American English`).


### Workspace > Workspace Behavior and Window Management

```bash
cat > ~/.config/kwinrc << EOF 
[Compositing]
AnimationSpeed[$d]
OpenGLIsUnsafe=false
[Effect-windowview]
BorderActivateAll=9
[Plugins]
diminactiveEnabled=true
kwin4_effect_dimscreenEnabled=true
[TabBox]
LayoutName=compact
EOF
```

To set:
* Workspace > Workspace Behavior
  * Desktop effects > Dim inactive and Dim for root privs
  * Screen Edges > remove the actions from corners and edges
* Workspace > Window Management:
  * Task Switcher > Main tab > Pick "Compact" from the top-left dropdown menu
  
### Workspace > Shortcuts > Global Shortcuts

```bash
sed -i 's/^display=Display.*/display=Display,Display\tMeta+P,Switch Display/' ~/.config/kglobalshortcutsrc
sed -i 's/^increase_volume=.*/increase_volume=Meta+=\tVolume Up,Volume Up,Increase Volume/' ~/.config/kglobalshortcutsrc
sed -i 's/^decrease_volume=.*/decrease_volume=Volume Down\tMeta+-,Volume Down,Decrease Volume/' ~/.config/kglobalshortcutsrc
sed -i 's/^mute=.*/mute=Volume Mute\tMeta+0,Volume Mute,Mute/' ~/.config/kglobalshortcutsrc
sed -i 's/^playpausemedia=.*/playpausemedia=\tMedia Play,Media Play,Play/Pause media playback/' ~/.config/kglobalshortcutsrc
sed -i 's/^view_zoom_in=.*/view_zoom_in=none,Meta+=,Zoom In/' ~/.config/kglobalshortcutsrc
sed -i 's/^view_zoom_out=.*/view_zoom_out=none,Meta+-,Zoom Out/' ~/.config/kglobalshortcutsrc
sed -i 's/^Increase Screen Brightness=.*/Increase Screen Brightness=Meta+]\tMonitor Brightness Up,Monitor Brightness Up,Increase Screen Brightness/' ~/.config/kglobalshortcutsrc
sed -i 's/^Decrease Screen Brightness=.*/Decrease Screen Brightness=Monitor Brightness Down\tMeta+[,Monitor Brightness Down,Decrease Screen Brightness/' ~/.config/kglobalshortcutsrc
```

To set:

* Workspace > Shortcuts > Global Shortcuts > (reassign if needed):
  * Audio Volume > Alternates for: Mute/Decrease/Increase Volume to `META`-`0`/`-`/`=` (meta being the WIN key)
  * Media Controller > Play/Pause media playback to `META`-`p`
  * Power Management > Alternates for: Increase/Decrease Screen Brightness to `META`-`[`/`]`

### Workspace > Startup and Shutdown > Background Services

Disable background services that I consider unneeded:
* SMB Watcher
* Wacom Tablet
* Thunderbolt device monitor
* Write Daemon

### Workspace > Startup and Shutdown

  * Login Screen (SDDM) > Theme > Breeze (top one will do) > Background (click the button)
  * Splash Screen > None
  
### Personalization > Regional Settings

```bash
cat > ~/.config/plasma-localerc << EOF
[Formats]
LANG=C
LC_MEASUREMENT=C
LC_MONETARY=C
LC_NUMERIC=en_US.UTF-8
LC_TIME=C
EOF
```

To be slightly less US-centric, and prefer 24h time format over the AM/PM-style. Though keep the thousands separators in for big numbers.

### Death to the bouncy busy pointer

By default KDE shows a horrible animation besides the pointer when you start a program.

```bash
cat > ~/.config/klaunchrc << EOF 
[BusyCursorSettings]
Bouncing=false
[FeedbackStyle]
BusyCursor=false
EOF
```

To set:
* Personalization > Applications > Launch Feedback > No feedback


## Konsole

After a long adventure with `tmux` I now simply use Konsole's tabs. No splitting, no "remote sessions", but I find little need for that nowadays.

```bash
cat > ~/.local/share/konsole/Default.profile << EOF
[Appearance]
ColorScheme=Breeze
Font=Source Code Pro,14,-1,5,50,0,0,0,0,0
[General]
Name=Default
Parent=FALLBACK/
[Scrolling]
HistoryMode=2
EOF
cat > ~/.config/konsolerc << EOF 
[Desktop Entry]
DefaultProfile=Default.profile
[General]
ConfigVersion=1
[KonsoleWindow]
ShowWindowTitleOnTitleBar=true
UseSingleInstance=true
[MainWindow]
MenuBar=Disabled
[TabBar]
CloseTabButton=None
EOF
```

To set

* Settings > Edit default profile
  * 70 rows
  * Apperance: Breeze
  * Font: Source Code Pro 14px (install instructions in `../fonts/README.md`)
  * Scrolling -> Unlimited scroll
* Settings > Configure Konsole
  * General: DONT Show menubar by default (Ctrl-M to toggle)
  * TabBar:
    * Position: Below
    * Close tab button: None (use `CTRL-D` or `exit`)
    * Use user defined stylesheet: `konsole.css` from the same dir as this README
    * New tab behavior: After current tab

Some cool shortcuts I keep forgetting:

* `CTRL-SHIFT-t` for new tab
* `SHIFT-left/right` for tab switch, or `CTRL-SHIFT-left/right` for tab ordering
* `SHIFT-PgUp/Down` for half page scrolling
* `CTRL-SHIFT-f` for search in buffer, `ESC` to get out, `(SHIFT)-enter` for prev/next match

Splitting does not work well. It should split a tab in panes, but instead each split has its own tabs.


## Dolphin

Dolphin prefs:
* View Modes > Details > Folder: NOT Expandable & 32 pixels size for both

Control:
* Set tree view "details"
* Show hidden

Places toolbar:
* Desktop
* Home
* Documents
* Root (`/_` icon)
* Tmp (Green icon)
* Trash

Remove the "Search For" submenu in the places sidebar


## Display Configuration

Everytime I attach a monitor you I have to place it over my laptop screen in the *Display Configuration*. It will remember them.


## Window shortcuts

These dont seem to stick to the windows, any tips welcome here (how to get a shortcut for the first Firefox, Konsole, IntelliJ window)


# Some nice shortcuts (to remember)

* Toggle windows maximize: `META-PgUp`
* Minimize: `META-PgDn`
* Quick tile window L/R/T/B: `META-<arrows>`
* Switch to window to the L/R or A/B: `META-ALT-<arrows>`
* Toggle present windows: `META-W` and you can type to find a window in this mode
* Open print screen dialog: `PrScr` and you can add annotations with this tool as well


## Notes

* Keep resizing those tiny default dialogs and windows, KDE is quite good it remembering those.




