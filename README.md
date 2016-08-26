# Create Raspberry Pi Kiosk on Raspbian Debian Wheezy

*[Chromium is not available](http://unix.stackexchange.com/questions/182061/installing-chromium-browser-on-debian-wheezy-depends-chromium-10-but-it-is) in Debian Wheezy on the armhf architcture for the Raspberry Pi 3. If you get a "package not found" error when attempting to install chromium, try another browser below."

## Chromium Browser ##

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
	@chromium --noerrdialogs --kiosk [URL] --incognito --disable-translate
	```

## Epiphany Browser ##

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

## Midori Browser ##

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

## Firefox (Iceweasel) ##

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

## Sources ##

- https://github.com/basdegroot/raspberry-pi-kiosk
- https://www.danpurdy.co.uk/web-development/raspberry-pi-kiosk-screen-tutorial/
- http://raspberrypi.stackexchange.com/questions/30093/epiphany-browser-in-full-screen-mode
- https://wiki.mhcsoftware.de/raspberrypi_als_web-kiosk-system
