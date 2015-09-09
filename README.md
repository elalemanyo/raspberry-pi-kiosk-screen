# Create Raspberry Pi Kiosk on Raspbian Debian Wheezy

## With Chromium

1. Install Raspbian Debian Wheezy

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan. (Advanced Options)
	- Change your boot to desktop environment.

3. Now start the terminal and update your system:
`sudo apt-get update && sudo apt-get upgrade`

4. Install chromium (browser), x11 server utils and unclutter (hide the cursor from the screen)
```
sudo apt-get install chromium x11-xserver-utils unclutter
```

5. When the GUI starts up chromium needs to boot in kiosk-mode and open the webpage. Because of that we edit the autostart file:
```
sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
```

The autostart files needs to look like this:
```
@lxpanel --profile LXDE
@pcmanfm --desktop --profile LXDE
@xset s off
@xset -dpms
@xset s noblank
@sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium Default/Preferences
@chromium --noerrdialogs --kiosk http://www.page-to.display --incognito --disable-translate
```
