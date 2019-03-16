# Raspberry Advanced

## SSH
Below are a number of limitations or things to be aware of when using SSH with your Raspberry Pi.

* Unable to use `pip install` or `pip3 install` <br>
**_Solution_**: Instead use `apt-get`
* Unable to launch a `Tkinter` or any GUI <br>
**_Solution_**: To have a GUI open remotely, you first need to set the $DISPLAY variable using the following command: `export DISPLAY=:0.0`<br>
Upon running the program again, it will launch.

### Cron Jobs
[https://linuxize.com/post/scheduling-cron-jobs-with-crontab/](https://linuxize.com/post/scheduling-cron-jobs-with-crontab/)


---
**[Home](README.md)** | **[Setup a Raspberry Pi](setup-raspberry-pi.md)** | **[Setup eduroam on RPi 3](setup-eduroam-raspberry-pi-3.md)**
