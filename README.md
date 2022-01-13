---
description: Software for brewing and fermenting beer
---

# Craftbeerpi4

## Craftbeerpi 4 overview

Craftbeerpi4 is an open source software solution developped by Manuel Fritsch to control the brewing and fermentation of beer and maybe more in future. The server side is based on python 3 and the front end on a react / Material UI interface. The hardware is focused on the RapsberryPi as this board has plenty of GPIO's to read sensors and control actors. However, it is also possible to connect other sensors and actors via http connection. Therefore, plugins are required.

The user interface has the option to define up to 10 different dashboards and separate brewing and fermentation or define a simple dashboard for your cellphone.

Below you can see an example dashboard for a single vessel brew setup.

![Brewing Dasboard](.gitbook/assets/cbpi4\_brew.png)

On a second dashboard you can add for instance your fermenters:

![Fermenter Dashboard](.gitbook/assets/cbp4\_ferment.png)

Below you can see an example of a dashboard that is intended for a mobile phone.

![Dashboard 4 example for a mobile device](.gitbook/assets/cbpi\_mobile\_dashboard.jpg)

You can write your recipes directly from the user interface,

![Recipe Editor](.gitbook/assets/cbpi4\_mash\_profile.png)

or you can import recipes through several ways such as beer.xml files, the 'Kleiner Brauhelfer 2' database or directly via the Brewfather app API (Paid Brewfather account required).

![Recipe Upload](.gitbook/assets/cbpi4\_recipe\_upload.png)

The required mash- and boil-steps will be created automatically incl. alarms for hop / misc additions.

Details on how to install the server and user interface, how to install plugins and how to configure the software are described in the next chapters.
