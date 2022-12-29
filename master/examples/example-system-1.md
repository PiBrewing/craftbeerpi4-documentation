# Example System: Speidel Braumeister 20

- This setup is working with a  a Speidel Braumeister 20 Plus (2015). It should also work with the 10  and 50 Liter models. 
- However, for the 50 Liter model you need to figure out on how to manage the two pumps and heaters. I recommend the GroupedActor Plugin to combine two actors into one.

## Hardware requirements 

- I recommend to use the original temp sensor from the Braumeister which is a 2 wire PT1000. In this case you don't need to deal with thermowells that may not fit into the existing hole of the Braumeister.
-  Therefore you need to add a max31865 board (incl. a 4300 ohm resistor for PT1000) to your craftbeerpi hardware setup. (https://learn.adafruit.com/adafruit-max31865-rtd-pt100-amplifier/)
- To connect to the probe, you need a Binder coupling connector (https://www.conrad.de/de/p/binder-99-0406-00-03-rundstecker-kupplung-gerade-serie-rundsteckverbinder-712-gesamtpolzahl-3-1-st-738917.html)
- You just need to unplug the probe from the Braumeister controller and plug your cable with the aforementioned connector to your craftbeerpi setup
- To connect the pump and the heater, you will need hirschmann connectors. I am using the following connectors
- Hirschmann STAK 2 for the pump (https://www.conrad.de/de/p/hirschmann-stak-2-netz-steckverbinder-stak-serie-netzsteckverbinder-stak-buchse-gerade-gesamtpolzahl-2-pe-16-a-gra-1177484.html)
- Hirschmann STAK 200 for the heating element (https://www.conrad.de/de/p/hirschmann-stak-200-netz-steckverbinder-stak-serie-netzsteckverbinder-stak-buchse-gerade-gesamtpolzahl-2-pe-16-a-g-730025.html)
- You will need 2 Hirschmann safety Clips (https://www.conrad.de/de/p/sicherungsbuegel-hirschmann-730980.html)
- Just unplug pump and heater and connect it to your Relais outputs. I am using SSR for both, heater and pump. They can handle 20A @ 240V AC
- Thats about it for the hardware part.

![](../../.gitbook/assets/dchandlr\_dchandlr\_work.svg.svg)
