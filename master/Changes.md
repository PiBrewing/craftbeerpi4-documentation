# Changes

## UI Version 0.3.18.1 and 0.3.18.2 has the following changes:

### Maintenance:
- Improve Recipe widget and add maxheight and scrollbar, if widget is higher than maxheight.
- Minor tweaks to ispindle data pages (automatically reload data on currentdata page and improved behavior for xaxis range of diagrams)

### Fixes:
- Fix intermittent issue when changing between dashboards (empty dashboard appears). Improves also speed (issue #60)
- Fix issue for temperature sliders when changing temp omn server side  (issue #61)




## Server Version 4.5.1 has the following changes:

### Fixes:
- Minor adaptions to notification controller to prevent issues on the UI side (convert message to string)

### Features:
- Allow different Cooldown steps (Step Type must start with 'Cooldown') in order to support new [cooldown step plugin](https://github.com/PiBrewing/cbpi4-cooldown-braumeister)


## UI Version 0.3.18 has the following changes:

### Features:
- Add Spindle data pages (iSpindle plugin >= 1.0.0 required)


## Server Version 4.5.0 has the following changes:

### Maintenance:
- Add swagger descriptions for more api functions (e.g. bf upload)
- Use SIGKILL instead of SIGTERM to stop service (working, but other solutions to be analyzed in future)
- Remove asyncio_timeout from requirements and use python integrated version
- Adapt log file controller to remove deprecated pandas operations (date_parser)
- Close tasks in Notification controller
- Sort Code with isort and black
- Some changes in sattelite controller to close tasks (trying to address the sigterm/sigkill topic)

### Features:
- Add possibility to have 2nd status text for steps
- Add next hop timer information to step widget
- Add possibility to have extra pages in UI for ispindle plugin (ispindle plugin >= 1.0.0 and UI >= 0.3.18 required)


## Server Version 4.4.8 has the following changes:

### Fixes:
- Recipe import from Kleiner Brauhelfer database (corrupted during brewfather V2 api update)

## Server Version 4.4.7 has the following changes:

### Maintenance:
- Update requirements for packages and address dependabot warnings.

### Fixes:
- Fix in basiccontroller to prevent issue in case plugins have been removed but actors are still in the config. This is also true if config is being restored and not all plugins have been installed.

### Features
- Raise Error notifications and list them at the 'alarm bell' in case hardware cannot be started (e.g. plugin is missing).

## Server Version 4.4.6 has the following changes:

### Maintenance:
- Change routine to detect user running cbpi for autostart
- Update requirements for packages and adapt some routines for numpy 2.0 (cooldown step)

### Fixes:
- Fix function to restore settings

### Features
- Add optional offset for mqtt temperature Sensor

## UI Version 0.3.17.1 has the following changes:

- Change position of notification information in tab title. Add options to pipes (opaqueness, width, color). Add progress bar to actor button for power display and for progress on compressor actor plugin
- Fix some issues if button actions are not available
- Add features for compressor actor plugin
- Add linear progress bar to buttons for power display
- Fix on update for power value on power slider if button action was used to set power


## Server Version 4.4.2 has the following changes:

### Maintenance:

- Adapted Brewfather recipe import to Brewfather V2 API (requires UI version 0.3.15)
- No limit of 250 recipes for BF import
- Additional setting parameter in Settings on how many BF recipes can be displayed at once
- All BF recipes are called once when browser is opened to reduce API calls. In case of changes in brewfather (additional or changed recipe), upload page has now the option to reload recipe list.

## UI Version 0.3.15 has the following changes:

- Adaption for BF recipe upload (requires sever version 4.4.2+)
- Add information about number of notifications to tab title

## Server Version 4.4.1 has the following changes:

### <strong>!!! libsystemd-dev requirement !!!</strong>:

- In order to change some code at a later point of time, you must install ´libsystemd-dev´ manually prior upgrading the server. In a fresh installation, this can be combined with the libatlas-base-dev installation -> documentation has been updated accordingly.
```
    sudo apt install libsystemd-dev
```
### Updated requirements:

- some requirements have been updated with respect to dependabot recommendations
- new aiomqtt version required some changes to the sattelite controller

### Fixes:

- Fix for import of KBH recipes: Temperature was somehow imported as string. Fixed but only integer values will be imported which is sufficient with respect to precision.
- Add system-python to requirements and change download of log data to prevent code injection (CVE-2024-3955).
- Remove restored config zip file from config folder.
- Add default values for fermenter hysteresis in case users don't enter a value.
- Some config adaptions to devcontainer files

### Features:

- Adapted Actor controller in order to support the [compressoractor plugin](https://github.com/pascal1404/cbpi4_compressorActor)
- Notification badge top right is now activated and notifications (up to 100) are stored. You can review/delete them, when you click on the badge icon.
- Show missing files in restore zip file content in case of issues at command line.
- Add range parameters to sensor hardware to enable different color in dashboard in case sensor value is out of range compared to target temp.

## Server Version 4.3.1 has the following changes:

- Fixed an issues for a fresh install.
- added asyncio-timeout package to the requirements as ait was missing with a fresh install. Obviously, some dependencies have changes with the change of the required packages in 4.3.0

## UI Version 0.3.14.rc0 has the following changes 

### Features:
- Download of config backup has now year and date in filename.
- Sensor value can be displayed in different color in dashboard if out of defined range -> second color can be chosen in sensor dashboard properties.
- CustomSVG can now be defined as actor dependent (one svg for actor off and one for actor on)
- Notification badge top right is now activated and notifications (up to 100) are stored. You can review/delete them, when you click on the badge icon
- Restore of config backup will also show notification after restart with information on missing files in zip content


## Server Version 4.3.0 has the following changes:

- Update of some required packages

### RPi.GPIO is replaced with rpi.lgpio to accommodate compatibility with Pi5
- <strong>On Pi 4 and below you need to remove the RPi.GPIO package from your system prior installation of cbpi4 or prior update from 4.2.0 to 4.3.0. Please read the adapted installation instructions carefully prior upgrading!!!</strong>
- <strong>It is also required to install/upgrade cbpi4 now with the `--system-site-packages` parameter with pipx to ensure that all system packages are usable in the virtual environment. This may change later, but is currently required due to dependencies of rpi-lgpio.</strong>



## Server Version 4.2.0 has the following changes:

cbpi version is available at [pypi.org](https://pypi.org/project/cbpi4/4.2.0a6/) or from the [github development branch](https://github.com/PiBrewing/craftbeerpi4/tree/development)

### Kleiner Brauhelfer import

- Craftbeerpi4 is now compatible with Kleiner Brauhelfer 2.6 database for recipe import. OlderKBH  database versions won't be supported with 4.2.0+

### Installation of cbpi won't require sudo and can be installed under other user with working autostart

- These changes (in particular removal of sudo requirement) are required for compatibility with newer OS versions such as bookworm.
- An update of older cbpi versions can be still done with the sudo installation as in the past. However, it is recommended to migrate to bookworm os.
- Migration to bookworm needs to start from scratch with an empty SD card as migration from bullseye to bookworm via dist upgrade is not recommended and most likely not working.
- For installation under bookworm, you need to use pipx instead of pip. This package must be installed first (see new Server Installation instructions).
- To run cbpi you need to run the command `pipx ensurepath` after installation of cbpi. Close the terminal and open it again. Then the cbpi command will be working.
- Pipx will create a virtual environment for cbpi4 and you need to install all plugins inside this virtual environment (see new Plugin installation instructions). The virtual environment is only required for plugin installation.
- cbpi onewire | setup | autostart and chromium commands must be carried out in your normal bash and not in the virtual environment. sudo is not required anymore and won't be working. 
- You should also be able to install cbpi under a different user than pi and have it starting with autostart. The file craftbeerpi.service in the config folder is replaced by a flexible craftbeerpi.template file that is adapted to the user you are logged into your terminal session. cbpi setup must be carried out in your home folder as in older versions (typically the folder you are in, when you open a terminal session).
- Backup your config on your existing system -> Install bookworm aon an empty sd card -> follow the server and plugin installation instructions. Restore your config file. You need to activate onewire, I2C, autostart,... on your new system. Although I have successfully tested bookworm incl. installation and everything on my productive system,  KEEP your SD card with the old system, until you have completed your first batch on the new system.

### Debugging: added `debug-log-level` parameter to config.yaml

- If not in config.yaml, default log level is 30 (warnings)
- You can add / modify the level in config.yaml and log level will be also adapted to server running in automode

### Dashboard: added hidden `gridwidth` parameter to settings

- will be used with upcoming UI to change the dashboard grid settings in edit mode on the fly

### Bookworm compatibility

- This cbpi version is compatible with bookworm os if installed with pipx (tested).
- Bookworm is now using wayland instead of X11 window manager as default. I have some issues with my touchscreen in combination with wayland but also the standard VNC did not work properly. Therefore I switched to X11 which is possible via raspi-config. You will find the option to switch under Advanced Options.
- Chromium Kiosk mode is working under X11. I have not tested this under wayland window manager.

## Server Version 4.1.10 has the following changes:

### Individual Data logging can be now done via plugin (provided by prash3r)

- In case a developer wants to log data to different databases or files, data can be logged via plugin.
- As an example you can review the [influxdb logger](https://github.com/PiBrewing/craftbeerpi4/commit/e01850f2dc8fac02acd60685cceb071157dcf3ae#diff-107f71bd92549585a518ca8d6b3f6eb086d22d83614c75b4e3e3ac04afbaf38d)

### Adding global settings for a plugin requires now a source parameter

!!! Plugins with global settings (e.g. cpbi4-buzzer, cbpi4-system, cbpi4-Pushover, cbpi4-scd30-co2-sensor,.....) need to be updated to the latest versions !!!

- To get a better overview on the settings page, plugins that add global settings parameters need to use a 'source' parameter.
- With UI version 0.3.12, the settings page has a drop down menu at the top to select also settings for individual plugins.
- An example can be seen [here](https://github.com/PiBrewing/cbpi4-PushOver/commit/6ee61a35ae4d225737764d251e3cea074ef0d646#diff-73e2a5d205487d09c209cfcbcbc3f7a56568faa301b03209369859548932d00b)
- This example also shows  how to update parameters in case you want to change for example the description. Therefore, you can use also the source key 'hidden' and write the plugin version to the settings.
- Hidden parameters won't be shown on the settings page.
- Added function to remove global config parameters.

## UI Version Version 0.3.12 has the following changes:

- Added selection option to settings page for global plugin settings, Option to remove obsolete global settings on system page.
- Added various tooltips and fixed issue if fermenter recipe contained special characters (-> will be automatically replaced). 
- Option on analytics page to delete all logs


## Server Version 4.1.7 has the following changes:

### Cryptography update may cause error 'X509_V_FLAG_CB_ISSUER_CHECK' or 'Illegal instruction'

For security reasons the cryptography package had to be updated to 40.0.0. This may cause an issue later when you are using pip. To fix it, please have a look [here](./cryptography_update.md) 

In case of the illegal instruction issue (armv6 based 32 bit systems), you can try to adapt the setup.py manually. However, there won't be any further support with older versions of cryptography due to security issues with older versions of this dependency.

### INFLUXDB configuration
- INFLUXDBADDR address must contain the full address incl. http(s) and port
- INFLUXDBPORT becomes obsolete
    - old: INFLUXDB: localhost | INFLUXDBPORT: 8086
    -> new: INFLUXDBADDR: http://localhost:8086
- INFLUXDBNAME: 1.8 => database name | 2.X or cloud: bucket name
- INFLUXDBPWD: 1.8 => password if required | 2.X or cloud: token
- INFLUXDBUSER: 1.8 => user if required | 2.X or cloud: organization

### Buzzer Tone via Browser
- PLAY_BUZZER  parameter has been added to global settings
    - Yes will enable buzzer tone via web interface (UI >= 0.3.11 required)

### Reduced sensor logging (integrated http, mqtt and onewire sensors)
- The sensor plugins that come with cbpi have an additional functionality with respect to logging.
- The user can assign a Kettle <strong>OR</strong> a Fermenter to the sensor
- The reduced logging frequency defines the reduced frequency of sensor logging. Readings will be still handled with regular frequency, but logging to CSV or influxdb is reduced. A value of 0 disables logging completely on inactivity.
- Reduced logging will be only enabled if a Kettle or Fermenter is specified for this sensor and if the Kettle or Fermenter is inactive (not running in auto mode).
- For the onewire sensor the reduced logging value must be larger than the regular logging/reading frequency.
- Example: Onewire sensor is reading data every second and you have set your brew kettle as Kettle for this sensor, you have further set reduced logging to 300. the sensor will log a value every second, while you are brewing in auto mode, but sends values to the log only every 300 seconds, when the kettle is inactive. If the reduced logging value is set to 0, the logging will only take place when brewing, but it is completely disabled when the kettle is inactive.

## UI Version Version 0.3.11 has the following changes:

### Buzzer Tone via Browser
- Buzzer signal with server >=4.1.7

### Grafana Widget requires change in UI property settings for refresh
- old: 10 | new: 10s or 1m (time unit required)
- Only data will be updated and not the entire iframe

## Changelog

- To be created