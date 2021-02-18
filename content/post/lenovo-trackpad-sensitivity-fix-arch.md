+++
author = "Jeses"
title = "Lenovo TrackPad & TrackPoint Fix"
date = "2021-02-17"
description = "A fix for slow trackpad / trackpoing sensitivity using i3 window manager."
tags = [
    "arch",
    "lenovo",
    "i3wm",
]
categories = [
    "Linux",
]
+++
# Lenovo Arch TrackPoint & TrackPad Sensitivity Fix

For those not following me on Instagram or Twitter I've recently made the switch to EndeavourOS (a flavour of Arch Linux) and have been using it as my daily driver. I'll do a more detailed write up as to why I chose EOS > vanilla Arch in the next few weeks. It's been a slow but enlightening experience getting everything setup. Today, I decided to tackle an annoying problem, a slow AF TrackPoint and TrackPad.

My first non-persistent solution was to use xinput to set the increased acceleration ... *every* login.

```sh
xinput list
```

To change the sensitivity (acceleration) use the commands below. I personally prefer the use of the device id vs the name of the device; but, if you prefer you can swap the id integer for the string name encased in quotes.

*if I were using removable hardware than I'd probably use the name vs id.*

```sh
xinput --set-prop 13 "libinput Accel Speed" 0.7
xinput --set-prop 12 "libinput Accel Speed" 0.75
```

### The Persistent Auto Fix
 
I've placed the above commands into an executable shell script which resides in ```/usr/bin/```.

#### Creating The Shell Script 
```sh
sudo nvim /usr/bin/set-sensitivity.sh
```
```sh
#!/bin/bash
xinput --set-prop 13 "libinput Accel Speed" 0.7
xinput --set-prop 12 "libinput Accel Speed" 0.75
```
```sh
chmod +x /usr/bin/set-sensitivity.sh
```
I then appended my local i3 configuration file to run the script when the environment loads

```sh
echo 'exec --no-startup-id sensitivity-fix' >> ~/.config/i3/config
