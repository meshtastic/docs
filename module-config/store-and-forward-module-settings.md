# Store & Forward Module Settings

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

:::info Currently only available for ESP32 based devices with external PSRAM like the tbeam. Requires the device to be set as a ROUTER or ROUTER\_CLIENT. :::

### Overview

:::caution This is a work in progress and the required client support is not yet available. :::

The Store & Forward Module is an implementation of a Store and Forward system to enable resilient messaging in the event that a client device is disconnected from the main network.

Because of the increased network traffic for this overhead, it's not advised to use this if you are duty cycle limited for your airtime usage (EU\_868 and EU\_433) nor is it advised to use this for presets using SF11 or SF12 (e.g. all of the LongRange and VeryLongRange presets).

### Details

#### How it works

#### Requirements

Initial Requirements:

* Must be installed on a ROUTER or ROUTER\_CLIENT node.
  * This is an artificial limitation, but is in place to enforce best practices.
  * Router nodes are intended to be always online. If this module misses any messages, the reliability of the stored messages will be reduced.
* ESP32 Processor based device with external PSRAM. (tbeam > v1.0, T3S3, and maybe others)

#### Usage Overview

* To use / test this you will want at least 3 devices
  * One ESP32 device with PSRAM configured as a Meshtastic router.
  * Two others will be regular clients. Nothing special required.

#### Meshtastic channel configuration

Don't use this on the "LongRange" channel settings. You're welcome to try and report back, but those channels have a low bitrate.

Either use a custom channel configuration with at an at least 1kbit data rate or use a Medium or Short range preset.

#### Router setup

* Configure your device as a router.
* Name your router node something that makes it easily identifiable, aka "Router".
*   Configure the Store and Forward module

    ```shell
    meshtastic --set store_forward.enabled true
    ```

    ```shell
    meshtastic --set store_forward.records 100
    ```

    :::tip Best to leave `store_forward.records` at the default (`0`) where the module will use 2/3 of your device's available PSRAM. This is about 11,000 records. :::

#### Client Usage

Currently there are no clients that support store and forward.

### Settings

#### Enabled

Enables the module.

#### Heartbeat

The Store & Forward Router sends a periodic message onto the network. This allows connected devices to know that a router is in range and listening to received messages. A client like Android, iOS, or Web can (if supported) indicate to the user whether a store and forward router is available.

#### History Return Max

Sets the maximum number of messages to return to a client device.

#### History Return Window

Limits the time period (in minutes) a client device can request.

#### Records

Set this to the maximum number of records to save. Best to leave this at the default (`0`) where the module will use 2/3 of your device's available PSRAM. This is about 11,000 records.

#### Client Config

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info Store and Forward Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Store & Forward** :::

:::info Store and Forward configuration is not currently available via the Apple clients. :::

|                 Setting                | Acceptable Values | Default |
| :------------------------------------: | :---------------: | :-----: |
|         store\_forward.enabled         |  `true`, `false`  | `false` |
|        store\_forward.heartbeat        |  `true`, `false`  | `false` |
|   store\_forward.history\_return\_max  |     `integer`     |   `0`   |
| store\_forward.history\_return\_window |     `integer`     |   `0`   |
|         store\_forward.records         |     `integer`     |   `0`   |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set store_forward.enabled true --set store_forward.history_return_max 0
```

:::

#### Examples of CLI Usage

```shell
meshtastic --set store_forward.enabled true
```

```shell
meshtastic --set store_forward.enabled false
```

```shell
meshtastic --set store_forward.heartbeat 0
```

```shell
meshtastic --set store_forward.history_return_max 0
```

```shell
meshtastic --set store_forward.history_return_max 100
```

```shell
meshtastic --set store_forward.history_return_window 0
```

```shell
meshtastic --set store_forward.history_return_window 1440
```

```shell
meshtastic --set store_forward.records 0
```

```shell
meshtastic --set store_forward.records 100
```

:::info Store and Forward configuration is not currently available via the web client. :::
