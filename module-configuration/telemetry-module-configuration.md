# Telemetry Module Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Telemetry Module provides three types of data over the mesh: Device metrics (Battery Level, Voltage, Channel Utilization and Airtime) from your Meshtastic device, Environment Metrics from attached I2C sensors, and Air Quality Metrics from attached I2C particle sensors.

Supported sensors connected to the I2C bus of the device will be automatically detected at startup. Environment Telemetry and Air Quality must be enabled for them to be instrumented and their readings sent over the mesh.

#### Currently Supported Sensor Types

|  Sensor  | I2C Address |                          Data Points                          |
| :------: | :---------: | :-----------------------------------------------------------: |
|  BMP280  |  0x76, 0x77 |              Temperature and barometric pressure              |
|  BME280  |  0x76, 0x77 |         Temperature, barometric pressure and humidity         |
|  BME680  |  0x76, 0x77 | Temperature, barometric pressure, humidity and air resistance |
|  MCP9808 |     0x18    |                          Temperature                          |
|  INA260  |  0x40, 0x41 |                      Current and Voltage                      |
|  INA219  |  0x40, 0x41 |                      Current and Voltage                      |
|   LPS22  |  0x5D, 0x5c |                      Barometric pressure                      |
|   SHTC3  |     0x70    |                    Temperature and humidity                   |
|   SHT31  |     0x44    |                    Temperature and humidity                   |
| PMSA003I |     0x12    |    Concentration units by size and particle counts by size    |

### Module Config Values

#### Environment Telemetry Enabled

Enable the Environment Telemetry (Sensors).

#### Environment Metrics Update Interval

How often we should send Environment(Sensor) Metrics over the mesh.

Default is `900` seconds (15 minutes).

#### Device Metrics Update Interval

How often we should send Device Metrics over the mesh.

Default is `900` seconds (15 minutes).

#### Environment Screen Enabled

Show the environment telemetry data on the device display.

Default is `false`.

#### Display Fahrenheit

The sensor is always read in Celsius, but the user can opt to display in Fahrenheit (on the device display only) using this setting.

Default is `false`.

#### Air Quality Enabled

This option is used to enable/disable the sending of air quality metrics from an attached supported sensor over the mesh network.

Default is `false`.

#### Air Quality Interval

This option is used to configure the interval (in seconds) that should be used to send air quality metrics from an attached supported sensor over the mesh network.

Default is `900` seconds (15 minutes).

### Telemetry Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Telemetry Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Telemetry**

:::

:::info All telemetry module config options are available on iOS, iPadOS and macOS at Settings > Module Configuration > Telemetry.

:::

:::info

All telemetry module config options are available in the python CLI. Example commands are below:

:::

### Settings

|                   Setting                   |  Acceptable Values  |                  Default                  |
| :-----------------------------------------: | :-----------------: | :---------------------------------------: |
|      telemetry.device\_update\_interval     | `integer` (seconds) | Default `0` is 15 minutes(`900` seconds). |
|  telemetry.environment\_display\_fahrenheit |   `true`, `false`   |                  `false`                  |
| telemetry.environment\_measurement\_enabled |   `true`, `false`   |                  `false`                  |
|    telemetry.environment\_screen\_enabled   |   `true`, `false`   |                  `false`                  |
|   telemetry.environment\_update\_interval   | `integer` (seconds) | Default `0` is 15 minutes(`900` seconds). |
|       telemetry.air\_quality\_enabled       |   `true`, `false`   |                  `false`                  |
|       telemetry.air\_quality\_interval      | `integer` (seconds) | Default `0` is 15 minutes(`900` seconds). |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set telemetry.device_update_interval 0 --set telemetry.environment_update_interval 0
```

:::

```shell
meshtastic --set telemetry.device_update_interval 0
// Device Metrics Two Minutes
meshtastic --set telemetry.device_update_interval 0
// Environment Metrics Two Minutes
meshtastic --set telemetry.environment_update_interval 120
```

```shell
meshtastic --set telemetry.environment_measurement_enabled true
meshtastic --set telemetry.environment_measurement_enabled false
```

```shell
meshtastic --set telemetry.environment_screen_enabled true
meshtastic --set telemetry.environment_screen_enabled false
```

```shell
meshtastic --set telemetry.environment_display_fahrenheit true
meshtastic --set telemetry.environment_display_fahrenheit false
```

:::info

All telemetry module config options are available in the Web UI.

:::

### Examples

#### RAK 4631 with BME680 Environment Sensor

Setup of a RAK 4631 with Environment Sensor

\<img src="RAK4631\_with\_EnvSensor" src="/img/hardware/rak/RAK4631\_with\_EnvSensor.jpg" style=\{{zoom:'25%'\}} />

Requirements:

* RAK4631
* Environment Sensor

Steps:

* configure the device:

```shell
meshtastic --set telemetry.environment_measurement_enabled true --set telemetry.environment_screen_enabled true --set telemetry.environment_display_fahrenheit true
```

:::tip

While the above values serve as an example and can be modified to fit your specific needs, it is advisable to chain multiple commands together, as demonstrated in the example. This approach will minimize the number of necessary reboots.

:::

* Device will reboot after command is sent.
* When the device boots again it should say "Telemetry" and it may show the sensor data
* If this does not appear to have any effects, run:

```shell
meshtastic --noproto
```

And examine the serial logs for Telemetry diagnostic information.

### Supporting Additional Sensors

#### Environment Metrics

The environment metrics in the telemetry module supports a limited amount of fields as they are stored in memory on the device. Support for sensors that provide one or more of the following fields can potentially be added to the main firmware provided there is a GPL licensed library for us to include to support it, and the library size is not prohibitive.

* Temperature
* Relative Humidity
* Barometric Pressure
* Gas Resistance (AQI)
* Voltage
* Current

#### Supporting Other Sensor types

For other interesting sensor types and use cases we need to add a portnum for more generic telemetry packets and a second MCU will be required to interact with the sensor and process the data to be sent over the mesh. This data will not be stored in the nodedb on the device.
