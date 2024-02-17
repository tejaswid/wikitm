---
layout: page
title: Broadcasting internet from your Windows computer
thumbnail:  '/assets/thumbnails/cat_200x200.png'
blurb: A simple way to broadcast internet from an Windows PC.
date_published: 26th August, 2013
categories: [Miscellaneous, Windows]
---
# Broadcasting internet from your Windows computer

Scenario: You receive internet through an ethernet cable (LAN) or a USB dongle. You want to create a hotspot and share this internet to other devices.

Here is the simplest way to do it using the command prompt on a Windows system that has a suitable wifi card.

Open command prompt.

1. First check if your wifi card supports sharing:

	```
	netsh wlan show drivers
	```
	Look for the property: Hosted network supported.  It should say yes here.

2. Set up a network: Replace Some_Name and Some_Password with what you like.

	```
	netsh wlan set hostednetwork mode=allow ssid=Some_Name key=Some_Password
	```

3. Check if the network has been created:

	```
	netsh wlan show hostednetwork
	``` 
	It should show you the details and in the status property. It should say not started.

4. Now start the network:

	```
	netsh wlan start hostednetwork The_Name_You_Gave_It
	```
	It should now say started.

5. After doing this, open network and sharing center, and chose change adapter settings in the left side panel. There, right click on the connection you want to share. It could be your LAN or USB device. Open up properties.

6. In the properties window, click on the sharing tab and check *Allow other network users to connect through this internet connection*. No need to tick the second box.

7. Try connecting to this network on your phone or tablet. Everything should work. Access a web page to make sure it is working.

8. To Stop broadcasting:

	```
	netsh wlan stop hostednetwork The_Name_You_Gave_It
	```

In general, the network gets deleted when you restart. To start it, you will have to do steps 2 and 4 each time. To make it simple, you can open up notepad, paste these two lines (step 2 and 4) and save it as a .bat file. Put the file on your desktop, right click and run as administrator.

Steps 5 and 6 are not necessary every time. If you are changing the source from which you are sharing, then you have to do this. You can only share, through wifi, the internet you are getting from an ethernet cable (LAN), or some other internet dongle. You cannot connect to a new wifi signal when you are sharing since your laptop is now acting like a broadcaster.

If you move to another location and want to connect to wifi there, your wifi will be disabled (indicated by a red x mark). You have to first stop this network to free your wifi card. So, save step 8 as another batch file. Now you have a start button and a stop button on your desktop. Simply click them whenever you want