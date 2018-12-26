---
title: Broadcasting Internet From A Windows Laptop
keywords: windows, broadcast, internet, wifi
summary: "Explains how to broadcast internet from a Windows laptop"
sidebar: misc_sidebar
permalink: misc_broadcasting_internet.html
folder: misc
---

## Scenario
You receive internet through an ethernet cable (LAN) or a USB dongle. You want to share this internet to laptops, mobile phones, tablets etc.

Here is the simplest way to do it on a Windows system that has a suitable wifi card using the command line.

## How to
- Open a command prompt.
- First check if your wifi card supports sharing by typing
```
netsh wlan show drivers
```
Look for the property: **Hosted network supported**.  It should say **yes** here.

- Set up a Network
```
netsh wlan set hostednetwork mode=allow ssid=Some_Name key=Some_Password
```

- Check if the network has been created
```
netsh wlan show hostednetwork
```
It should show you the details and in the status property, where is should say **not started**.

- Start the network
```
netsh wlan start hostednetwork Some_Name
```
Now it should say that the network has **started**.

- After doing this, open **network and sharing center**, and chose **change adapter settings** in the left side panel. There, right click on the connection you want to shareand open up **Properties**.

- In the properties window, click on the **sharing tab** and check **Allow other network users to connect through this internet connection**. No need to tick the second box.

-  Try connecting to this network on your phone or tablet. Everything should work. Access a web page to make sure it is working.

- To Stop broadcasting
```
netsh wlan stop hostednetwork Some_Name
```

In general, the network gets deleted when you restart the computer. To start it, you will have to do steps 2 and 4 each time. To make it simple, you can open up notepad, paste these two lines (step 2 and 4) and save it as a .bat file. Put the file on your desktop, right click and **run as administrator**.

**Note**
Step 5 and 6 are not necessary every time. If you are changing the source from which you are sharing, then you have to do this. You can only share, through wifi, the internet you are getting from an ethernet cable (LAN), or some other USB dongle internet. You cannot connect to a new wifi network when you are sharing since your wifi is now acting like a broadcaster. In order to connect to another wifi network you have to stop broadcasting the current network (step 8). SO save that as another .bat file.

{% include links.html %}
