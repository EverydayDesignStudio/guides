# Raspian Locations and Commands 🥧

> A list of file path locations and commands for Raspian (Raspberry Pi)


## Locations of Things
| What | Location |
|--------|------------|
|WiFi Passwords| `/etc/wpa_supplicant/wpa_supplicant.conf`|
|Startup Services|`/lib/systemd/system/sample.service`|


## Helpful Commands
|What|Command|
|----|-------|
|Python version|`python --version`|
|Shutdown|`sudo shutdown -h now`|
|Reboot|`sudo reboot`|
|View python scripts running|`pgrep -af python`|
|[Kill process](https://www.linux.com/learn/intro-to-linux/2017/5/how-kill-process-command-line)|`sudo kill -9 [pid]`|
|[Set WiFi Network Passwords](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)|`sudo vim /etc/wpa_supplicant/wpa_supplicant.conf`|


---
**[Home](README.md)** | **[Setup eduroam on RPi 3](setup-eduroam-raspberry-pi-3.md)** | **[Raspberry Pi Advanced](raspberry-advanced.md)**
