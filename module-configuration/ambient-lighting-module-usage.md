# Ambient Lighting Module Usage

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Ambient Lighting Module has settings for control of onboard LEDs and allows users to adjust the brightness levels and respective color levels. Initially created for the RAK14001 RGB LED module using the NCP5623 with I2C. Config options are: LED State, Current, Red Level, Green Level, and Blue Level.

In order to use this module, make sure your devices have firmware version 2.2.5 or higher.

### Ambient Lighting Config Values

#### LED State

Sets the LED to on or Off

#### Current

Sets the current for the LED output. Default is 10.

#### Red

Sets the red LED level. Values are 0-255.

#### Green

Sets the green LED level. Values are 0-255.

#### Blue

Sets the blue LED level. Values are 0-255.

### Ambient Lighting Module Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info All Ambient Lighting Module config options are available for Android in app version 2.2.3 and higher.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Ambient Lighting** :::

:::info All Ambient Lighting Module config options are available on iOS, iPadOS and macOS app versions 2.2.3 and higher at Settings > Modules > Ambient Lighting :::

:::info

All Ambient Lighting Module config options are available in the python CLI version 2.2.3 and higher.

:::

Example commands are below:

```shell
meshtastic --set ambient_lighting.led_state 1
meshtastic --set ambient_lighting.led_state 0
```

```shell
meshtastic --set ambient_lighting.current 5
```

```shell
meshtastic --set ambient_lighting.red 103
```

```shell
meshtastic --set ambient_lighting.green 234
```

```shell
meshtastic --set ambient_lighting.blue 148
```

```shell
meshtastic --get ambient_lighting
```

:::info

All Ambient Lighting module config options are available in the Web UI.

:::
