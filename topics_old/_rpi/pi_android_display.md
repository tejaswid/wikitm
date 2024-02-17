---
title: Setting up an Android Display
layout: page
---

## Beginning
Let us set up an Android device as a display for the raspberry pi. Once set up, you can use it without a mouse, keyboard and a HDMI equipped display.
You can still use an external devices as we will be using only one usb port on the pi. This method only works with an android device that has a USB tethering option.

It is good to be able to see things when changing settings. So connect a keyboard, mouse and display and boot up.

## Enabling SSH on the pi
SSH needs to be running on the pi at startup. This is usually the default case. Else, go to raspberry pi configuration and enable SSH on boot.


## Configure the USB port on the pi
Open a terminal and do the following

```
sudo nano /etc/network/interfaces
```

Add these lines at the end

```
allow-hotplug usb0
iface usb0 inet dhcp
```

Changes will take place after you reboot. Or you could type

```
sudo /etc/init.d/networking restart
```

Note down the IP of the pi. If you do an `ifconfig` it should show up under usb0 as something similar to inet addr:192.168.42.41
If it doesnt show up, do this bit after you connect your pi to an android device while also keeping the keyboard and screen connected.

You could also add a static IP. Maybe it is better. TODO: update this with command for adding static IP
Maybe this link helps [https://www.raspberrypi.org/forums/viewtopic.php?t=18916&p=331361](https://www.raspberrypi.org/forums/viewtopic.php?t=18916&p=331361)

## Enable X11 Forwarding (optional)
If you are interested in working with a GUI, then enable X11 forwarding. In the terminal type

```
sudo nano /etc/ssh/sshd_config
```

Uncomment the line that has X11 forwarding set to yes. It should look like this

```
X11Forwarding yes
```

We are done configuring the pi. Shut it down.

## Download a terminal emulator on your android device
Check that your device has a USB tethering option. It will usually be under Settings --> more networks --> USB tethering

Download a terminal emulator of your choice. I prefer connectBot since it is light on the device.

## Connect your pi to the android device
First connect a USB port on the pi to the USB port on your device. Then, power up the pi and wait for some time. The indicator LED will stabilize after a while, takes about a minute. Then enable USB tethering: Settings --> more networks --> USB tethering.

## SSH into the pi
Open connectBot and ssh into the pi using the same IP we noted down above.
```
ssh pi@192.168.42.41
```
default password for the pi is raspberry. Enter that and you are in.

At this point you have full access to the pi.

## Enabling pi to use your android device's wifi / 3g connection
You might want to let your pi have access to the internet. For this open up the terminal and enter the following commands

```
sudo su -
arp -a
```

The above line should give you the IP address of your android device.

```
route add default gw 192.168.42.123 usb0
```

You should now have access to the internet. Try pinging www.google.com and you should get responses. You might have to add this setting each time you log in. 
TIP: if you tap above the keyboard on the connectBot screen, you can press the Ctrl button. Useful to kill applications.

The next steps will get you started if you want to use a GUI on the android device. Disconnect from the SSH session before doing the next steps.

## Configure port forwarding
In the connectBot app, hold down on the connection name to get an option to configure port forwarding.

These are the setting you should use:
mode = remote
from port = 6000
to address:port = localhost:6000

You can also choose the from port to be 6001, 6002 etc. It might happen that 6000 is allotted to the HDMI display if one is connected.

## Get an X server app
I downloaded the XServer XSDL app. The simple X Server app in uninformative and did not work.

## Making things work
If you were already in a session, logout and close the connectBot app.

First start the XServer XSDL app. It will give you the option of choosing screen resolution and font size. It will then give you instructions on what to enter into a Linux PC. Note these down.

Then open connectBot and login through SSH. The port forwarding option should be enabled and not crossed out.
After you have successfully logged in, enter the commands that XServer XSDL told you to enter. It will be something like

```
export DISPLAY=192.168.42.123:0
```

Just this command should do. No need to do the `PULSE_SERVER` command. The IP address is that of your android device (same as in step 8).
Dont forget the :0 in the end. If you used port 6001 or 6002, change this to :1 or :2 accordingly.

Then launch any x11 app

```
xlogo
```

Other examples are `xterm` and `xclock`. You might have to do an apt-get install x11-apps to be able to run these commands

Now, if you switch over to the XServer XSDL app, you should see the app you called. xlogo will display the x.org logo

## Full blown UI
For the full blown raspberry pi UI, just type

```
startlxde-pi
```

Be patient and a raspberry will pop up.

TIP: move your finger on the screen to move the mouse. Tap to click.  
TIP: pressing the back button on the android device will toggle the keyboard.

References:  
[http://elinux.org/How_to_use_an_Android_tablet_as_a_Raspberry_Pi_console_terminal_and_internet_router](http://elinux.org/How_to_use_an_Android_tablet_as_a_Raspberry_Pi_console_terminal_and_internet_router)  
[https://www.raspberrypi.org/forums/viewtopic.php?t=18916&p=331361](https://www.raspberrypi.org/forums/viewtopic.php?t=18916&p=331361)  
[http://sonelli.freshdesk.com/support/solutions/articles/182200-how-to-tunnel-x-over-ssh-using-port-forwarding](http://sonelli.freshdesk.com/support/solutions/articles/182200-how-to-tunnel-x-over-ssh-using-port-forwarding)  
[https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=105336](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=105336)  
