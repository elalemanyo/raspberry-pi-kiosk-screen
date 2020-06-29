# Create Raspberry Pi Kiosk on Raspbian Debian Wheezy

## Table of Contents
1. [Raspbian Wheezy](#raspbian-wheezy)  
 1.1. [Chromium Browser](#chromium-browser)  
 1.2. [Epiphany Browser](#epiphany-browser)  
 1.3. [Midori Browser](#midori-browser)  
 1.4. [Firefox (Iceweasel)](#firefox-iceweasel)
2. [Raspbian Jessie](#raspbian-jessie)  
 2.1. [Chromium Browser](#chromium-browser)  
3. [Connecting to Wi-Fi on boot](#connecting-to-wi-fi-on-boot)
4. [Splashscreen](#splashscreen)
5. [References](#references)
6. [Sources](#sources)

## Raspbian Wheezy

*[Chromium is not available](http://unix.stackexchange.com/questions/182061/installing-chromium-browser-on-debian-wheezy-depends-chromium-10-but-it-is) in Debian Wheezy on the armhf architcture for the Raspberry Pi 3. If you get a "package not found" error when attempting to install chromium, try another browser below."

### Chromium Browser ###

1. Install Raspbian Debian Wheezy

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan. (Advanced Options)
	- Change your boot to desktop environment.

3. Now start the terminal and update your system:
	```
	sudo apt-get update
	sudo apt-get upgrade
	```

4. Install chromium (browser), x11 server utils and unclutter (hide the cursor from the screen)
	```
	sudo apt-get install chromium x11-xserver-utils unclutter
	```

5. When the GUI starts up chromium needs to boot in kiosk-mode and open the webpage. Because of that we edit the autostart file:
	```
	nano ~/.config/lxsession/LXDE-pi/autostart
	```

	The autostart files needs to look like this:
	```
	@lxpanel --profile LXDE
	@pcmanfm --desktop --profile LXDE
	@xset s off
	@xset -dpms
	@xset s noblank
	@sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium Default/Preferences
	@chromium-desktop --noerrdialogs --kiosk [URL] --incognito --disable-translate
	```

### Epiphany Browser ###

1. Install Raspbian Debian Wheezy

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan. (Advanced Options)
	- Change your boot to desktop environment.

3. Now start the terminal and update your system:
	```
	sudo apt-get update
	sudo apt-get upgrade
	```
4. Install Epiphany (browser), x11 server utils, xautomation and unclutter (hide the cursor from the screen)
	```
	sudo apt-get install epiphany-browser x11-xserver-utils xautomation unclutter
	```

5. Save this shell script: /home/pi/fullscreen.sh
	```
	nano /home/pi/fullscreen.sh
	```
	Fullscreen shell script should look like this: (example replace [URL] http://github.com/elalemanyo/raspberry-pi-kiosk-screen)

	```
	sudo -u pi epiphany-browser -a -i --profile ~/.config [URL] --display=:0 &
	sleep 15s;
	xte "key F11" -x:0
	```

	Change file mode:
	```
	chmod 755 /home/pi/fullscreen.sh
	```

6. Edit the autostart file to run the script:
	```
	nano ~/.config/lxsession/LXDE-pi/autostart
	```

	The autostart files needs to look like this:
	```
	@xset s off
	@xset -dpms
	@xset s noblank
	@/home/pi/fullscreen.sh
	```

7. Reload the page (if needed) every hour:
	```
	crontab -e
	```

	Add this:
	```
	0 */1 * * * xte -x :0 "key F5"
	```

### Midori Browser ###

1. Install Raspbian Debian Wheezy

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan. (Advanced Options)

3. Now start the terminal and update your system:
	```
	sudo apt-get update
	sudo apt-get upgrade
	```

4. Install Midori (browser), x11 server utils, matchbox and unclutter (hide the cursor from the screen)
	```
	sudo apt-get install midori x11-xserver-utils matchbox unclutter
	```

5. Save this shell script: /home/pi/fullscreen.sh
	```
	nano /home/pi/fullscreen.sh
	```
	Fullscreen shell script should look like this:

	```
	unclutter &
	matchbox-window-manager &
	midori -e Fullscreen -a [URL]
	```

	Change file mode:
	```
	chmod 755 /home/pi/fullscreen.sh
	```

6. Edit the autostart file to run the script:
	```
	nano ~/.config/lxsession/LXDE-pi/autostart
	```

	The autostart files needs to look like this:
	```
	@xset s off
	@xset -dpms
	@xset s noblank
	@/home/pi/fullscreen.sh
	```

7. Auto StartX (Run LXDE):
	```
	sudo nano /etc/rc.local
	```

	Scroll to the bottom and add the following above exit 0:
	```
	su -l pi -c startx
	```

### Firefox (Iceweasel) ###

1. Install Raspbian Debian Wheezy

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan (Advanced Options)
	- Memory Split (Advanced Options)

3. Now start the terminal and update your system:
	```
	sudo apt-get update
	sudo apt-get upgrade
	```

4. Install Firefox (Iceweasel), Gnash, x11 server utils, matchbox, xautomation and unclutter (hide the cursor from the screen)
	```
	sudo apt-get install iceweasel gnash gnash-common browser-plugin-gnash x11-xserver-utils matchbox xautomation unclutter
	```

5. Save this shell script: /home/pi/fullscreen.sh
	```
	nano /home/pi/fullscreen.sh
	```
	Fullscreen shell script should look like this:

	```
	unclutter &
	matchbox-window-manager &
	iceweasel http://www.domradio.de/foyer/wrapper --display=:0 &
	sleep 15s;
	xte "key F11" -x:0
	```

	Change file mode:
	```
	chmod 755 /home/pi/fullscreen.sh
	```

6. Edit the autostart file to run the script:
	```
	sudo nano ~/.config/lxsession/LXDE-pi/autostart
	```

	The autostart files needs to look like this:
	```
	@xset s off
	@xset -dpms
	@xset s noblank
	@/home/pi/fullscreen.sh
	```

7. Auto StartX (Run LXDE):
	```
	sudo nano /etc/rc.local
	```

	Scroll to the bottom and add the following above exit 0:
	```
	su -l pi -c startx
	```

8. Reload the page (if needed) every hour:
	```
	crontab -e
	```

	Add this:
	```
	0 */1 * * * xte -x :0 "key F5"
	```

## Raspbian Jessie

### Chromium Browser ###

1. Install Raspbian Debian Jessie

2. `sudo raspi-config`
	- Update the Raspi config tool (Advanced Options)
	- Enable SSH
	- Disable overscan. (Advanced Options)

3. Now start the terminal and update your system:
	```
	sudo apt-get update
	sudo apt-get upgrade
	```

4. Install unclutter (hide the cursor from the screen)
	```
	sudo apt-get install unclutter
	```

5. When the GUI starts up chromium needs to boot in kiosk-mode and open the webpage. Because of that we edit the autostart file:
	```
	nano ~/.config/lxsession/LXDE-pi/autostart
	```
	or
	```
	sudo nano /etc/xdg/lxsession/LXDE-pi/autostarts
	```

	The autostart files needs to look like this:
	```
	@lxpanel --profile LXDE-pi
	@pcmanfm --desktop --profile LXDE-pi
	@xset s off
	@xset -dpms
	@xset s noblank
	@sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium-browser Default/Preferences
	@chromium-browser --noerrdialogs --kiosk [URL] --incognito --disable-translate
	```



## Connecting to Wi-Fi on boot ##

You may wish to have your Kiosk device connect automatically both to your home wifi (where you test it) as well as the wireless network where you deploy it.

In `/etc/network/interfaces` Add a line that says to cause wireless to connect automatically auto wlan0.


Then edit `/etc/wpa_supplicant/wpa_supplicant.conf` to define one or more wireless networks, changing the SSIDs and passwords to match your networks. The `id_str` values just need to be unique.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
	ssid="SCHOOLS NETWORK NAME"
	psk="SCHOOLS PASSWORD"
	id_str="school"
}

network={
	ssid="HOME NETWORK NAME"
	psk="HOME PASSWORD"
	id_str="home"
}
```

## Splashscreen ##

1. Install Frame Buffer Imageviewer (FBI):

	```
	sudo apt-get install fbi
	```
2. Upload splash.png:

	```
	scp splash.png pi@[IP raspberry pi]:splash.png
	```

3. Move splash image to /etc/:

	```
	sudo mv splash.png /etc/
	```

4. Add script:

	```
	sudo nano /etc/asplashscreen
	```

	```
	do_start () {
		/usr/bin/fbi -T 1 -noverbose -a /etc/splash.png
		exit 0
	}
	case "$1" in
		start|"")
	do_start
	;;
	restart|reload|force-reload)
	echo "Error: argument '$1' not supported" >&2
	exit 3
	;;
	stop)
	# No-op
	;;
	status)
	exit 0
	;;
	*)
	echo "Usage: asplashscreen [start|stop]" >&2
	exit 3
	;;
	esac
	:
	```

5. Run:

	```
	sudo mv asplashscreen /etc/init.d/asplashscreen
	```

6. Run:

	```
	sudo chmod a+x /etc/init.d/asplashscreen
	sudo insserv /etc/init.d/asplashscreen
	```

## References: ##

 * [How to get Wi-Fi to connect on boot?](http://raspberrypi.stackexchange.com/questions/13558/how-to-get-wi-fi-to-connect-on-boot)
 * [How to setup multiple wifi networks?](http://raspberrypi.stackexchange.com/questions/11631/how-to-setup-multiple-wifi-networks)


## Sources ##

- https://github.com/basdegroot/raspberry-pi-kiosk
- https://www.danpurdy.co.uk/web-development/raspberry-pi-kiosk-screen-tutorial/
- http://raspberrypi.stackexchange.com/questions/30093/epiphany-browser-in-full-screen-mode
- https://wiki.mhcsoftware.de/raspberrypi_als_web-kiosk-system
