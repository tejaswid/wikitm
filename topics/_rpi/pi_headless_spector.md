---
title: Setting up a Headless Raspberry Pi
layout: page
---

The goal of this tutorial is to configure a raspberry pi zero such that it can be accessed via usb from a computer without the need for a second display and keyboard.

**Note that this method only works on a raspberry pi zero.**

References used:  
1. [https://blog.gbaman.info/?p=791](https://blog.gbaman.info/?p=791)
2. [https://gist.github.com/gbaman/50b6cca61dd1c3f88f41](https://gist.github.com/gbaman/50b6cca61dd1c3f88f41)

My setup:  
Ubuntu 14.04 running on my laptop  
Raspbian Jessie 2016-05-27  

## Burn a Raspbian image

Download the latest Raspbian image from this link  Then burn the image onto an SD card following the instructions appropriate to your operating system. For Ubuntu, go [here](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md). To summarise, the command looks something like this

```
dd bs=4M if=2016-05-27-raspbian-jessie.img of=/dev/mmcblk0
```

After you are finished, unplug the SD card and insert it again. You should see two partitions (if you are on Ubuntu) or one partition (if you are on Windows).

## Edit config.txt in the boot partition

Make yourself super user

```
sudo su
```

Now, find where the boot partition is loaded

```
df -h
```

From the size of your SD card, you should be able to recognise the device. It might show up as **/dev/sdx** or **/dev/mmblk0**.
On the last column of the table, you will see where the boot partition and main partition are mounted to. This might be of the type **/media/userName/boot** and **/media/userName/someNumbers**. Change directory to the boot partition.

```
cd /media/userName/boot
```

look for the file config.txt and open it.

```
gedit config.txt
```

On a new line at the end, add the following

```
dtoverlay=dwc2
```

Save and close the file.

Update: While you are at it, enable SSH. Some versions of Raspbian do not enable it by default. You can do this by creating an empty file named ssh without any extension in the boot partition. As per the official documentation, when the pi boots, it looks for this file and if it is present, enables SSH and deletes the file.

## Edit cmdline.txt in the boot partition

Now open cmdline.txt.

```
gedit cmdline.txt
```

The text in this file is formatted to be on a single line with a single space between arguments.
Insert the following after **rootwait**

```
modules-load=dwc2,g_ether
```

Note that there is no space after the comma. There should be a space after rootwait and the above words. The next word should begin after a space following g_ether. Save and close the file.

## Power up the raspberry pi

Eject the SD card and insert it into the pi. Power it on through the pwr port.
Once it boots up, connect a USB cable between your computer and the usb port on the raspberry pi.
The pi should be detected as a ethernet over usb device when you do

```
ifconfig
```

## Find out the IP address of the pi

avahi-browse -art

The above command should print out the ipv6 address of the raspberry pi. If it does not print out the IP address, then it has not been detected.

Now try pinging the pi using the ipv6 address. I am assuming it is fe80:xxxx:xxxx:xxxx:xxxx

```
ping6 -I usb0 fe80:xxxx:xxxx:xxxx:xxxx
```

If that succeeds, you are ready to ssh into the pi

```
ssh -6 pi@fe80:xxxx:xxxx:xxxx:xxxx%usb0
```

Note that the last part is the device name that you got from `ifconfig` or `avahi-browse` command. 
Default password is raspberry and you should be able to login.

Note: If you have the latest Raspbian OS, Pixel installed (Oct,2016), then you can also ssh into the pi using the following command.

```
ssh pi@raspberrypi.local
```

Tip: Once inside the pi, do an ifconfig to get the ipv4 address that you can use even if you are on putty in windows later on.

## Troubleshooting

**Problem with step 2 on windows**  
When you burn an image onto the SD card, it creates two partitions. One is a boot partition in FAT16 (32?) format and the other is the main partition in EXT3 format. Windows is able to recognise the FAT partition, but depending on your operating system and administrator rights, you may or may not be able to open this partition. You will have to switch over to a linux machine. Windows cannot read the main partition.

**Problem with detecting IP address**
On windows, if you plug in the pi after step 3, it will be detected as a USB Ethernet/RNDIS Gadget under Network Adapters in the Device Manager. If you try an ipconfig, it will show up as an ethernet connection.
However you will not be able to connect the the IP address that is shown. 

If you install bonjour print service that comes with apple itunes, you can connect to the pi using the address raspberrypi.local
Else you will need to get the correct IP of the pi. You can get this by using a linux system and follow step 5 and the tip above.

On Ubuntu, I had to power the pi first through the pwr port and connect a usb cable from the usb port to the computer. Only then it was detected as a ethernet device and got assigned an IP address. It turns out that on windows you can power the pi using a single usb cable on the usb port. No need to connect anything to the pwr port.
