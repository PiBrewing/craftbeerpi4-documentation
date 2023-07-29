---
description: Here is the description of the recommanded hardware configuration for CBPI.
---

# Recommended Hardware

## Recommended hardware configuration

The latest version of Raspberry Pi is Raspberry Pi 4 Model B.

With this version, you can choose your RAM from 2gB to 8gB. The 2gB version is sufficient to run CBPI V4.

{% hint style="warning" %}
I have tested a CraftbeerPi4 (<= 4.1.6) Installation ona Pi Zero W. Although a bit slow during startup, the server seems to be working. However, memory might be a limiting factor and you should definitely not think about installing and running Chromium on the Zero. This will keep the CPU quite busy.

Due to updates on some required packages, there are [complications](https://github.com/PiBrewing/craftbeerpi4/issues/108) with the Pi zero gen 1 and the Pi 1 (armv6l based devices). These devices are not supported anymore, but you can try to follow the instructions in the aforementioned link to downgrade the cryptography related packages. There won't be any support from cbpi side for those old devices.

The Zero 2 W has been tested successfully with a 64 bit based raspbian and most recent versions of cbpi4 are running on this pi if you don't want to run the UI with Chromium directly from the pi.
{% endhint %}

For further information on Raspberry Pi, please have a look there : [https://www.raspberrypi.com/products/raspberry-pi-4-model-b/](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)

We also recommend to use a stable power supply, at least 15W USB-C power supply. For about 10 dollars, you'll buy the official raspberry power supply :).

Take care of the global power supply. Your supply needs to be sufficient to supply the Pi but also the other hardware unless you choose to have two separate supplies.

We also need a SD card to install CBPI. We recommend to use at least a 16gB SD card.&#x20;

{% hint style="info" %}
The server is writing continuously data to the drive which bears the risk that your sd-card will get corrupted. Therefore, it is recommended to use a hard disk with a USB3 to SATA adapter
{% endhint %}

To finish, you also need a USB mouse and a USB keyboard.

## Display configuration

The Pi has different options to connect a display.&#x20;

The first possibility is HDMI. Raspberry Pi 4 uses micro HDMI ports (2) so you need one cable to connect to one display via Raspberry Pi 4's micro HDMI ports. Whereas it is possible to use a touch screen, we recommend to use a classic LCD screen. It will be easier to set up and use CBPI. &#x20;

The second option is the installation of a display that is connected to the [DSI](https://de.wikipedia.org/wiki/Display\_Serial\_Interface)interface of the raspberry. There are several displays available. The original raspberry display has only 7" with a touch screen included but is working pretty reliable if you use also the multiple dashboard function.

## CBPI Extension board

It will be easier to set up your brewery if you use a CBPI extension board.

Manuel Fritsch, creator of CBPI, developed few months ago the CBPI extension board V3. It costs about 55$ but it is really priceless...

Unfortunately, Manuel doesn't answer for the moment and we don't know when he will come back to the project. New projects of extension board are under development... Keep patient, that's currently the best option.&#x20;

To order, send an email to order@craftbeerpi.com and see if Manuel answers.

## Where to buy ?

It is right now difficult to buy a Raspberry Pi, due to the global shortage of semiconductors components.

We recommend to check on the official website to find a Raspberry Pi Approved Resellers. Unfortunately, prices are getting high for this reason.

Let's have a look there : [https://www.raspberrypi.com/products/raspberry-pi-4-model-b/](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)

