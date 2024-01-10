# MQTT Module Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

If your device is connected to Internet via wifi or ethernet, you can enable it to forward packets along to an MQTT server. This allows users on the local mesh to communicate with users on the internet. One or more channels must also be enabled as uplink and/or downlink for packets to be transmitted from and/or to your mesh (See channels). Without these settings enabled, the node will still connect to the MQTT server but only send status messages.

The MQTT module config options are: Enabled, Server Address, Username, Password, Encryption Enabled, JSON Enabled, TLS Enabled, and Root Topic. MQTT Module config uses an admin message sending a `ConfigModule.MQTT` protobuf.

### Settings

### MQTT Module Config Values

#### Enabled

Enables the MQTT module.

#### Server Address

The server to use for MQTT. If not set, the default public server will be used.

#### Username

MQTT Server username to use (most useful for a custom MQTT server). If using a custom server, this will be honored even if empty. If using the default public server, this will only be honored if set, otherwise the device will use the default username.

#### Password

MQTT password to use (most useful for a custom MQTT server). If using a custom server, this will be honored even if empty. If using the default server, this will only be honored if set, otherwise the device will use the default password.

#### Encryption Enabled

Whether to send encrypted or unencrypted packets to MQTT. This parameter is only honored if you also set server (the default official mqtt.meshtastic.org server can handle encrypted packets). Unencrypted packets may be useful for external systems that want to consume meshtastic packets.

#### JSON Enabled

Enable the sending / consumption of JSON packets on MQTT. These packets are not encrypted, but offer an easy way to integrate with systems that can read JSON.

#### TLS Enabled

If true, we attempt to establish a secure connection using TLS.

#### Root Topic

The root topic to use for MQTT messages. This is useful if you want to use a single MQTT server for multiple meshtastic networks and separate them via ACLs.

### MQTT Module Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

MQTT Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > MQTT**

:::

:::info

MQTT Config options are available on iOS, iPadOS and macOS at Settings > Modules > MQTT.

:::

:::info

All MQTT module config options are available in the python CLI. Example commands are below:

:::

|          Setting         | Acceptable Values |        Default        |
| :----------------------: | :---------------: | :-------------------: |
|       mqtt.enabled       |  `true`, `false`  |        `false`        |
|       mqtt.address       |      `string`     | `mqtt.meshtastic.org` |
|       mqtt.username      |      `string`     |       `meshdev`       |
|       mqtt.password      |      `string`     |      `large4cats`     |
| mqtt.encryption\_enabled |  `true`, `false`  |        `false`        |
|    mqtt.json\_enabled    |  `true`, `false`  |        `false`        |
|     mqtt.tls\_enabled    |  `true`, `false`  |        `false`        |
|         mqtt.root        |      `string`     |                       |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set mqtt.enabled true --set mqtt.json_enabled true
```

:::

```shell
meshtastic --set mqtt.enabled true
meshtastic --set mqtt.enabled false
```

```shell
meshtastic --set mqtt.json_enabled true
meshtastic --set mqtt.json_enabled false
```

:::info All MQTT module config options are available for the Web UI. :::

### Connect to the Default Public Server

:::important The default channel (LongFast) on the public server usually has a lot of traffic. Your device may get overloaded and may no longer function properly anymore. It is recommended to use a different channel or to use your own MQTT server if you experience issues. :::

\<Tabs defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

#### 1. Enable the MQTT Module

Navigate to: Vertical Ellipsis (3 dots top right) > Radio configuration > MQTT: Turn on the slider for **MQTT enabled** and tap **Send**.



_Optional:_ To use your phone's internet connection to send and receive packets over the web, also enable the slider for **MQTT Client Proxy** and skip the Configure Network Settings step below.



#### 2. Enable Channel Uplink & Downlink

Navigate to: Vertical Ellipsis (3 dots top right) > Radio configuration > Channels > LongFast: Turn on the sliders for **Uplink enabled** and **Downlink enabled**, then tap **Save** and tap **Send**.



#### 3. Configure Network Settings

Navigate to: Vertical Ellipsis (3 dots top right) > Radio configuration > Network: Turn on the slider for **WiFi enabled**, Enter the **SSID** and **PSK** for your network, then tap **Send**.



#### 1. Enable the MQTT Module

Navigate to Settings > MQTT: Turn on the slider for MQTT enabled and tap **Save**

&#x20;

_Optional:_ To use your phone's internet connection to send and receive packets over the web, also enable the slider for **MQTT Client Proxy** and skip the Configure Network Settings step below.



#### 2. Enable Channel Uplink & Downlink

Navigate to Settings > Channels > Primary Channel: Turn on the sliders for **Uplink enabled** and **Downlink enabled** - Tap **Save**



#### 3. Configure Network Settings

Navigate to Settings > Network: Turn on the slider for **WiFi enabled** - Enter your **SSID** and **PSK** for your network - Tap **Save**



#### 1. Enable the MQTT Module

```shell
meshtastic --set mqtt.enabled true
```

#### 2. Enable Channel Uplink & Downlink

```shell
meshtastic --ch-set uplink_enabled true --ch-index 0
meshtastic --ch-set downlink_enabled true --ch-index 0
```

or chained together:

```shell
meshtastic --ch-set uplink_enabled true --ch-index 0 --ch-set downlink_enabled true --ch-index 0
```

#### 3. Configure Network Settings

```shell
meshtastic --set network.wifi_enabled true
meshtastic --set network.wifi_ssid "your network"
meshtastic --set network.wifi_psk yourpassword
```

or chained together:

```shell
meshtastic --set network.wifi_enabled true --set network.wifi_ssid "your network" --set network.wifi_psk yourpassword
```

#### 1. Enable the MQTT Module

Navigate to Config > Module Config > MQTT - Turn on the slider for MQTT enabled - Click the **Save** icon.



_Optional:_ To use your client's internet connection to send and receive packets over the web, also enable the slider for **Proxy to Client Enabled** and skip the Configure Network Settings step below.



:::caution

Though this option may be visible in your UI, Client Proxy is not yet functional with the Web Client.

:::

#### 2. Enable Channel Uplink & Downlink

Navigate to Channels > Primary: Turn on the sliders for **Uplink Enabled** and **Downlink Enabled** - Click the **Save** icon.



#### 3. Configure Network Settings

Navigate to Radio Config > Device > Network: Turn on the slider for **Enabled** - Enter your **SSID** and **PSK** for your network - Click the **Save** icon.

