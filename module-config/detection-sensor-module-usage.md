# Detection Sensor Module Usage

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Detection Sensor module allows you to configure a GPIO pin to be monitored for a specified high/low status and send text alerts over the Detection Sensor portnum when an event is detected. This is particularly useful for motion detection sensors, reed switches, and other open / closed state systems in which notifications over the mesh are desired. Config options are: Enabled, Minimum Broadcast Interval, State Broadcast Interval, Send Bell, Name, Monitor Pin, Detection Triggered High, and Use Pull-up.

In order to use this module, make sure your devices have firmware version 2.2.2 or higher.

### Detection Sensor Module Config Values

#### Enabled

Whether the Module is enabled.

#### Minimum Broadcast Interval

The interval in seconds of how often we can send a message to the mesh when a state change is detected.

#### State Broadcast Interval

The interval in seconds of how often we should send a message to the mesh with the current state regardless of changes, When set to 0, only state changes will be broadcasted, Works as a sort of status heartbeat for peace of mind.

#### Send Bell

Send ASCII bell with alert message. Useful for triggering ext. notification on bell name.

#### Friendly Name

Used to format the message sent to mesh. Example: A name "Motion" would result in a message "Motion detected". Maximum length of 20 characters.

#### Monitor Pin

The GPIO pin to monitor for state changes.

#### Detection Triggered High

Whether or not the GPIO pin state detection is triggered on HIGH (1), otherwise LOW (0).

#### Use Pull-up

Whether or not use INPUT\_PULLUP mode for GPIO pin. Only applicable if the board uses pull-up resistors on the pin.

### Detection Sensor Module Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info All Detection Sensor Module config options are available for Android in app version 2.2.2 and higher.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Detection Sensor** :::

:::info All Detection Sensor Module config options are available on iOS, iPadOS and macOS app versions 2.2.2 and higher at Settings > Modules > Detection Sensor :::

:::info

All Detection Sensor Module config options are available in the python CLI version 2.2.2 and higher.

:::

Example commands are below:

```shell
meshtastic --set detection_sensor.enabled true
meshtastic --set detection_sensor.enabled false
```

```shell
meshtastic --set detection_sensor.minimum_broadcast_secs 90
```

```shell
meshtastic --set detection_sensor.state_broadcast_secs 300
```

```shell
meshtastic --set detection_sensor.send_bell true
meshtastic --set detection_sensor.send_bell false
```

```shell
meshtastic --set detection_sensor.name "motion"
```

```shell
meshtastic --set detection_sensor.monitor_pin 7
```

```shell
meshtastic --set detection_sensor.detection_triggered_high true
```

```shell
meshtastic --set detection_sensor.detection_triggered_high false
```

```shell
meshtastic --set detection_sensor.use_pullup true
```

```shell
meshtastic --get detection_sensor
```

:::info

All Detection Sensor module config options are available in the Web UI.

:::
