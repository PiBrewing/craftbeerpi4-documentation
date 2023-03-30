# Changes

## Server Version Version 4.1.7 has the followning changes:

### INFLUXDB configuration
- INFLUXDBADDR address must contain the full address inlc. http(s) and port
- INFLUXDBPORT becomes obsolete
    - old: INFLUXDB: localhost | INFLUXDBPORT: 8086
    -> new: INFLUXDBADDR: http://localhost:8086
- INFLUXDBNAME: 1.8 => database name | 2.X or cloud: bucket name
- INFLUXDBPWD: 1.8 => password if required | 2.X or cloud: token
- INFLUXDBUSER: 1.8 => user if required | 2.X or cloud: organization

### Buzzer Tone via Browser
- PLAY_BUZZER  paramter has been added to global settings
    - Yes will enable buzzer tone via web interface (UI >= 0.3.11 required)


## UI Version Version 0.3.11 has the following changes:

### Buzzer Tone via Browser
- Buzzer signal with server >=4.1.7

### Grafana Widget requires change in UI propertie settings for refresh
- old: 10 | new: 10s or 1m (time unit required)
- Only date will be updated and not the entire iframe

## Changelog

- To be created