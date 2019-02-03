# Setting up a Raspberry Pi 🥧

### Quick Overview
You can use a Raspberry Pi (RPi for short) basically like any other desktop computer. The first time you boot the RPi, you will need to install an OS, which is all explained pretty comprehensively on the RPi website (linked below). Most recent projects in EDS, have used [`Raspbian`](https://www.raspbian.org).
If you connect a display, a keyboard and a mouse to the RPi, it practically is a desktop computer (albeit a little slower than today’s standard).

### SD Card
Note that the operating system is run off of the micro SD card. Hence the read/write speeds are crutial. Tal and Jordan found [this comparison](https://www.jeffgeerling.com/blog/2018/raspberry-pi-microsd-card-performance-comparison-2018) helpful when deciding which card to buy. <br>
Recommended SD Card: [`Samsung 64GB EVO Plus`](https://www.amazon.ca/Samsung-Class-Adapter-MB-MC64DA-AM/dp/B01273JZMG/ref=sr_1_1_sspa?ie=UTF8&qid=1549166261&sr=8-1-spons&keywords=samsung+evo+plus+64gb&psc=1)

### OS Installation
[`Comprehensive tutorial from Raspberrypi.org`](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up)<br>
[`NOOBS`](https://www.raspberrypi.org/downloads/noobs/)

---

### Controlling over SSH
You can run the RPi in what is called a ‘headless’ mode. This means no screen, no keyboard and mouse or other conventional control peripherals. In order to control the RPi in this manner, you can connect to it over SSH. Most RPis the studio is using has an onboard WiFi antenna and is capable of connecting to the studio’s wireless network. 

0. In order to connect to the RPi over SSH you first need to make sure _your laptop_ and the _RPI_ is connected to the same wireless network: `eds5g` password: `DesignStudio2016`.

1. On the RPi find the IP address by opening up a terminal and typing: `ping raspberrypi` <br>
If the IP were ever to change, you can always find it using this method.

2. Back on your laptop open up a Linux terminal and type the following command and hit enter: <br>
`ssh pi@192.168.0.101` (where _192.168.0.101_ is the IP address of your specific RPi).

3. If the RPi is on and has an active internet connection, you should see the following response:
`pi@192.168.0.101's password:`

4. You can then type the password to user ‘pi’ on the RPi and confirm with Enter.

In case you are setting up a new RPi and running it in headless mode, [this website](https://howtoraspberrypi.com/how-to-raspberry-pi-headless-setup/) does a good job of explaining how you make a RPi connect to a WiFi network without ever requiring so much as a mouse or keyboard for the RPi itself. Handy if you’re working on a laptop and have no external keyboard/mouse or display available.

### Programs to Install
| Program| Link / Code|
|--------|------------|
|Processing | https://pi.processing.org|
|Vim | `sudo apt-get install vim`|

---
**[Home](README.md)**
