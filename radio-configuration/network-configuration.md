# Network Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Network config options are: NTP Server, WiFi Enabled, WiFi SSID, WiFi PSK, Ethernet Enabled, IPv4 Networking Mode, and Static Address. Network config uses an admin message sending a `Config.Network` protobuf.

:::info Enabling WiFi will disable Bluetooth. Only one connection method will work at a time. :::

ESP32 devices have the ability to connect to WiFi as a client. SoftAP mode is not supported by the Meshtastic firmware.

### Network Config Values

#### NTP Server

The NTP server used if IP networking is available.

Set to `0.pool.ntp.org` by default. (Max Length: 32)

#### WiFi Enabled

Enables or Disables WiFi.

Set to `false` (Disabled) by default.

#### WiFi SSID

This is your WiFi Network's SSID.

Empty `""` by default. (Case Sensitive, Max Length: 32)

#### WiFi PSK

This is your WiFi Network's password.

Empty `""` by default. (Case Sensitive, Max Length: 64)

#### Ethernet Enabled

Enables or Disables Ethernet.

Set to `false` (Disabled) by default.

#### IPv4 Networking Mode

Set to `DHCP` by default. Change to `STATIC` to use a static IP address. Applies to both Ethernet and WiFi.

#### IPv4 Static Address configuration

contains ip, gateway, subnet and dns server in case you want a static configuration.

:::tip The first time your device restarts after enabling WiFi or Ethernet, it will take an additional 20-30 seconds to boot. This is to generate self-signed SSL keys. The keys will be saved for future reuse. :::

### Network Config Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Network Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Network**

:::

:::info

Network config options are available on iOS, iPadOS and macOS.

:::

:::info

All Network config options are available in the python CLI.

:::

|        Setting        | Acceptable Values |      Default     |
| :-------------------: | :---------------: | :--------------: |
|  network.ntp\_server  |       string      | `0.pool.ntp.org` |
| network.wifi\_enabled |  `true`, `false`  |      `false`     |
|   network.wifi\_ssid  |       string      |       `""`       |
|   network.wifi\_psk   |       string      |       `""`       |
|  network.eth\_enabled |  `true`, `false`  |      `false`     |
| network.address\_mode |  `DHCP`, `STATIC` |      `DHCP`      |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set network.wifi_enabled true --set network.wifi_ssid "my network" --set network.wifi_psk mypassword
```

:::

```shell
meshtastic --set network.ntp_server "0.pool.ntp.org"
```

```shell
meshtastic --set network.wifi_enabled true
meshtastic --set network.wifi_enabled false
```

```shell

meshtastic --set network.wifi_ssid mynetwork
// With spaces
meshtastic --set network.wifi_ssid "my network"
```

```shell
meshtastic --set network.wifi_psk mypassword
// With spaces
meshtastic --set network.wifi_psk "my password"
```

:::info All Network config options are available in the Web UI. :::

### Examples

#### WiFi Client

With `network.wifi_ssid` & `network.wifi_psk` populated, the device will know to connect to your network. Make sure you are in range of your WiFi. If you have a single Meshtastic device on your local network it's easy to connect to your device with DNS `http://meshtastic.local`. If you have multiple Meshtastic devices you will need to connect using their respective IP addresses.

#### Disable WiFi

To disable WiFi completely, set `network.wifi_enabled` to `false`.
