# Changes

## Server Version Version 4.1.7 has the following changes:

### INFLUXDB configuration
- INFLUXDBADDR address must contain the full address inlc. http(s) and port
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
- The reduced logging frequency defineces the reduced frequency of sensor logging. Readings will be still handled with regular frequency, but logging to CSV or influxdb is reduced. A value of 0 disables logging completely on inactivity.
- Reduced logging will be only enabled if a Kettle or Fermenter is specified for this sensor and if the Kettle or Fermenter is inactive (not running in automode).
- For the onewire sensor the reduced logging value must be larger than the regular logging/reading frequency.
- Example: Onewire sensor is reading data every second and you have set your brewkettle as Kettle for this sensor, you have further set reduced logging to 300. the sensor will log a value every second, while you are brewing in automode, but sends values to the log only every 300 seconds, when the kettle is inactive. If the reduced logging value is set to 0, the logging will only take plöace when brewing, but it is completely disabled when the kettle is inactive.

## UI Version Version 0.3.11 has the following changes:

### Buzzer Tone via Browser
- Buzzer signal with server >=4.1.7

### Grafana Widget requires change in UI propertie settings for refresh
- old: 10 | new: 10s or 1m (time unit required)
- Only data will be updated and not the entire iframe

## Changelog

- To be created