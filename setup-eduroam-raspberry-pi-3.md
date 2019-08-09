# Setup eduroam on Raspberry Pi 3 🌐

>Created by:​ [Ivan Aguilar](http://ispace.iat.sfu.ca/person/ivan-aguilar/) | [iSpace Lab](http://ispace.iat.sfu.ca) <br>
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
country=your_country_abbreviation

network={ 
	ssid="eduroam"
	
	identity="your_login_info@institutional_email_domain"
	password="your_password"
	
	eap=TTLS 
	phase2="auth=MSCHAPV2"

	pairwise=CCMP
	key_mgmt=WPA-EAP 
}
```
If you are at SFU: <br>
`your_country_abbreviation` = `CA`
`@institutional_email_domain` = `@sfu.ca`

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
country=your_country_abbreviation
```
If you are at SFU: <br>
`your_country_abbreviation` = `CA`


### Additional Instructions
I based this documentation off of these sites, with some modifications to them. Might be relevant to someone who reads the documentation and if it doesn't work. In our case (SFU) IT told me to use TTLS eap setup, which is why these other documentations didn't  work. But if the person is trying to do it in another institution our country their eap might not be TTLS and it won't work, they would need to follow a different documentation, like one of these below, which use PEAP.

* [https://www.raspberrypi.org/forums/viewtopic.php?t=86253](https://www.raspberrypi.org/forums/viewtopic.php?t=86253)
* [https://www.instructables.com/id/Access-Eduroam-on-a-Raspberry-Pi-in-Cambridge/](https://www.instructables.com/id/Access-Eduroam-on-a-Raspberry-Pi-in-Cambridge/)


---
**[Home](README.md)** | **[Raspberry Pi Introduction](raspberry-pi-introduction.md)** | **[Raspberry Pi Advanced](raspberry-advanced.md)**
