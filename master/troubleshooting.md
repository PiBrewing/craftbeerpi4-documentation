# Troubleshooting

In case of issues with the software and questions you need to share at least some information to get help and save time. Otherwise questions can't be answered.

1. Describe your problem including error messages and all the information that might be important. Questions where other might require a crystal ball to get insight into the issue are typically not (properly) answered. If you are asking something like 'My system is not working, what am I doing wrong?', it is very unlikely, that you will get an answer at all. Just think, what Information you would need if somebody is asking you a question.
2. What version of server have you installed?
3. What version of user interface are you running?
4. If you have problems with a plugin: How did you install the plugin?
e.g. `python -m pip install https://github.com/PiBrewing/cbpi4-pt100x`
Inside the virtual environment? 
5. In case of issues with the server: How did you install the server / user interface?
6. If you run the server as system service, you can stop the service and [start the server in manual mode](server-installation.md#automatically-start-the-server-as-service). -> You will see logging and maybe also errors that may cause the issue. Report these errors as they may help to identify the problem.
7. The server shows only warning and error log by default. If you want to see more, start the server with the -d option (e.g. sudo cbpi -d 20 start) to see also informational logging.
8. The server has also the possibility to download the log file via the [system page](craftbeerpi-4-server/system.md)


Here you can see how the startup should look like if all plugins have been loaded correctly. If not, you would see error messages. (Informational logging enabled)

```
Jan 25 13:47:58 raspberrypi systemd[1]: Started Craftbeer Pi.
Jan 25 13:48:05 raspberrypi cbpi[360]: 2022-01-25:13:48:05,154 INFO     [craftbeerpi.py:90] Init CraftBeerPI
Jan 25 13:48:05 raspberrypi cbpi[360]: 2022-01-25:13:48:05,437 INFO     [eventbus.py:56] Topic #
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,935 INFO     [craftbeerpi.py:248]
Jan 25 13:48:07 raspberrypi cbpi[360]:   _____            __ _   ____                 _____ _
Jan 25 13:48:07 raspberrypi cbpi[360]:  / ____|          / _| | |  _ \               |  __ (_)
Jan 25 13:48:07 raspberrypi cbpi[360]: | |     _ __ __ _| |_| |_| |_) | ___  ___ _ __| |__) |
Jan 25 13:48:07 raspberrypi cbpi[360]: | |    | '__/ _` |  _| __|  _ < / _ \/ _ \ '__|  ___/ |
Jan 25 13:48:07 raspberrypi cbpi[360]: | |____| | | (_| | | | |_| |_) |  __/  __/ |  | |   | |
Jan 25 13:48:07 raspberrypi cbpi[360]:  \_____|_|  \__,_|_|  \__|____/ \___|\___|_|  |_|   |_|
Jan 25 13:48:07 raspberrypi cbpi[360]:                                                        
Jan 25 13:48:07 raspberrypi cbpi[360]:                                                        
Jan 25 13:48:07 raspberrypi cbpi[360]:  _  _    ___  __ _  _
Jan 25 13:48:07 raspberrypi cbpi[360]: | || |  / _ \/_ | || |
Jan 25 13:48:07 raspberrypi cbpi[360]: | || |_| | | || | || |_
Jan 25 13:48:07 raspberrypi cbpi[360]: |__   _| | | || |__   _|
Jan 25 13:48:07 raspberrypi cbpi[360]:    | |_| |_| || |_ | |
Jan 25 13:48:07 raspberrypi cbpi[360]:    |_(_)\___(_)_(_)|_|
Jan 25 13:48:07 raspberrypi cbpi[360]:                          
Jan 25 13:48:07 raspberrypi cbpi[360]:                          
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,936 INFO     [craftbeerpi.py:249] www.CraftBeerPi.com
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,936 INFO     [craftbeerpi.py:250] (c) 2021 Manuel Fritsch
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,962 INFO     [plugin_controller.py:32] Trying to load plugin httpsensor
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,978 INFO     [plugin_controller.py:32] Trying to load plugin mqtt_sensor
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,986 INFO     [plugin_controller.py:32] Trying to load plugin FermenterHysteresis
Jan 25 13:48:07 raspberrypi cbpi[360]: 2022-01-25:13:48:07,995 INFO     [plugin_controller.py:32] Trying to load plugin mashstep
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,10 INFO     [plugin_controller.py:32] Trying to load plugin hysteresis
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,18 INFO     [plugin_controller.py:32] Trying to load plugin FermentationStep
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,28 INFO     [plugin_controller.py:32] Trying to load plugin ConfigUpdate
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,83 INFO     [plugin_controller.py:32] Trying to load plugin gpioactor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,102 INFO     [plugin_controller.py:32] Trying to load plugin dummyactor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,110 INFO     [plugin_controller.py:32] Trying to load plugin dummysensor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,119 INFO     [plugin_controller.py:32] Trying to load plugin mqtt_actor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,134 INFO     [plugin_controller.py:32] Trying to load plugin onewire
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,226 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4ui
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,241 INFO     [plugin_controller.py:57] Plugin cbpi4ui loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,241 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-BM_PID_SmartBoilWithPump
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,251 INFO     [plugin_controller.py:57] Plugin cbpi4-BM_PID_SmartBoilWithPump loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,251 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-buzzer
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,260 INFO     [plugin_controller.py:57] Plugin cbpi4-buzzer loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,261 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-iSpindle
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,273 INFO     [plugin_controller.py:57] Plugin cbpi4-iSpindle loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,274 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-PID_AutoTune
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,282 INFO     [plugin_controller.py:57] Plugin cbpi4-PID_AutoTune loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,283 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-pt100x
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,294 INFO     [plugin_controller.py:57] Plugin cbpi4-pt100x loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,294 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-PushOver
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,300 INFO     [plugin_controller.py:57] Plugin cbpi4-PushOver loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,300 INFO     [plugin_controller.py:53] Try to load plugin:  LCDisplay
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,345 INFO     [plugin_controller.py:57] Plugin LCDisplay loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,346 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-system
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,572 INFO     [plugin_controller.py:57] Plugin cbpi4-system loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,572 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-scd30_CO2_Sensor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,594 INFO     [plugin_controller.py:57] Plugin cbpi4-scd30_CO2_Sensor loaded successfully
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,595 INFO     [plugin_controller.py:53] Try to load plugin:  cbpi4-KettleSensor
Jan 25 13:48:08 raspberrypi cbpi[360]: 2022-01-25:13:48:08,601 INFO     [plugin_controller.py:57] Plugin cbpi4-KettleSensor loaded successfully
```

