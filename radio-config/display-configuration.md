# Display Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The display config options are: Screen On Duration, Auto Carousel Interval, Always Point North, GPS Format, Preferred Display Units, OLED Definition, Display Mode, Heading Bold, and Wake on Tap or Motion. Display config uses an admin message sending a `Config.Display` protobuf.

### Display Config Values

#### Screen On Duration

How long the screen remains on after the user button is pressed or messages are received.

#### Auto Carousel Interval

Automatically toggles to the next page on the screen like a carousel, based on the specified interval.

#### Always Point North

If this is set, the compass heading on the screen outside of the circle will always point north. This feature is off by default and the top of display represents your heading direction, the North indicator will move around the circle.

#### GPS Format

The format used to display GPS coordinates on the device screen.

Acceptable values:

|  Value |           Description           |
| :----: | :-----------------------------: |
|  `DEC` |         Decimal Degrees         |
|  `DMS` |     Degrees Minutes Seconds     |
|  `UTM` |  Universal Transverse Mercator  |
| `MGRS` |  Military Grid Reference System |
|  `OLC` | Open Location Code (Plus Codes) |
| `OSGR` |  Ordnance Survey Grid Reference |

#### Preferred Display Units

Switch between `METRIC` (default) and `IMPERIAL` units

#### Flip Screen

If enabled, the screen will be rotated 180 degrees, for cases that mount the screen upside down

#### OLED Definition

The type of OLED Controller is auto-detected by default, but can be defined with this setting if the auto-detection fails. For the SH1107, we assume a square display with 128x128 Pixels like the GME128128-1.

Acceptable values:

|      Value     |                 Description                 |
| :------------: | :-----------------------------------------: |
|   `OLED_AUTO`  |        Auto detect display controller       |
| `OLED_SSD1306` |          Always use SSD1306 driver          |
|  `OLED_SH1106` |           Always use SH1106 driver          |
|  `OLED_SH1107` | Always use SH1107 driver (Geometry 128x128) |

#### Display Mode

The display mode can be set to `DEFAULT` (default), `TWOCOLOR`, `INVERTED` or `COLOR`. The `TWOCOLOR` mode is intended for OLED displays with the first line of output being a different color than the rest of the display. The `INVERTED` mode will invert that bicolor area, resulting in a white background headline on monochrome displays.

#### Heading Bold

The heading can be hard to read when 'INVERTED' or 'TWOCOLOR' display mode is used. This setting will make the heading bold, so it is easier to read.

#### Wake on Tap or Motion

This option enables the ability to wake the device screen when motion, such as a tap on the device, is detected via an attached accelerometer.

### Display Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Display Config is available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Display**

:::

:::info All display config options are available on iOS, iPadOS and macOS at Settings > Radio Configuration > Display. :::

:::info

All display config options are available in the python CLI. Example commands are below:

:::

| Setting                              | Acceptable Values                                         | Default                       |
| ------------------------------------ | --------------------------------------------------------- | ----------------------------- |
| display.auto\_screen\_carousel\_secs | `integer`                                                 | Default of `0` is off.        |
| display.compass\_north\_top          | `false`, `true`                                           | `false`                       |
| display.flip\_screen                 | `false`, `true`                                           | `false`                       |
| display.gps\_format                  | `DEC`, `DMS`, `UTM`, `MGRS`, `OLC`, `OSGR`                | `DEC`                         |
| display.oled                         | `OLED_AUTO`, `OLED_SSD1306`, `OLED_SH1106`, `OLED_SH1107` | `OLED_AUTO`                   |
| display.screen\_on\_secs             | `integer`                                                 | Default of `0` is 10 minutes. |
| display.units                        | `METRIC`, `IMPERIAL`                                      | `METRIC`                      |
| display.displaymode                  | `DEFAULT`, `TWOCOLOR`, `INVERTED`, `COLOR`                | `DEFAULT`                     |
| display.heading\_bold                | `false`, `true`                                           | `false`                       |
| display.wake\_on\_tap\_or\_motion    | `false`, `true`                                           | `false`                       |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set display.screen_on_secs 120 --set display.gps_format UTM
```

:::

```shell
meshtastic --set display.screen_on_secs 0
meshtastic --set display.screen_on_secs 120
```

```shell
meshtastic --set display.auto_screen_carousel_secs 0
// Set to 2 Minutes (120 Seconds)
meshtastic --set display.auto_screen_carousel_secs 120
```

```shell
meshtastic --set display.gps_format UTM
```

:::info All display config options are available in the Web UI. :::
