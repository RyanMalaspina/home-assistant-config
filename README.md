# Home Assistant Config

These are my config files for [Home Assistant](https://home-assistant.io/). 
I've switched to running Home Assistant in a docker container on my RPi3 for easy upgrading and portability if I want to move it to a different device later. 
With Docker and Docker Compose installed, `cd` into this directory and fire it up with
```bash
docker-compose up -d
```
Note: If you're not running it on a Pi use the traditional Linux image: `homeassistant/home-assistant` instead.

## Device Aliasing
To auto-alias USB devices like I have with `/dev/hdmi` and `/dev/zwave` you'll first have to find the vendor and product ids of the device.
You can do this by running `lsusb`. I have a Prolific PL2303 USB to Serial for my HDMI switch and it lists as:
```
Bus 001 Device 004: ID 067b:2303 Prolific Technology, Inc. PL2303 Serial Port
```
The ids (067b:2303) are the vendor and product ids.
Add a udev rule to automatically create a symlink for this device:
```
# /etc/udev/rules.d/99-usb-serial.rules
SUBSYSTEM=="tty", ATTRS{idVendor}=="067b", ATTRS{idProduct}=="2303", SYMLINK+="hdmi"
```
