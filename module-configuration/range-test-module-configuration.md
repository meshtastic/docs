# Range Test Module Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

This module allows you to test the range of your Meshtastic nodes. It requires at least two nodes, a sender and a receiver. The receiving node saves the messages along with the GPS coordinates at which they were received into a .csv file. This .csv file can then be integrated into [Google Earth](https://earth.google.com), [Google Maps - My Maps](https://mymaps.google.com), or any other program capable of processing .csv files. This can enable you to visualize your mesh.

While a minimum of two radios is required, more can be used. You can have any number of receivers and senders that your mesh is able to handle. You can test having a single sender with multiple receivers or a single receiver with multiple senders. Let us know on the [forum thread](https://meshtastic.discourse.group/t/new-plugin-rangetestplugin/2591) the results of your configuration.

:::info

Be sure to turn off the module or disable sending when not in use. This will use a lot of time on air, slow down your mesh, and spam your channel.

:::

The range test module config options are: Enabled, Save, and Sender. Range Test Module config uses an admin message sending a `ConfigModule.RangeTest` protobuf.

### Range Test Module Config Values

#### Enabled

Enables the range test module.

#### Save CSV File

:::info

Saving files is only available on ESP32-based devises

:::

If enabled, a log of all received messages will be saved to a file named rangetest.csv.

To access this file, first turn on the WiFi on your device and connect to your network. Once you can connect to your device, navigate to `meshtastic.local/rangetest.csv` (or your\_device\_ip/rangetest.csv) and the file will be downloaded automatically. This file will only be created after receiving initial messages.

To prevent filling up the storage, the device will abort writing if there is less than 50kb of space on the filesystem.

#### Sender Interval

How long to wait between sending sequential test packets. 0 is default which disables sending messages.

#### Recommended Sender Settings

| Radio Setting | `range_test.sender` |
| :-----------: | :-----------------: |
|   Long Slow   |          60         |
|    Long Alt   |          30         |
|     Medium    |          15         |
|   Short Fast  |          15         |

### Range Test Module Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Range Test Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Range Test**

:::

Android also had the option to download a rangetest.csv file which is stored on your phone. This file does not require the Range Test module to be active and will log all incoming Nodeinfo, Position, Telemetry and Messages.

:::info All range test module config options are available on iOS, iPadOS and macOS at Settings > Modules > Range Test. :::

Apple apps also have the option to download logged position data which is stored on your iPhone/iPad/Mac. Access this by clicking on the Nodes tab, selecting a node, then select Position Log and click Save. This data file does not require the Range Test module to be active.

:::info

Range Test module config options are available in the python CLI. Example commands are below:

:::

|       Setting       |  Acceptable Values  | Default |
| :-----------------: | :-----------------: | :-----: |
| range\_test.enabled |   `true`, `false`   | `false` |
|   range\_test.save  |   `true`, `false`   | `false` |
|  range\_test.sender | `integer` (Seconds) |   `0`   |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set range_test.enabled true --set range_test.save false
```

:::

```shell
meshtastic --set range_test.enabled true
meshtastic --set range_test.enabled false
```

```shell
meshtastic --set range_test.save true
meshtastic --set range_test.save false
```

```shell
meshtastic --set range_test.sender 60
```

```shell
meshtastic --set range_test.sender 0
```

:::info

All range test module config options are available in the Web UI.

:::

### Application Examples

#### Google Earth Integration

Steps:

1. [Download](https://www.google.com/earth/versions/#download-pro) and open Google Earth
   1. Select File > Import
   2. Select CSV
   3. Select Delimited, Comma
   4. Make sure the button that states “This dataset does not contain latitude/longitude information, but street addresses” is unchecked
   5. Select “rx lat” & “rx long” for the appropriate lat/lng fields
   6. Click finish
2. When it prompts you to create a style template, click yes.
   1. Set the name field to whichever column you want to be displayed on the map (don’t worry about this too much, when you click on an icon, all the relevant data appears)
   2. Select a color, icon, etc. and hit OK.

Your data will load onto the map, make sure to click the checkbox next to your dataset in the sidebar to view it.

#### My Maps

You can use [My Maps](http://mymaps.google.com). It takes CSVs and the whole interface is much easier to work with.

Google has instructions on how to do that [here](https://support.google.com/mymaps/answer/3024836?co=GENIE.Platform%3DDesktop\&hl=en#zippy=%2Cstep-prepare-your-info%2Cstep-import-info-into-the-map).

You can style the ranges differently based on the values, so you can have the pins be darker the if the SNR or RSSI (if that gets added) is higher.

### FAQ

Q: Do I need to have WiFi turned on for the file to be saved?

* Nope, it'll just work.

Q: Do I need a phone for this module?

* There's no need for a phone.

Q: Can I use this as a message logger?

* While it's not the intended purpose, sure, why not. Do it!

Q: What will happen if I run out of space on my device?

* We have a protection in place to keep you from completely filling up your device. This will make sure that other device critical functions will continue to work. We will reserve at least 50k of free space.

Q: What do I do with the rangetest.csv file when I'm done?

* Currently the only way to erase the file is to perform a factory reset.

Q: Can I use this as a sender while on battery power?

* Yes, but your battery will run down quicker than normal. While sending, we tell the device not to go into low-power mode since it needs to keep to a fairly strict timer.

Q: Why is this operating on incoming messages instead of the existing location discovery protocol?

* This module is still young and currently supports monitoring just one port at a time. I decided to use the existing message port because that is easy to test with. A future version will listen to multiple ports to be more promiscuous.
