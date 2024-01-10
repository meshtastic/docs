# Device Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The device config options are: Role, Serial Output, and Debug Log. Device config uses an admin message sending a `Config.Device` protobuf.

### Device Config Values

#### Role

Sets the role of the node.

Acceptable values:

|       Value      |                                                                                                                                                                                                                                                                                 Description                                                                                                                                                                                                                                                                                |
| :--------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     `CLIENT`     |                                                                                                                                                                                                 Client (default) - This role will follow the standard routing rules while also allowing the device to interact with client applications via BLE/Wi-Fi (Android/Apple/Web).                                                                                                                                                                                                 |
|   `CLIENT_MUTE`  |                                                                                                                                                                                                                         Client Mute - Same as a client except packets will not hop over this node, does not contribute to routing packets for mesh.                                                                                                                                                                                                                        |
|     `ROUTER`     |                                                           Router - Mesh packets will prefer to be routed over this node. The assumption is that Router-type devices will be placed in locations with a height/range/antenna advantage, and therefore have better overall coverage. This node will not be used by client apps. The BLE/Wi-Fi radios and the OLED screen will be put to sleep. Please note: Due to the preferred routing, this role may cause higher power usage due to more frequent transmission.                                                          |
|  `ROUTER_CLIENT` |                                                                                                                                                                              Router Client - Hybrid of the Client and Router roles. Similar to Router, except the Router Client can be used as both a Router and an app connected Client. BLE/Wi-Fi and OLED screen will not be put to sleep.                                                                                                                                                                              |
|    `REPEATER`    | Repeater - Mesh packets will prefer to be routed over this node. This role eliminates unnecessary overhead such as NodeInfo, DeviceTelemetry, and any other mesh packet, resulting in the device not appearing as part of the network. As such, direct messaging this node is not available, as it will not appear in your nodes list which results in a cleaner mesh network. Channel and modem settings of the mesh packets being repeated must be identical to the repeater's configuration. Please see Rebroadcast Mode for additional settings specific to this role. |
|     `TRACKER`    |                                                                                                    Tracker - For use with devices intended as a GPS tracker. Position packets sent from this device will be higher priority. Smart Position Broadcast will default to the currently configured settings. When used in conjunction with power.is\_power\_saving = true, nodes will wake up, send position, and then sleep for position.position\_broadcast\_secs seconds.                                                                                                   |
|     `SENSOR`     |                                                                                                      Sensor - For use with devices intended to primarily collect sensor readings. Telemetry packets sent from this device will be higher priority, broadcasting every five minutes. When used in conjunction with power.is\_power\_saving = true, nodes will wake up, send environment telemetry, and then sleep for telemetry.environment\_update\_interval seconds.                                                                                                      |
|       `TAK`      |                                                                                                                                                                                         TAK - Used for nodes dedicated for connection to an ATAK EUD. Turns off many of the routine broadcasts to favor CoT packet stream from the Meshtastic ATAK plugin -> IMeshService -> Node.                                                                                                                                                                                         |
|  `CLIENT_HIDDEN` |                                                                                                                           Client Hidden - Used for nodes that "only speak when spoken to." Turns off all of the routine broadcasts but allows for ad-hoc communication. Still rebroadcasts, but with local only rebroadcast mode (known meshes only). Can be used for clandestine operation or to dramatically reduce airtime / power consumption                                                                                                                          |
| `LOST_AND_FOUND` |                                                                                                                                                                                                   Lost and Found - Used to automatically send a text message to the mesh with the current position of the device on a frequent interval: "I'm lost! Position: lat / long"                                                                                                                                                                                                  |

#### Rebroadcast Mode

This setting defines the device's behavior for how messages are rebroadcasted.

|        Value        |                                                                                        Description                                                                                       |
| :-----------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|        `ALL`        |     ALL (Default) - This setting will rebroadcast ALL messages from its primary mesh as well as other meshes with the same modem settings, including when encryption settings differ.    |
| `ALL_SKIP_DECODING` |                       ALL\_SKIP\_DECODING - Same as behavior as ALL, but skips packet decoding and simply rebroadcasts them. **Only available with Repeater role.**                      |
|     `LOCAL_ONLY`    |  LOCAL\_ONLY - Ignores observed messages from foreign meshes that are open or those which it cannot decrypt. Only rebroadcasts message on the nodes local primary / secondary channels.  |
|     `KNOWN_ONLY`    | KNOWN\_ONLY - Ignores observed messages from foreign meshes like LOCAL\_ONLY, but takes it a step further by also ignoring messages from nodenums not in the node's known list (NodeDB). |

#### Serial Console

Acceptable values: `true` or `false`

Disabling this will disable the SerialConsole by not initializing the StreamAPI.

#### Debug Log

Acceptable values: `true` or `false`

By default we turn off logging as soon as an API client connects. Set this to true to leave the debug log outputting even when API is active.

#### GPIO for user button

This is the GPIO pin number that will be used for the user button, if your device does not come with a predefined user button.

#### GPIO for PWM Buzzer

This is the GPIO pin number that will be used for the PWM buzzer, if your device does not come with a predefined buzzer.

#### Node Info Broadcast Seconds

This is the number of seconds between NodeInfo message broadcasts from the device. The device will still respond ad-hoc to NodeInfo messages when a response is wanted.

#### Double Tap as Button Press

This option will enable a double tap, when a supported accelerometer is attached to the device, to be treated as a button press.

#### Managed Mode

Enabling Managed mode will restrict access to all radio configurations via client applications. Radio configurations will only be accessible through the Admin channel. To avoid being locked out, make sure the Admin channel is working properly before enabling it.

### Device Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Device Config is available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Device**

:::

:::info All device config options other than NTP Server are available on iOS, iPadOS and macOS at Settings > Radio Configuration > Device. :::

:::info

All device config options are available in the python CLI. Example commands are below:

:::

| Setting                               | Acceptable Values                                                                   | Default           |
| ------------------------------------- | ----------------------------------------------------------------------------------- | ----------------- |
| device.debug\_log\_enabled            | `true`, `false`                                                                     | `false`           |
| device.role                           | `CLIENT`, `CLIENT_MUTE`, `ROUTER`, `ROUTER_CLIENT`, `REPEATER`, `TRACKER`, `SENSOR` | `CLIENT`          |
| device.rebroadcast\_mode              | `ALL`, `ALL_SKIP_DECODING`, `LOCAL_ONLY`                                            | `ALL`             |
| device.serial\_enabled                | `true`, `false`                                                                     | `true`            |
| device.button\_gpio                   | `0` - `34`                                                                          | `0`               |
| device.buzzer\_gpio                   | `0` - `34`                                                                          | `0`               |
| device.node\_info\_broadcast\_secs    | `0` - `UINT MAX`                                                                    | `10800` (3 hours) |
| device.double\_tap\_as\_button\_press | `false`, `true`                                                                     | `false`           |
| device.is\_managed                    | `false`, `true`                                                                     | `false`           |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set device.role CLIENT --set device.debug_log_enabled true
```

:::

```shell
meshtastic --set device.role CLIENT
```

```shell
meshtastic --set device.serial_enabled false
```

```shell
meshtastic --set device.debug_log_enabled true
```

:::info All device config options are available in the Web UI. :::
