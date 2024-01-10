# Remote Node Administration

:::caution Disclaimer This is an advanced feature that few users should need. Keep in mind that it is possible (if you are not careful) to assign settings to a remote node that cause it to completely drop off of your mesh. We advise network admins have a test node to test settings with before applying changes to a remote node to prevent this. :::

This feature allows you to remotely administer Meshtastic nodes through the mesh.

### Prerequisites

For any node that you want to administer, you must:

1. Configure a channel with the channel name of `admin`.
2. Set the PSK for the `admin` channel with the same key you wish to administer other all other nodes of the mesh with.

### Creating the admin channel

By default, nodes will **only** respond to administrative commands via the local USB/Bluetooth/TCP interface. This provides basic security to prevent unauthorized access and is how normal administration and settings changes work. The only difference for the remote case is that we are sending those commands over the mesh.

Before a node will allow remote admin access, it must have a primary channel:

```shell
meshtastic --info
```

```markdown
Connected to radio
...
Channels:
  PRIMARY psk=default { "psk": "AQ==" }
Primary channel URL: https://meshtastic.org/e/#CgMSAQESCggBOANAA0gBUBs
```

From this output you see can that this node knows about only one channel and that its PSK is set to the default value.

Now we add an admin channel: :::note The name of the channel is important and must be `admin`. :::

```shell
meshtastic --ch-add admin
```

```shell
Connected to radio
Writing modified channels to device
```

Run `meshtastic --info` and your channels will now look like this:

```shell
Connected to radio
...
Channels:
  PRIMARY psk=default { "psk": "AQ==" }
  SECONDARY psk=secret { "psk": "YyDCitupTAOOXTcaMDxyNhDpPa3eThiQFziPFCqT0mo=", "name": "admin" }
Primary channel URL: https://meshtastic.org/e/#CgMSAQESCggBOANAA0gBUBs
Complete URL (includes all channels): https://meshtastic.org/e/#CgMSAQEKKRIgYyDCitupTAOOXTcaMDxyNhDpPa3eThiQFziPFCqT0moaBWFkbWluEgoIATgDQANIAVAb
```

Notice that now we have a new secondary channel and the `--info` option prints out TWO URLs. The `Complete URL` includes all of the channels this node understands. The URL contains the preshared keys and should be treated with caution and kept a secret. When deploying remote administration, you only need the node you want to administer and the node you are locally connected to, to know this new "admin" channel. All of the other nodes will forward the packets as long as they are a member of the primary channel.

### Sharing the admin channel with other nodes

Creating an "admin" channel automatically generates a preshared key. In order to administer other nodes remotely, we need to copy the preshared key to the new nodes.

For this step you need physical access to both the nodes.

1. Create the "admin" channel on the "local node" using the instructions above.
2. Copy the "Complete URL" someplace for permanent reference/access.
3. Connect to the "remote node" over the USB port.
4. For the "remote node" type

```shell
meshtastic --seturl the-url-from-step-2
```

5. Run `meshtastic --info` and confirm that the "Complete URL" is the same for both of the nodes.

At this point you can take your remote node and install it far away. You will still be able to change any of its settings.

### Remotely administering your node

Now that both your local node and the remote node contain your secret admin channel key, you can now change settings remotely:

Get the node list from the local node:

```shell
meshtastic --nodes
```

```markdown
Connected to radio
/-------------------------------------------------------------------------------------------------------------\
|N|    User    |AKA|   ID    |Latitude|Longitude|Altitude|Battery|   SNR   |     LastHeard     |    Since     |
|-+------------+---+---------+--------+---------+--------+-------+---------+-------------------+--------------|
|1|Unknown 9058|?58|!28979058|25.0382°|121.5731°|  N/A   |  N/A  |-13.50 dB|2021-03-22 09:25:42|19 seconds ago|
\-------------------------------------------------------------------------------------------------------------/
```

Using the node ID from that list, send a message through the mesh telling that node to change its owner name. :::info The --dest argument value must be in single quotes for linux/mac: '!28979058', no quotes for Windows: !28979058. :::

```shell
meshtastic --dest '!28979058' --set-owner "Im Remote"
```

```markdown
Connected to radio
Setting device owner to Im Remote
Waiting for an acknowledgment from remote node (this could take a while)
```

And you can now confirm via the local node that the remote node has changed:

```shell
meshtastic --nodes
```

```markdown
Connected to radio
/----------------------------------------------------------------------------------------------------\
|N|  User   |AKA|   ID    |        Position        |Battery|  SNR  |     LastHeard     |    Since    |
|-+---------+---+---------+------------------------+-------+-------+-------------------+-------------|
|1|Im Remote|IR |!28979058|25.0382°, 121.5731°, N/A|  N/A  |8.75 dB|2021-03-22 09:35:42|3 minutes ago|
\----------------------------------------------------------------------------------------------------/
```

Note: you can change **any** parameter, add channels or get settings from the remote node. Here's an example of setting ls\_secs using the `--set` command and printing the position settings from the remote node using the `--get` command:

```shell
meshtastic --dest '!28979058' --set power.ls_secs 301 --get position
```

```markdown
Connected to radio
Requesting current config from remote node (this can take a while).

power:
wait_bluetooth_secs: 60
mesh_sds_timeout_secs: 7200
sds_secs: 4294967295
ls_secs: 300
min_wake_secs: 10

Set power.ls_secs to 301
Writing modified preferences to device
Requesting current config from remote node (this can take a while).
Received an ACK.
Completed getting preferences
Waiting for an acknowledgment from remote node (this could take a while)

position:
position_broadcast_secs: 900
position_broadcast_smart_enabled: true
gps_enabled: true
gps_update_interval: 120
gps_attempt_time: 900
position_flags: 3
rx_gpio: 15
tx_gpio: 13
broadcast_smart_minimum_distance: 100
broadcast_smart_minimum_interval_secs: 30

Completed getting preferences
```

To set the `hop_limit` to 4 and then get the lora settings to confirm your new settings:

```shell
meshtastic --dest '!28979058' --set lora.hop_limit 4 --get lora
```

```markdown
lora:
use_preset: true
region: US
hop_limit: 3
tx_enabled: true
tx_power: 30

Set lora.hop_limit to 4
Writing modified preferences to device
Requesting current config from remote node (this can take a while).
Received an ACK.
Completed getting preferences
Waiting for an acknowledgment from remote node (this could take a while)

lora:
use_preset: true
region: US
hop_limit: 4
tx_enabled: true
tx_power: 30
```

### Admin Channel Setup is Complete

You've finished setting up and adding two devices to the admin channel. Remember, because this is a mesh network, it doesn't matter which node you are at; you could administer your first device we set up from the second one we added to the channel. And the settings and examples on this page are just a taste of the other settings you can set.

For further reading, I recommend starting out with the Meshtastic Python CLI Guide if you haven't already gone through this (hopefully you have since you are reading this). But for a full reference to the settings you can change, please see:

Settings Overview and [Complete list of user settings in Protobufs](https://buf.build/meshtastic/protobufs/docs/main:meshtastic#meshtastic.User)
