# Channel Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Channels config options are: Index, Roles, and Settings. Channel config uses an admin message sending a `Channel` protobuf which also consists of a `ChannelSettings` protobuf.

:::info **Channel Settings** (as described on this page) should not be confused with Modem Preset Settings

Modem Preset Settings contain the modem configuration (frequency settings, spreading factor, bandwidth, etc.) used for the LoRa radio. These settings are identical for all channels and can **not** be unique per channel.

**Channel Settings** contain information for segregating message groups, configuring optional encryption, and enabling or disabling messaging over internet gateways. These settings **are** unique and configurable per channel. :::

### Channel Config Values

#### Index

The channel index begins at 0 and ends at 7.

_Indexing_ can not be modified.

| Index | Channel | Default Role |          Purpose          |
| :---: | :-----: | :----------: | :-----------------------: |
|   0   |    1    |   `PRIMARY`  | Used as `default` channel |
|   1   |    2    |  `DISABLED`  |        User defined       |
|   2   |    3    |  `DISABLED`  |        User defined       |
|   3   |    4    |  `DISABLED`  |        User defined       |
|   4   |    5    |  `DISABLED`  |        User defined       |
|   5   |    6    |  `DISABLED`  |        User defined       |
|   6   |    7    |  `DISABLED`  |        User defined       |
|   7   |    8    |  `DISABLED`  |        User defined       |

:::note You can **not** have `DISABLED` channels in-between active channels such as `PRIMARY` and `SECONDARY`. Active channels must be consecutive. :::

#### Role

Each channel is assigned one of 3 roles:

1. `PRIMARY` or `1`
   * This is the first channel that is created for you on initial setup.
   * Only one primary channel can exist and can not be disabled.
   * Periodic broadcasts like position and telemetry are only sent over this channel.
2. `SECONDARY` or `2`
   * Can modify the encryption key (PSK).
3. `DISABLED` or `0`
   * The channel is no longer available for use.
   * The channel settings are set to default.

:::note While you can have a different PRIMARY channel and communicate over SECONDARY channels with the same Name & PSK, a hash of the PRIMARY channel's name sets the LoRa channel number, which determines the actual frequency you are transmitting on in the band. To ensure devices with different PRIMARY channel name transmit on the same frequency, you must explicitly set the LoRa channel number. :::

### Channel Settings Values

The Channel Settings options are: Name, PSK, Downlink Enabled, and Uplink Enabled. Channel settings are embedded in the `Channel` protobuf as a `ChannelSettings` protobuf and sent as an admin message.

#### Name

A short identifier for the channel. _(< 12 bytes)_

|  Reserved Name |                                                             Purpose                                                            |
| :------------: | :----------------------------------------------------------------------------------------------------------------------------: |
| `""` (default) |                          If left empty on the Primary channel, this designates the `default` channel.                          |
|     `admin`    | On Secondary channels, the name `admin` (case sensitive) designates the `admin` channel used to administer nodes over the mesh |

:::note

Matching channel names are required in order to communicate on the same channel with other devices. Example: If your device is using the channel name `LongFast` the device you are attempting to communicate with must also have a channel named `LongFast`.

:::

#### PSK

The encryption key used for private channels.

Hex byte `0x01` for the Primary `default` channel.

Must be either 0 bytes (no crypto), 16 bytes (AES128), or 32 bytes (AES256).

:::note

Matching PSKs are required in order to communicate on the same channel with other devices. Example: If your device is using a channel with the default PSK of `AQ==` the device you are attempting to communicate with must also have a matching channel with the same PSK.

:::

#### Downlink Enabled

If enabled, messages captured from a **public** internet gateway will be forwarded to the local mesh.

Set to `false` by default for all channels.

#### Uplink Enabled

If enabled, messages from the mesh will be sent to the **public** internet through any node's configured gateway.

Set to `false` by default for all channels.

### Examples

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info Channel Config options are available on Android. :::&#x20;

The Radio Configuration tab can be used for common tasks:

1. View your current channel configuration QR code and URL.
2. Quickly create or modify your primary channel.
3. Select a modem preset for all your channels i.e. `Long Range / Fast`.

See Android App Usage for more further instruction on setting up your primary channel.



Tap the Channel Name (or the pen icon) to access the Channel Menu:

1. Add, remove, or modify secondary channels
2. Create or modify encryption keys
3. Enable uplink and downlink for individual channels

:::info Channel settings are only available on Apple platforms by scanning QR codes. :::

:::info All Channel config options are available in the python CLI. Example commands are below: :::

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --ch-set name "My Channel" --ch-set psk random --ch-set uplink_enabled true --ch-index 4
```

:::

#### Name

```shell
# without spaces
meshtastic --ch-set name MyChannel --ch-index 0
# with spaces
meshtastic --ch-set name "My Channel" --ch-index 0
```

#### PSK

If you use Meshtastic for exchanging messages you don't want other people to see, `random` is the setting you should use. Selecting `default` or any of the `simple` values from the following table will use publicly known encryption keys. They're shipped with Meshtastic source code and thus, anyone can listen to messages encrypted by them. They're great for testing and public channels.

|         Setting        |                                        Behavior                                       |
| :--------------------: | :-----------------------------------------------------------------------------------: |
|         `none`         |                                   Disable Encryption                                  |
|        `default`       |                    Default Encryption (use the weak encryption key)                   |
|        `random`        | Generate a secure 256-bit encryption key. Use this setting for private communication. |
| `simple0`- `simple254` |                       Uses a single byte encoding for encryption                      |

```shell
meshtastic --ch-set psk default --ch-index 0
```

```shell
meshtastic --ch-set psk random --ch-index 0
```

```shell
meshtastic --ch-set psk simple15 --ch-index 0
```

```shell
meshtastic --ch-set psk 0x1a1a1a1a2b2b2b2b1a1a1a1a2b2b2b2b1a1a1a1a2b2b2b2b1a1a1a1a2b2b2b2b --ch-index 0
```

```shell
meshtastic --ch-set psk base64:puavdd7vtYJh8NUVWgxbsoG2u9Sdqc54YvMLs+KNcMA= --ch-index 0
```

:::tip Use this to copy and paste the `base64` encoded (single channel) key from the meshtastic --info command. Please don't use the omnibus (all channels) code here, it is not a valid key. :::

```shell
meshtastic --ch-set psk none --ch-index 0
```

#### Uplink / Downlink

For configuring gateways, please see MQTT

```shell
meshtastic --ch-set uplink_enabled true --ch-index 0
meshtastic --ch-set uplink_enabled false --ch-index 0
```

```shell
meshtastic --ch-set downlink_enabled true --ch-index 1
meshtastic --ch-set downlink_enabled false --ch-index 5
```

:::info All Channel config options are available in the Web UI. :::
