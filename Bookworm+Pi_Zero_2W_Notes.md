<h1>WiFi camera-streamer and misc issues with the Pi Zero W and Zero 2W running  raspbian bookworm.</h1>
Initial WiFi and SSH connection
Image the SD card using the Raspberry Pi Imager to create an image with SSH enabled and a username/pass for your wifi network.
---
In order to get your pi camera to stream reliably and un-cropped:
set gpu ram to at least 128 mb. *** works with 1640 x 1232, this is the minimum resolution with no cropping on the picam v2.1.  You can probably set gpu_mem to 192.  monitor free memory in the terminal with "free -m"  or in mainsail on the machine tab under system loads.

to set the gpu memory on boot up edit the config text file in /boot/firmware
sudo nano /boot/firmware/config.txt
```

[all]
gpu_mem=128
```

---
The crowsnest config looks like the text bellow. open the crowsnest log file visible in mainsail log area to find your device id. scroll to the end for the latest info!
```
[cam raspi]
mode: camera-streamer                      # ustreamer - Provides MJPG and snapshots. (All devices)
                                        # camera-streamer - Provides WebRTC, MJPG and snapshots. (only RPiOS + RPi 0/1/2/3/4)
enable_rtsp: false                      # If camera-streamer is used, this also enables usage of an RTSP server
rtsp_port: 8554                         # Set different ports for each device!
port: 8080                              # HTTP/MJPG stream/snapshot port
device: /base/soc/i2c0mux/i2c@1/imx219@10                     # See log for available devices
resolution: 1640x1232                      # <width>x<height> format
max_fps: 5                             # If hardware supports it, it will be forced, otherwise ignored/coerced.
```
---
Wifi issues:  
Setting your routers WiFi to another chanel fixes the delay issue so you can use the terminal without that 10 min delay. It will fix it for 24  hours or so.
To keep this from happening, there seems to be a bug in bookwork that sets power saving to ON. As far as I can tell the pi does not have power saving. Setting wifi power to off seems to fix the issue.  
It might also be related to an incorrect time.  Time servers are not set up in raspian bookworm for some reason.
checking if power saving is on:
terminal command:    
```
journalctl | grep brcmfmac
```
Set wifi power off:
1: create a new service so it's persistent:
Terminal Command:
```
sudo systemctl --full --force edit wifi_powersave_off.service
```
in nano, enter this into the blank file and save:
```
[Unit]
Description=Set WiFi power save off
After=sys-subsystem-net-devices-wlan0.device
[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/sbin/iw dev wlan0 set power_save off
[Install]
WantedBy=sys-subsystem-net-devices-wlan0.device
```
2: enable the service:
terminal command:
```
sudo systemctl enable wifi_powersave_off.service
```
---
you must setup time server.  
username@YourKlippedPrinter:~ $ 
```
sudo nano /etc/systemd/timesyncd.conf
```
enter find some time servers and enter them into the NTP field.  You may also just uncomment the FallbackNTP field and that should take care of the issue.
```
[Time]
NTP=0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org
FallbackNTP=0.debian.pool.ntp.org 1.debian.pool.ntp.org 2.debian.pool.ntp.org 3.debian.pool.ntp.org
#RootDistanceMaxSec=5
#PollIntervalMinSec=32
#PollIntervalMaxSec=2048
#ConnectionRetrySec=30
#SaveIntervalSec=60
```
Note the time with the shell command

```
date
```
</h1>
Shutdown the Pi softly and wait a few minutes. It does not have a RTC let alone a battery powered one. When you power it back on, log in again and check the time again with the date command. As long as you have an internet connection it should not lag behind the time you had the pi off.
---

<h1>Raspicam Cheat Sheet:</h1>

http://192.168.101.99/webcam/control
or
http://192.168.101.99/webcam

---
initial setup of Klipper is best done by

1: installing raspian with the raspberry pi imager. Use the lite 32bit image for the Pi Zero. https://www.raspberrypi.com/software/

2: install Klipper with KIAUH https://github.com/dw-0/kiauh
