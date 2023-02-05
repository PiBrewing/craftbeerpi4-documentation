# Sensor data logging (CSV or InfluxDB)

The data retrieved by sensors can be logged in different ways. The default is csv logging. CSV files will be stored for each sensor and the log is rotating. This means, that up to 4 csv files with 1 Mb per file can be created per sensor. Some sensors write one datapoint per second. If you have variuos sensors, plenty of write cycle will be done per second which is not good for the health of an SD card. In addition to that dashboard charts will read from those files which may drive up the CPU usage to 100% and may cause an unresponsive Interface in particular, when you are using a screen that is directly connetec to your pi.

Therefore, it is not recommended to use charts in your brewing or femrentation dashboard. I recommend to use the analytics page instead.

To reduce the write cycles to the sd card, you could symlink the log folder also to an external hdd or even a network folder.

If you are not using the log files and charts at all, it is recommended to switch of csv logging, which can be done on the [settings page](settings.md#global-system-parameters) via the parameter CSVLOGFILES. Set this parameter to 'No'.

CraftbeerPi4 has also the possibility to forward the sensor data to an InfluxDB database. This method is way more sophisticated and recommended as it allows the user to use and display the data for instance with grafana. 

To activate this functionality, you need to adapt the [settings](settings.md#global-system-parameters) for InfluxDB (Address, Port, database name,..) and set the parameter INFLUXDB to 'Yes'. Currently, InfluxDB verions up to 1.8.X are supported. Version 2.0 or larger is not yet supported since the authorization is different.

Below is an example for the usage of Influxdb in combination with Grafana. The dashboard shows sensor data for the Kettle and two fermenters. In addition, data from the cbpi4-system plugin that monitors the CPU load, free memory and more is displayed. I am also using a SCD30 sensor to monitor the 'environmental condition' of the room (CO2, temp and rel. humidity) and display this on the same dashboard.

![Example for InfluxDB usage in combination with Grafana](../../.gitbook/assets/cbpi4-grafana.png)

## Grafana Dashboard Chart
As shown in the [dashboard](dashboard.md#item-menu) section, you can also add a grafana chart to the dashboard.

![Example of Grafana Chart in Dashboard](../../.gitbook/assets/cbpi4-grafana-chart.png)

Required Parameters are described briefly in the dashboard section. Below is an example, how to set the 'url' and 'panelID' for your chart.

![Grafana URL Example](../../.gitbook/assets/cbpi4-grafana-url-example.png)

The url for this chart on Grafana is: ```http://192.168.163.105:3000/d/m5Lx6uYnz/craftbeerpi-4?orgId=1&viewPanel=7```

You need to use the first part of the url prior to the `?` and replace the `d`with `d-solo` and enter this into the url parameter:

```url: http://192.168.163.105:3000/d-solo/m5Lx6uYnz/craftbeerpi-4```

In the `panelID` Parameter you need to enter just the number of the panel from the original url: `7`

```panelID: 7```

In your grafana server, you need to adapt a few settings.

1. You need to allow [embedding](https://grafana.com/docs/grafana/latest/administration/configuration/#allow_embedding) of your charts.
2. You need to allow [external_access](https://grafana.com/docs/grafana/latest/administration/configuration/#external_enabled).  

{% hint style="info" %}
Depending on your setup on your pi, you may need to adapt also the chromium settings if the charts are displayed on another device, but not on your pi screen.

In this case, you need to allow all third party cookies in chromium. You need to enter: ```chrome://settings/content```

and change the cookie setting accordingly.
{% endhint %}