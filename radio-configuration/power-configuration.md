# Power Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem"; import calculateADC from "/src/utils/calculateADC";

:::info Power settings are advanced configuration, most users should choose a role under Device Config to manage power for their device and shouldn't ever need to adjust these settings. :::

The power config options are: Power Saving, Shutdown after losing power, ADC Multiplier Override, Wait Bluetooth Interval, Light Sleep Interval, Minimum Wake Interval, and Device Battery INA2xx Address. Power config uses an admin message sending a `Config.Power` protobuf.

:::info ADC Multiplier, The Light Sleep setting only applies to ESP32-based boards. This settings will have no effect on nRF52/RP2040 modules. :::

### Power Config Values

#### Power Saving

If set, Bluetooth, Wifi, and screen (if applicable) are turned off. When using the Router device role, this setting is on by default. The assumption for a Router is that the device will be used in a standalone manner where these features are not needed, and will be shut off to conserve power. This is especially useful when a device is powered from a low-current source (i.e. solar).

#### Shutdown after losing power

Automatically shut down a device after a defined time period if power is lost.

#### ADC Multiplier Override

Ratio of voltage divider for battery pin e.g. 3.20 (R1=100k, R2=220k)

Overrides the ADC\_MULTIPLIER defined in the firmware device variant file for battery voltage calculation.

Should be set to floating point value between 2 and 6

**Calibration Process (**[**Attribution**](https://wiki.uniteng.com/en/meshtastic/nano-g1-explorer#calibration-process)**)**

1. Install the rechargeable Li-Polymer battery.
2. Charge the battery until full. Indication of this state may vary depending on device. At this point, the battery voltage should be 4.2V +-1%.
3. Input the "Battery Charge Percent" displayed on the screen or in your connected app into the calculator below.
4. If "Battery Charge Percent" (e.g., B 3.82V 60%) is not displayed on the screen, it means that the default value of "Operative Adc Multiplier" is too high. Lower the "Operative Adc Multiplier" to a smaller number (it is recommended to decrease by 0.1) until the screen displays "Battery Charge Percent". Enter the current "Operative Adc Multiplier" in use into the "Operative Adc Multiplier" field in the calculator. Also, input the "Battery Charge Percent" displayed on the screen into the calculator.
5. Click the "Calculate" button to compute the "Calculated New Operative Adc Multiplier", and set it as the new "Operative Adc Multiplier" for the device.

:::tip ADC Calculator

| Battery Charge Percent:                  |           |
| ---------------------------------------- | --------- |
| Current Adc Multiplier:                  |           |
| Calculated New Operative Adc Multiplier: |           |
|                                          | Calculate |

:::

:::info It's important to note that this calibration method only maps 4.2V to Battery Charge Percent 100%, and does not address the potential non-linearities of the ADC. :::

#### Wait Bluetooth Interval

How long to wait before turning off BLE in no Bluetooth states

`0` for default of 1 minute

#### Light Sleep Interval

In light sleep the CPU is suspended, LoRa radio is on, BLE is off and GPS is on

`0` for default of five minutes

#### Minimum Wake Interval

While in light sleep when we receive packets on the LoRa radio we will wake and handle them and stay awake in no Bluetooth mode for this interval

`0` for default of 10 seconds

#### Device Battery INA2xx Address

If an INA-2XX device is auto-detected on one of the I2C buses at the specified address, it will be used as the authoritative source for reading device battery level voltage. Setting is ignored for devices with PMUs (e.g. T-beams)

:::tip I2C addresses are normally represented in hexadecimal and will require conversion to decimal in order to set via Meshtastic clients. For example the I2C address of 0x40 converted to decimal is 64. :::

### Power Config Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Power Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Power**

:::

:::info

Power config is not available on Apple OS's. :::

:::info

All Power config options are available in the python CLI.

:::

|                  Setting                 |          Acceptable Values         |               Default               |
| :--------------------------------------: | :--------------------------------: | :---------------------------------: |
|          power.is\_power\_saving         |           `true`, `false`          |               `false`               |
| power.on\_battery\_shutdown\_after\_secs |         `integer` (seconds)        |        Default of `0` is off        |
|      power.adc\_multiplier\_override     |    `2-4` (floating point value)    | Default of `0` uses firmware values |
|        power.wait\_bluetooth\_secs       |         `integer` (seconds)        |      Default of `0` is 1 minute     |
|              power.ls\_secs              |         `integer` (seconds)        |     Default of `0` is 5 minutes     |
|           power.min\_wake\_secs          |         `integer` (seconds)        |     Default of `0` is 10 seconds    |
|    power.device\_battery\_ina\_address   | `integer` (I2C address as decimal) |   Default of `0` is no address set  |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set power.is_power_saving true --set power.on_battery_shutdown_after_secs 120
```

:::

```shell
meshtastic --set power.is_power_saving true
meshtastic --set power.is_power_saving false
```

```shell
meshtastic --set power.on_battery_shutdown_after_secs 120
meshtastic --set power.on_battery_shutdown_after_secs 0
```

```shell
meshtastic --set power.wait_bluetooth_secs 0
meshtastic --set power.wait_bluetooth_secs 120
```

```shell
meshtastic --set power.ls_secs 0
meshtastic --set power.ls_secs 120
```

```shell
meshtastic --set power.min_wake_secs 0
meshtastic --set power.min_wake_secs 120
```

:::info All power config options are available in the Web UI. :::
