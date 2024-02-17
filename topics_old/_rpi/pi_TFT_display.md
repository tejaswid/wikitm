---
title: Setting up a TFT display for a Raspberry Pi
layout: page
---

In this tutorial, we shall look at setting up a TFT display.

The setup used here:  
1. Raspberry pi zero W with a fresh installation of Jessie (2017-07-05)
2. A 2.2" ili9341 TFT display without touch

Most tutorials go through the process of compiling a custom kernel to install the fbtft drivers provided by [Notro](https://github.com/notro/fbtft).
However, this is no longer required as newer versions of Raspbian provide it by default. This information has been compiled from that provided [here](https://www.raspberrypi.org/forums/viewtopic.php?f=91&t=111817) and [here](http://lallafa.de/blog/2015/03/fbtft-setup-on-modern-raspbian/). 

## Update and upgrade

A good first step is to update your raspberry pi.

```
sudo apt-get update
sudo apt-get upgrade
```

I had issues with `rpi-update`. In general, I would not recommend doing that.

## Connecting your display to the pi

There is a specific way in which the display must be connected to the pi. This detail is provided in the rpi-display overlay in the **/boot/overlays** folder.
To access it use the device tree compiler to convert the overlay file into a human readable format.

```
dtc -Idtb -Odts /boot/overlays/rpi-display.dtbo  | less
```

In the **rpi-display@0** section you will see details of pin numbers. The numbers provided here are in hex format. You will need to convert them to decimal.

For example, the RESET pin is labelled to connect to number 17 in hex, which is 23 in decimal. This 23 corresponds to BCM / GPIO 23. Which is pin 16 counted from the top left, row-wise. You can use the following command to convert to decimal from the command line.

```
echo "ibase=16; 17" | bc
```

Do the conversion for all the pins. You will end up with something like this

Raspberry pi pins    ---     Screen  
 3v3	pin 17 	     ---	 VCC  
 GND	pin 20 	     ---	 GND  
 BCM 8	pin 24	     ---	 CS  
 BCM 23	pin 16	     ---	 RESET  
 BCM 24	pin 18	     ---	 DC  
 BCM 10	pin 19	     ---	 MOSI  
 BCM 11	pin 23	     ---	 SCK  
 BCM 18	pin 12	     ---	 LED  
 BCM 9	pin 21	     ---	 MISO  

Hook up the screen according to the table. For the LED backlight, use a resistor or transistor as you see fit. I used a 100 Ohm resistor.
The idea is to limit the current drawn from the GPIO pin. Adjust for the brightness you seek.

## Configure screen

Let us now configure the screen. Edit the config.txt file as follows

```
sudo nano /boot/config.txt
```

At the end of the file add the following.

```
# TFT screen configuration
dtoverlay=rpi-display
```

You can change other parameters such as orientation later. Save the file, close it and reboot. Your screen should turn on. Let us check if it is being recognised. Type the following into the terminal to look for frame buffer devices.

```
ls /dev/fb*
```

If the response shows two devices, `/dev/fb0` and `/dev/fb1` that is good. Your TFT screen is `/dev/fb1`.

## Boot console on the screen

Open **/boot/cmdline.txt**

```
sudo nano /boot/cmdline.txt
```

Add the following at the end of the first line. Be careful with this file. The format is very specific.

```
fbcon=map:10 fbcon=font:VGA8x8 logo.nologo
```

The first part maps the boot console to frame buffer 1. Then we change the font to a small size and remove the raspberries that appear on the top left of the screen while booting. Reboot and you should see the console appear on your screen. If you have an external HDMI screen plugged in while doing this, you will notice that the boot messages appear on the TFT. The HDMI display goes to the splash screen and desktop after booting.

## Get X11 on the TFT

To get X11 on the TFT, create a configuration file as follows

```
sudo nano /usr/share/X11/xorg.conf.d/99-fbdev.conf
```

Add the following lines

```
Section "Device"
   Identifier "myTFTdevice"
   Driver "fbdev"
   Option "fbdev" "/dev/fb1"
EndSection
```

We are changing the device option to `/dev/fb1`. If you now type `startx` you should see the desktop on the TFT.

A bit of troubleshooting:  
When you reboot, you might get stuck in a loop at the login screen. I think the issue occurs if the default password has not been changed.
To solve this, enter the command line using Ctrl+Shift+F1. Then type

```
cd ~/
mv .Xauthority .Xauthority.backup
```

If you reboot, you should be able to login without hassle.

## Configure to switch between TFT and HDMI

TODO
