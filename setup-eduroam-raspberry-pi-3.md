# Setup eduroam on Raspberry Pi 3 🌐

>Created by:​ Ivan Aguilar <br>
>Date:​ January 16, 2019 <br>
>Device:​ Raspberry Pi 3 <br>
>Operating System:​ Raspbian

>**All of these instructions are to be done on the Raspberry Pi device**

---

1. Open folder: `/etc/network`
2. Right click file: `interfaces`
3. Click `Open With...` -> `Custom Command Line` Tab -> Command line to execute: `sudo leafpad` -> OK
4. Replace existing text with:

```
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

auto eth0 allow-hotplug eth0 iface eth0 inet manual
allow-hotplug wlan0
iface wlan0 inet manual

wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf

iface default inet dhcp
```

5. Save file
6. Open folder: `/etc/wpa_supplicant`
7. Right click file: `wpa_supplicant`
8. Click `Open With...` -> `Custom Command Line` Tab -> Command line to execute: `sudo leafpad` -> OK
9. Replace existing text with:

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev update_config=1
country=CA

network={ 
	ssid="eduroam"
	
	identity="your_login_info@sfu.ca" 
	password="your_password"
	
	eap=TTLS 
	phase2="auth=MSCHAPV2"

	pairwise=CCMP
	key_mgmt=WPA-EAP 
}
" (without these quotes. Leave quotes around identity, password, ssid, and phase2)
```
_leave quotes around the `identity`, `password`, `ssid`, and `phase2`_

10. Save file
11. Reboot Raspberry Pi and try out a web browser 
12. Setup complete

---

**Note:** doing this will disable your WiFi connection to other networks and the WiFi icon on the top bar of the operating system. If you want to manually connect to other WiFi, add them to the `wpa_supplicant` file.

Below are default text for the "interfaces" and the `wpa_supplicant` files in case you want to go back.


***`Interfaces` (/etc/network) (default):***

```
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback

iface eth0 inet manual

allow-hotplug wlan0
iface wlan0 inet manual
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

allow-hotplug wlan1
iface wlan1 inet manual
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

**`wpa_supplicant` (/etc/wpa_supplicant) (default):**

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=CA
```

---
**[Home](README.md)** | **[Setup a Raspberry Pi](setup-raspberry-pi.md)**