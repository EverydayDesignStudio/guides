# Setup a Raspberry Pi 🥧

### Quick Overview
You can use a Raspberry Pi (RPi for short) basically like any other desktop computer. The first time you boot the RPi, you will need to install an OS, which is all explained pretty comprehensively on the RPi website (linked below). Most recent projects in EDS, have used [`Raspbian`](https://www.raspbian.org).
If you connect a display, a keyboard and a mouse to the RPi, it practically is a desktop computer (albeit a little slower than today’s standard).

### SD Card

**Recommended SD Card:**[`Samsung 64GB EVO Plus`](https://www.amazon.ca/Samsung-Class-Adapter-MB-MC64DA-AM/dp/B01273JZMG/ref=sr_1_1_sspa?ie=UTF8&qid=1549166261&sr=8-1-spons&keywords=samsung+evo+plus+64gb&psc=1)

Note that the operating system is run off of the micro SD card. Hence the read/write speeds are crutial. Tal and Jordan found [this comparison](https://www.jeffgeerling.com/blog/2018/raspberry-pi-microsd-card-performance-comparison-2018) helpful when deciding which card to buy. <br>

If you are using an SD card larger than 32GB, the setup has a [formatting difference.](https://www.raspberrypi.org/documentation/installation/sdxc_formatting.md)

**Cloning SD Card (On Windows)**: For various purposes, such as creating a backup image or making multiple copies of the current state of Raspberry Pi, follow [this guide](https://www.howtogeek.com/341944/how-to-clone-your-raspberry-pi-sd-card-for-foolproof-backup/) to make clones of SD Cards. **Make sure you have enough storage space when making an image of the SD card!** For example, even if there are 3Gb data in 32Gb SD Card, you still need to have more than 32Gb storage space on your machine. 

**Accessing Files on RPi from Windows**: Use [this](https://www.diskgenius.com/how-to/ext4-windows.php#Read_write_EXT4_partition_in_Windows) freeware to read/write files on RPi from Windows.

### OS Installation
[`Comprehensive tutorial from Raspberrypi.org`](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up)<br>
If you already know how to do this and just need the link: [`NOOBS`](https://www.raspberrypi.org/downloads/noobs/)


### Finding RPi IP Address
On the RPi find the IP address by opening up a terminal and typing: `ifconfig`. Under **wlan0** you will find the IP address. <br>It will look something like this <br>
```
...
wlan0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        inet 192.168.0.104 netmask 0xffffff00 broadcast 192.168.0.255
...
```
If the IP were to change, you can find it again using this method.

---

### Controlling over SSH
You can run the RPi in what is called a ‘headless’ mode. This means no screen, no keyboard and mouse or other conventional control peripherals. In order to control the RPi in this manner, you can connect to it over SSH. Most RPis the studio is using has an onboard WiFi antenna and is capable of connecting to the studio’s wireless network. 

1. In order to connect to the RPi over SSH you first need to make sure _your laptop_ and the _RPI_ is connected to the same wireless network. Because eduroam and SFUNET-SECURE require extra steps, the easiest choice is eds2g or eds5g when working in the studio. 

2. Back on your laptop open up a Linux terminal and type the following command and hit enter: <br>
`ssh pi@192.168.0.101` (where _192.168.0.101_ is the IP address of your specific RPi). If you don't know your RPi's IP, [see above](#finding-rpi-ip-address).

3. If the RPi is on and has an active internet connection, you should see the following response:
`pi@192.168.0.101's password:`

4. You can then type the password to user ‘pi’ on the RPi and confirm with Enter.

Once your in, you can navigate the RPi just like you would any Linux machine, using `ls`, `cd`, `touch`, `mkdir`, etc.

In case you are setting up a new RPi and running it in headless mode, [this website](https://howtoraspberrypi.com/how-to-raspberry-pi-headless-setup/) does a good job of explaining how you make a RPi connect to a WiFi network without ever requiring so much as a mouse or keyboard for the RPi itself. Handy if you’re working on a laptop and have no external keyboard/mouse or display available.

### Connecting over VNC
The RPi also has VNC software installed. In case you require the desktop environment of the RPi, but can't connect a display to it, you can use your own VNC software to connect to the RPi.

1. On your laptop, download [`VNC Viewer`](https://www.realvnc.com/en/connect/download/viewer/). Note that unless you will need to be streaming your desktop, you just need VNC Viewer, not VNC Server. Also note that the titles on the VNC website make it confusing as to which program you're downloading.

2. On the RPI open VNC server to _broadcast_ your RPi's desktop over the network.

3. On your laptop, VNC Viewer, enter the IP address of the RPi. This is like selecting a particular chanel on a TV. If you don't know your RPi's IP, [see above](#finding-rpi-ip-address)

4. It should connect and you will be able to see the desktop of the RPi on your laptop.

#### Connecting to Pi over Cloud (on VNC)
Follow [this](https://lifehacker.com/how-to-control-a-raspberry-pi-remotely-from-anywhere-in-1792892937) instruction to create a VNC cloud account and list devices under the account to get cloud access to Pi from anywhere. 

---

### Programs to Install
_Here's a few recommended programs consider installing_

| Program| Link / Code|
|--------|------------|
|Processing | https://pi.processing.org|
|[SQLite Browser](https://sqlitebrowser.org) | `sudo apt-get install sqlitebrowser`|
|Vim | `sudo apt-get install vim`|
|[xclip](https://coderwall.com/p/oaaqwq/pbcopy-on-ubuntu-linux)|`sudo apt-get install -y xclip`|
|xscreensaver | `sudo apt-get install xscreensaver`|

---

### Update Python version
All new projects should start with at least Python 3.7.x. If you aren't sure the version on your machine, you can run `python3 --version` to check.<br>
[Instructions to update Python version](https://installvirtual.com/install-python-3-7-on-raspberry-pi/)
`

---

### Programming the RPi
>In the past I’ve edited code on the RPi (i.e. not headless, but while connecting the RPi to a display, keyboard and mouse). This is also possible, but the code editor on the RPi does not come close to the code editor you’re using on your laptop in terms of ease of use. Besides, the RPi is a somewhat slow computer, so typing can lag behind. - Tal Amram

### IO Programming
```python
import RPi.GPIO as gpio

gpio.setmode(gpio.BCM)
gpio.setup(24, gpio.OUT)
gpio.output(24, True)
```
Save As: `foo.py` <br>
Run: `python3 ./foo.py`

---

### Helpful Commands
|What|Command|
|----|-------|
|Python version|`python --version`|
|Shutdown|`sudo shutdown -h now`|
|Reboot|`sudo reboot`|
|View python scripts running|`pgrep -af python`|
|[Kill process](https://www.linux.com/learn/intro-to-linux/2017/5/how-kill-process-command-line)|`sudo kill -9 [pid]`|


---
**[Home](README.md)** | **[Setup eduroam on RPi 3](setup-eduroam-raspberry-pi-3.md)** | **[Raspberry Pi Advanced](raspberry-advanced.md)**
