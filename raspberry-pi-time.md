# Raspberry Pi Real Time Clock 🥧🕰

## Quick Overview
Due to the nature of the Raspberry Pi being a very cheap and economical microcontroller, it does not have all the features you may expect. One is that there is not a RTC (real time clock). This means that each time the Pi turns on it sets it's time from the global ntp (nework time protocol) servers. The system clock is not maintained from a chip on the Pi.

This is all fine and dangy when you are running a Pi which will always be connected to the internet. However, what happens if you are building a device such as Capra, which will be turning on and off outside of the range of WiFi networks for multiple days? The time won't be updated and will get out of sync with what the actual time is. The solution to this problem is to add a RTC to the Pi.

## Real Time Clock Options

* DS3231 - most accurate 
* DS1307 - most common
* PCF8523 - cheapest

[All 3 options on Adafruit](https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi/wiring-the-rtc)<br>
Another good discussion of RTCs is [here](https://makezine.com/2019/01/18/getting-started-with-real-time-clocks/).


## Reading from the RTC

**These Guides Have Extra Things That Aren't Needed**

* [Guide from The Pi Hut on RTC](https://thepihut.com/blogs/raspberry-pi-tutorials/17209332-adding-a-real-time-clock-to-your-raspberry-pi)
* [Adafruit - Adding a Real Time Clock to Raspberry Pi](https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi)
* [Pi My Life Up Guide](https://pimylifeup.com/raspberry-pi-rtc/)

**Good Guide from Adafruit**

* [Adafruit - adding a real time clock to RPi](https://learn.adafruit.com/adding-a-real-time-clock-to-raspberry-pi/set-rtc-time)

### Correct Method
#### Step 1
According to [this forum post](https://www.raspberrypi.org/forums/viewtopic.php?t=200891) all that is needed to set the Pi to read from a RTC is to do the following

Edit: **`/boot/config.txt`** <br>
`dtoverlay=i2c-rtc,ds3231`

The RTC was working after this, however the local time wasn't being updated:

```bash
pi@capra-camera2:~/capra $ sudo hwclock -r
2020-01-24 19:15:22.492136-08:00
pi@capra-camera2:~/capra $ date
Fri 24 Jan 2020 01:27:15 AM PST
pi@capra-camera2:~/capra $ 
```

```bash
pi@capra-camera2:~ $ sudo timedatectl
               Local time: Thu 2020-01-23 23:58:14 PST
           Universal time: Fri 2020-01-24 07:58:14 UTC
                 RTC time: Sat 2020-01-25 01:46:24
                Time zone: America/Vancouver (PST, -0800)
System clock synchronized: no
              NTP service: inactive
          RTC in local TZ: no
```

#### Step 2 (Jordan made this)

```python
#!/usr/bin/env python3

# Sets time on the DS3231 Real Time Clock
# This has to be done upon setup of every new camera

import os                       # For reading from bash script
import time                     # For unix timestamps
from datetime import datetime   # For printing readable time
from typing import Tuple        # For cleaner code
import busio                    # For interfacing with DS3231 Real Time Clock
import adafruit_ds3231          # pip install adafruit-circuitpython-ds3231


def main():
    print("Getting time from hardware clock")
    
    # Get the hardware clock time
    stream = os.popen('sudo hwclock -r')
    hwclock = stream.read()
    print("Hardware clock is:")
    print(hwclock)
    
    # Set date from hardware clock time
    os.popen('sudo date --set="{hwc}"'.format(hwc=hwclock))

    stream = os.popen('date')
    date = stream.read()
    print("Date is now:")
    print(date)

    print("--- End program ---")


if __name__ == "__main__":
    main()
```

---

#### Step 2 (This seems not to have worked)
[Raspberry Pi Forum Post](https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=16218&start=100) that shows how to create a service that forces the local time to be set by the RTC.

Create and edit: **`/lib/systemd/system/hwclock-start.service`**

```python
[Unit]
Description=Synchronise Hardware Clock to System Clock
DefaultDependencies=no
After=sysinit.target

[Service]
Type=oneshot
ExecStart=/sbin/hwclock -D --hctosys

[Install]
WantedBy=graphical.target multi-user.target
```

Run `systemctl daemon-reload`
Run `systemctl enable hwclock-start.service`

#### Step 2 (Trying this on Camera #2)
[Article](https://spellfoundry.com/sleepy-pi/accessing-real-time-clock-raspberry-pi/)

```python
/bin/bash -c "echo ds1374 0x68 > /sys/class/i2c-adapter/i2c-1/new_device"

# No network, set the system time from the hw clock
printf "\nSetting system time from hardware clock\n"
/sbin/hwclock -s

exit 0
```

#### Step 2 (Yet another method)
[The forum with suggested method](https://forums.gentoo.org/viewtopic-p-8303064.html?sid=13124a28e941b192720360d75c46a527)


---

### Other Tutorial State a Lot of Extra Things
* **`sudo vim /etc/modules`** <br>
Add the following: `rtc-ds3231`
* **`sudo vim /boot/config.txt`** Add the following to the bottom: 

```python
# RTC
dtoverlay=i2c-rtc,ds3231
```

* `sudo reboot now`
* `sudo apt-get -y remove fake-hwclock`
* `sudo update-rc.d -f fake-hwclock remove`
* `sudo systemctl disable fake-hwclock`
* **`sudo vim /lib/udev/hwclock-set`** <br>
Comment out the following:

```python
if [ -e /run/systemd/system ] ; then
exit 0
fi
```
* Set the hardware clock from system time: `sudo hwclock -s`
* Turn off Network Time Protocol sync: `sudo timedatectl set-ntp false`
* Read the from the hardware clock: `sudo hwclock --verbose -r`
* `sudo reboot now`

## Interference with Power On 

### Problem
Shorting `BCM3 / PIN 5` and GROUND on the RPi will turn on the Raspberry Pi. The RealTime Clock also connects to `BCM3 / PIN 5`
Whenever the RTC is grabbing the time, it checks on `BCM 3 / PIN 5`. If at any moment when the RTC is checking and the button is pressed, the `I2C` connection is screwed up by `BCM 3 / PIN 5` being shorted to `GND`.

### Solution
Set the system time based on the RTC at startup, then use `time.time()` to grab the time. This way there is no interferance with the RTC.

## Testing
**RTC not in use**, `sudo i2cdetect -y 1` will return the following:

```bash
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: 60 -- -- -- -- -- -- -- 68 -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- -- 
```

**RTC is in use**, `sudo i2cdetect -y 1` will return the following:

```bash
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: 60 -- -- -- -- -- -- -- UU -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- -- 
```

## Helpful Commands
|What|Command|
|----|-------|
|Get system time| `date`|
|Set system time with in put|`date -s '2017-05-14'`|
|Get plethora of info about system time settings|`timedatectl`|
|Turn off auto setting time with Network Time Protocol|`sudo timedatectl set-ntp false`|
|Install ntpdate|`sudo apt-get install ntpdate`|
|Set Pi's system time to internet time| ` - `|
|Checks to see if I2C device is connected|`sudo i2cdetect -y 1`|
|Read date and time of the RTC|`sudo hwclock -r`|
|Read *verbosely* date and time of the RTC|`sudo hwclock --verbose -r`|
|Set the system time from the RTC|`sudo hwclock -s`|
|Set the system time from input|`sudo hwclock --set --date='8:15'`|
|Set the system time from the input|`sudo hwclock --set --date='2017-05-14 08:15:00'`|
|Write the system time to the RTC|`sudo hwclock -w`|
|See what is in [/etc/adjtime](http://man7.org/linux/man-pages/man5/adjtime.5.html)|`cat /etc/adjtime`|
|-|-|



---
**[Home](README.md)** | **[Raspberry Pi Introduction](raspberry-pi-introduction)** | **[Raspberry Pi Advanced](raspberry-advanced.md)** | **[Setup eduroam on RPi 3](setup-eduroam-raspberry-pi-3.md)**
