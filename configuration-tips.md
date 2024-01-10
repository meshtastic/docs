# Configuration Tips

### Roles

Leave your ROLE set to `CLIENT` unless you're sure another role would suit the node's purpose. All `CLIENT` nodes will "repeat" and "route" packets. Roles are for very specific applications so please read the documentation before changing your node's role.

:::info

Users report that the current implementation of the `ROUTER` role with bluetooth switched off causes instability. If you would like the functionality associated with this role, it is recommended you choose `ROUTER_CLIENT` until a firmware fix is issued.

:::

### (Not) Sharing Your Location

Telemetry is shared over your PRIMARY channel. This means that if your node has acquired GPS coordinates from an integrated GPS chip, or from your mobile device, your coordinates will be sent to the mesh over this channel, using it's defined encryption (if any).

By default the PRIMARY channel's name is LongFast with the encryption key "AQ==" (Base64 equivalent of Hex 0x01). If this is left unchanged, your location will be shared with all nodes in range that are also using the default channel.

#### Creating a Private Primary with Default Secondary

If you'd like to connect with other Meshtastic users but only share your location with trusted parties, you may create a private PRIMARY channel and use the defaults for a SECONDARY channel.

1. Ensure you have not changed the LoRa Modem Preset from the default `unset` / `LONG_FAST`.
2. On your PRIMARY channel, set anything you'd like for the channel's name and choose a random PSK.
3. Enable a SECONDARY channel named "LongFast" with PSK "AQ==".
4. If your LoRa channel is set to the default (`0`), the radio's frequency will be automatically changed based on your PRIMARY channel's name. In this case, you will have to manually set it back to your region's default (in LoRa settings) in order to interface with users on the default channel:

#### Default Primary Channels by Region

|  US | EU\_433 | EU\_868 |  CN |  JP | ANZ |  KR |  TW |  RU |  IN | NZ\_865 |  TH | LORA\_24 | UA\_433 | UA\_868 |
| :-: | :-----: | :-----: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-----: | :-: | :------: | :-----: | :-----: |
|  20 |    4    |    1    |  36 |  20 |  20 |  12 |  16 |  2  |  4  |    4    |  16 |     6    |    6    |    2    |

To quickly test this configuration, find and scan your region's QR from [this repository](https://github.com/meshtastic/meshtastic/tree/master/static/img/configuration/qr-private-primary-example/). Remember to generate a new PSK for your private channel before sharing with your trusted nodes.

### Rebroadcast "Public" Traffic

Meshtastic nodes will rebroadcast all packets if they share LoRa modem settings, regardless of encryption (unless Rebroadcast mode is set to `LOCAL_ONLY`).

:::info If you would like your nodes to include/expand the "public" mesh, you must use the default modem preset `LONG_FAST`. If you change your PRIMARY channel name, you must manually set the LoRa channel to the default for your region (see above). :::

### Chat Channels VS LoRa Modem Channels

Meshtastic uses the word "channels" to define two different configuration properties: Messaging Channels & LoRa Modem Channels

#### Radio Config: Channels

These configure "message groups" and include your PRIMARY and SECONDARY channels. All SECONDARY channels use the same LoRa modem config as your PRIMARY channel (including LoRa channel number).

There are 8 total chat channels. Channel 0 is your PRIMARY channel, with channels 1-7 available for private group messaging and/or special channels such as `admin`.

#### Radio Config: LoRa: Channel Number

This configures the frequency the radio is set to. Check out the frequency calculator to view the relationship between "channel number" and radio frequency.

### Best Practices

* If you are part of a large mesh and don't know what a setting does, don't change it (unless you're super curious).
* Leave your MAX HOPS set to 3 unless you're sure you need more (or less) to reach your destination node.
* TEST your settings and hardware before you install in hard-to-reach locations.
* Connecting a node to the public MQTT server may publish the locations of all nodes in your mesh to the internet. This will also add every globally connected node to your node database and potentially flood your mesh with all types of packets.
