# Capra 🎥

> Capra is a system for hikers to document and revisit their past hikes.

---

## Remote Connection
There is a RealVNC Capra group for connecting to both the cameras and projectors remotely. Login details can be found in the Dropbox.

## Collector

### Services
The timelapse program is started on power up by the service: `/lib/systemd/system/capra-startup.service`

The (on/off) button is controlled by the service: `/lib/systemd/system/capra-listen-for-shutdown.service`

### LED Meanings
| LED   | Meaning |
| ------|:-------|
| 🔵 solid | Adafruit power board is connected 
| 🔴 blinking   | collector.py is PAUSED  |

## Explorer


---
**[Home](README.md)** | **[olo radio](olo.md)**
