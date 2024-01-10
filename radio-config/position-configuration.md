# Position Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The position config options are: GPS Enabled, GPS Update Interval, GPS Attempt Time, Fixed Position, Smart Broadcast, Smart Broadcast Minimum Distance, Smart Broadcast Minimum Interval, Broadcast Interval, Position Packet Flags, and GPS RX/TX Pins. Position config uses an admin message sending a `Config.Position` protobuf.

Position data from GPS is provided by either the radio or your paired phone. Position data is not required to use Meshtastic but time calculations require at least one device on the mesh have either a GPS or internet connection for time.

### Position Config Values

#### GPS Enabled

Acceptable values: `true` or `false`

Defaults to true. Enables GPS on the node.

#### GPS Update Interval

How often we should try to get GPS position (in seconds), or zero for the default of once every 2 minutes, or a very large value (maxint) to update only once at boot.

#### GPS Attempt Time

How long should we try to get our position during each GPS update interval attempt? (in seconds) Or if zero, use the default of 15 minutes.

#### Fixed Position

Acceptable values: `true` or `false`

False by default

If set, this node is at a fixed position. The device will generate GPS updates at the regular GPS update interval, but use whatever the last lat/lon/alt it saved for the node. The lat/lon/alt can be set by an internal GPS or with the help of the mobile device's GPS.

#### Smart Broadcast

Acceptable values: `true` or `false`

True by default

Smart broadcast will send out your position at an increased frequency only if your location has changed enough for a position update to be useful.

Smart broadcast complements broadcast interval (doesn't override that setting) but will apply an algorithm to more frequently update your mesh network if you are in motion and then throttle it down when you are standing still. If you use this feature, it's best to leave broadcast interval at the default.

Smart broadcast will calculate an ideal position update interval based on the data rate of your selected channel configuration.

#### Smart Broadcast Minimum Distance

Default of `0` is 100 meters

The minimum distance in meters traveled (since the last send) before we can send a position to the mesh if smart broadcast is enabled.

#### Smart Broadcast Minimum Interval

Default of `0` is 30 seconds

The minimum number of seconds (since the last send) before we can send a position to the mesh if smart broadcast is enabled.

#### Broadcast Interval

Default of `0` is 15 minutes

If smart broadcast is off we should send our position this often (but only if it has changed significantly)

The GPS updates will be sent out every Broadcast Interval, with either the actual GPS location, or an empty location if no GPS fix was achieved.

#### Position Flags

Defines which options are sent in POSITION messages. Values are stored as a bit field of boolean configuration options (bitwise OR of PositionFlags).

|        Value        |                            Description                            |
| :-----------------: | :---------------------------------------------------------------: |
|        UNSET        |                      Required for compilation                     |
|       ALTITUDE      |              Include an altitude value (if available)             |
|    ALTITUDE\_MSL    |                       Altitude value is MSL                       |
| GEOIDAL\_SEPARATION |                     Include geoidal separation                    |
|         DOP         |      Include the DOP value ; PDOP used by default, see below      |
|        HVDOP        | If POS\_DOP set, send separate HDOP / VDOP values instead of PDOP |
|      SATINVIEW      |               Include number of "satellites in view"              |
|       SEQ\_NO       |          Include a sequence number incremented per packet         |
|      TIMESTAMP      |          Include positional timestamp (from GPS solution)         |
|       HEADING       |           Include positional heading (from GPS solution)          |
|        SPEED        |            Include positional speed (from GPS solution)           |

#### GPIO RX/TX for GPS Module

If your device does not have a fixed GPS chip, you can define the GPIO pins for the RX and TX pins of a GPS module.

### Position Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Position Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Position**

:::

:::info All position config values are available on iOS, iPadOS and macOS at Settings > Radio Configuration > Position. :::

:::info

All Position config commands are available in the python CLI. Example commands are below:

:::

|                       Setting                      |                                                             Acceptable Values                                                             |            Default           |
| :------------------------------------------------: | :---------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------: |
|                position.gps\_enabled               |                                                              `true`, `false`                                                              |            `true`            |
|           position.gps\_update\_interval           |                                                            `integer` (seconds)                                                            |   Default `0` is 2 Minutes   |
|             position.gps\_attempt\_time            |                                                            `integer` (seconds)                                                            | Default of `0` is 15 Minutes |
|              position.fixed\_position              |                                                              `true`, `false`                                                              |            `false`           |
|    position.position\_broadcast\_smart\_enabled    |                                                              `true`, `false`                                                              |            `true`            |
|    position.broadcast\_smart\_minimum\_distance    |                                                             `integer` (meters)                                                            | Default of `0` is 100 Meters |
| position.broadcast\_smart\_minimum\_interval\_secs |                                                            `integer` (seconds)                                                            | Default of `0` is 15 Minutes |
|         position.position\_broadcast\_secs         |                                                            `integer` (seconds)                                                            | Default of `0` is 30 Seconds |
|                   position.flags                   | `UNSET`, `ALTITUDE`, `ALTITUDE_MSL`, `GEOIDAL_SEPARATION`, `DOP`, `HVDOP`, `PDOP`, `SATINVIEW`, `SEQ_NO`, `TIMESTAMP`, `HEADING`, `SPEED` |            `UNSET`           |
|                  position.rx\_gpio                 |                                                              `integer` (0-39)                                                             |            `UNSET`           |
|                  position.tx\_gpio                 |                                                              `integer` (0-34)                                                             |            `UNSET`           |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one. **This is especially important for position values to ensure they are set at the same time and avoid being overwritten by subsequent commands.**

```shell
meshtastic --set position.fixed_position true --setlat 37.8651 --setlon -119.5383
```

:::

```shell
meshtastic --set position.gps_update_interval 0
meshtastic --set position.gps_update_interval 45
```

```shell
meshtastic --set position.gps_attempt_time 0
meshtastic --set position.gps_attempt_time 45
```

```shell
meshtastic --set position.fixed_position true
```

:::note The device will continue to acquire GPS coordinates according to the `gps_update_interval`, but will use the last saved coordinates as its fixed point. :::

```shell
meshtastic --setlat 37.8651 --setlon -119.5383
```

```shell
meshtastic --set position.fixed_position false
```

```shell
meshtastic --set position.position_broadcast_smart_enabled true
meshtastic --get position.position_broadcast_smart_enabled false
```

```shell
meshtastic --set position.broadcast_secs 0
meshtastic --set position.broadcast_secs 60
```

:::note It may take some time to see that the change has taken effect. The GPS location is updated according to the value specified on `gps_update_interval` and the mesh will be notified of the new position in relation to the `position_broadcast_secs` value. :::

```shell
meshtastic --pos-fields ALTITUDE ALTITUDE_MSL
meshtastic --pos-fields UNSET
```

:::info All position config options are available in the Web UI. :::

:::caution Altering/disabling the GPS functionality does not mean that you will be unable to be found. Via triangulation of your radio, location may be given up to someone if they are determined enough. :::
