# Traceroute Module Usage

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

Due to the limited bandwidth of LoRa, Meshtastic does not keep track of the nodes a message used to hop to the destination. However, from firmware 2.0.8 on, there is a Traceroute Module that can show you this.

Only nodes that know the encryption key of the channel you use can be tracked. Also note that a message may arrive via multiple routes due to duplication because of rebroadcasting. This module will only track the hops of the first packet containing the traceroute request that arrived at the destination.

In order to use it, make sure your devices use firmware version 2.0.8 or higher.

### Traceroute Module Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

Make sure the app is at least version 2.1.10.

Under the node list, long hold a destination node and select 'Traceroute' to send the request. Depending on the amount of hops that is needed, this might take a while. The result will be shown using a pop-up.

Make sure the app is at least version 2.0.9.

Under Contacts > Direct Messages, long hold a destination node and select 'Trace Route' to send the request. Depending on the amount of hops that is needed, this might take a while. The result will be shown in the Mesh Log.

Make sure the CLI is at least version 2.0.6. Then use this command:

```shell
meshtastic --traceroute 'destinationId'
```

Where for `destinationId` you have to fill in the ID of the destination you want to track the hops to, which you can get from running `meshtastic --nodes`. Depending on your OS, you might need quotation marks around the ID. Then it will send a specific message to track the hops. For example, this is what you will get:

```shell
meshtastic --traceroute '!bff18ce4'
Connected to radio
Sending traceroute request to !bff18ce4 (this could take a while)
Route traced:
!25048234 --> !ba4bf9d0 --> !bff18ce4
```

The first ID shown is the device you are connected to with the CLI. As you can see, this packet went through one other node to get to its destination.

Not yet implemented.
