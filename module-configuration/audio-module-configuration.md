# Audio Module Configuration

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The audio module config options are: Codec2 Enabled, PTT GPIO, Audio Bitrate/Codec Mode, I2S Word Select, I2S Data IN, I2S Data OUT and I2S Clock. Audio Module config uses an admin message sending a `ConfigModule.Audio` protobuf.

With this **experimental** module, you can add a digital I2S microphone and speaker to any ESP32 device that has a SX128x radio and operates on the 2.4 GHz ISM Band. The Sub-1GHz bands are not wide enough to support continuous audio packets on the mesh, even in the Short and Fast modes. Right now, the only devices supported are the LilyGo TLora 2.1-1.8 and TLora T3S3 boards.

### Audio Module Config Values

#### Codec2 Enabled

Enables the audio module.

#### PTT GPIO

The GPIO to use for the Push-To-Talk button. The default is GPIO 39 on the ESP32.

#### Audio Bitrate/Codec Mode

The bitrate to use for audio. The default is `CODEC2_700B`. The available options are:

* CODEC2\_DEFAULT
* CODEC2\_3200
* CODEC2\_2400
* CODEC2\_1600
* CODEC2\_1400
* CODEC2\_1300
* CODEC2\_1200
* CODEC2\_700B
* CODEC2\_700

#### I2S Word Select

The GPIO to use for the WS signal in the I2S interface.

#### I2S Data IN

The GPIO to use for the SD signal in the I2S interface.

#### I2S Data OUT

The GPIO to use for the DIN signal in the I2S interface.

#### I2S Clock

The GPIO to use for the SCK signal in the I2S interface.

:::info What is this? These Pins comprise an I2S digital audio interface. Meshtastic uses it in monoaural mode. The software will use the logical 'LEFT' Stereo channel for the microphone and the logical 'RIGHT' Stereo channel for the speaker, so configure your breakouts accordingly. Audio is Half-Duplex, so we can re-use part of the pins for a bi-directional configuration. There's **no** default pin assignment, setting these is mandatory. :::

### Audio Module Config Client Availability

\<Tabs groupId="settings" defaultValue="apple" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info

Audio Config options are available for Android.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Audio**

:::

:::info Audio module config is not available on iOS, iPadOS and macOS. :::

:::info

All audio module config options are available in the python CLI. Example commands are below:

:::

|        Setting        |                                                        Acceptable Values                                                        |          Default         |
| :-------------------: | :-----------------------------------------------------------------------------------------------------------------------------: | :----------------------: |
| audio.codec2\_enabled |                                                         `true`, `false`                                                         |          `false`         |
|     audio.ptt\_pin    |                                                       GPIO Pin Number 1-39                                                      | Default of `39` is Unset |
|     audio.bitrate     | `CODEC2_DEFAULT` `CODEC2_3200` `CODEC2_2400` `CODEC2_1600` `CODEC2_1400` `CODEC2_1300` `CODEC2_1200` `CODEC2_700B` `CODEC2_700` |     `CODEC2_DEFAULT`     |
|     audio.i2s\_ws     |                                                       GPIO Pin Number 1-34                                                      |        no Default        |
|     audio.i2s\_sd     |                                                       GPIO Pin Number 1-39                                                      |        no Default        |
|     audio.i2s\_din    |                                                       GPIO Pin Number 1-34                                                      |        no Default        |
|     audio.i2s\_sck    |                                                       GPIO Pin Number 1-34                                                      |        no Default        |

:::tip

Because the device will reboot after each command is sent via CLI, it is recommended when setting multiple values in a config section that commands be chained together as one.

```shell
meshtastic --set audio.codec2_enabled true --set audio.bitrate CODEC2_1400
```

:::

```shell
meshtastic --set audio.codec2_enabled true
meshtastic --set audio.codec2_enabled false
```

```shell
meshtastic --set audio.i2s_ws 7
```

```shell
meshtastic --set audio.i2s_din 28
```

```shell
meshtastic --set audio.ptt_pin 37
```

```shell
meshtastic --set audio.bitrate CODEC2_DEFAULT
meshtastic --set audio.bitrate CODEC2_1400
```

:::info All audio module config options are available in the Web UI. :::

:::warning

GPIO access is fundamentally dangerous because invalid options can physically damage or destroy your hardware. Ensure that you fully understand the schematic for your particular device before trying this as we do not offer a warranty. Use at your own risk.

This module requires attaching a peripheral accessory to your device. It will not work without one.

:::
