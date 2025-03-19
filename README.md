<h1>WiFi and camera-streamer issues with the Pi Zero 2W.</h1> 
Look at the Bookworm+Pi_Zero_2W_Notes.md file for the notes taken when resolving these issues.

[Skip to the fix: ](https://github.com/avongil/PiZero_Bookworm_Notes/blob/main/Bookworm%2BPi_Zero_2W_Notes.md)

---

There are widespread ssh wifi lag issues with the Pi Zero. When you log into the Pi, it imediatly hangs.  It takes serveral minutes to even list a directory.  

There will also be random and frequent disconnects on the fluidd or mainsail interface.
There seems to be lots of conflicting info online. I have confirmed they are not due to signal stregth.   They seem to be connected to power saving or having a diffrent time than your DNS.

---

<h1> Camera-Streamer issues:  </h1> 
No connection or just the first few frames.  Loss of connection after a few minutes. 
These seem to be linked to not having enough GPU memory allocated since the GPU is used in camera-streamer.  

---
<h1>Do I really want to use a Pi Zero? </h1> 
If you really want to avoid any headaches and have the added benifit of a wired connection and regualr usb ports, splurge use a Pi3B+, Pi4 or Pi5 instead. A Pi4 1 GB would be my minimum recomendations for a 3D printer with a webcam. 

The Pi Zero 2W only has 2 micro USB ports. One can be used for the MCU, one is open for the accelerometer or other option and the Pi Ribbon cable is used for the cam. 

You can fee up an addional USB by using a serial connection from the GPIO ports to drive the MCU. This is a bit more invlolved and will require a custom header to USB cable.

Unless you need the space savings, a full size Pi is certainly faster to set up and packs more camera streaming performance. It is probably even more cost effective considering the added ports don't require additional cables that you might not have.

[Click here for the FIX](https://github.com/avongil/PiZero_Bookworm_Notes/blob/main/Bookworm%2BPi_Zero_2W_Notes.md)