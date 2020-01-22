# Raspberry Pi Real Time Clock 🥧🕰

## Quick Overview
Due to the nature of the Raspberry Pi being a very cheap and economical microcontroller, it does not have all the features you may expect. One is that there is not a RTC (real time clock). This means that each time the Pi turns on it syncs the system clock via the internet. 

However, what happens if you are building a device such as Capra? Well, what happens is that it gets out of sync. The solution is that you need a real time

## Real Time Clock Options

**[DS3231 on Adafruit]()**

## Reading from the RTC

### Reading from RTC Clock on Startup
* **`sudo vim /boot/config.txt`** <br>
Add the following to the bottom: 

```python
# RTC
dtoverlay=i2c-rtc,ds3231
```
* **`sudo vim /lib/udev/hwclock-set`** <br>
Comment out the following:

```python
if [ -e /run/systemd/system ] ; then
exit 0
fi
```
<!--* `sudo reboot now` <br>-->
* `sudo apt-get -y remove fake-hwclock` <br>
* `sudo update-rc.d -f fake-hwclock remove` <br>
* Read the from the hardware clock: `sudo hwclock --verbose -r` <br>
* Set the hardware clock from system time: `sudo hwclock -s`
* Turn off Network Time Protocol sync: `sudo timedatectl set-ntp false`
* `sudo reboot now` <br>

### Interference with Power On 

#### Problem
Shorting `BCM3 / PIN 5` and GROUND on the RPi will turn on the Raspberry Pi. The RealTime Clock also connects to `BCM3 / PIN 5`
Whenever the RTC is grabbing the time, it checks on `BCM 3 / PIN 5`. If at any moment when the RTC is checking and the button is pressed, the `I2C` connection is screwed up by `BCM 3 / PIN 5` being shorted to `GND`.

#### Solution
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
|Install nptdate|`sudo apt-get install ntpdate`|
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
