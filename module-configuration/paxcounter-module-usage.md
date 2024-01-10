# Paxcounter Module Usage

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Paxcounter module counts the number of people passing by a specific area. It is commonly used in retail stores, museums, and other public spaces to monitor foot traffic and gather valuable data for analysis.

In order to use this module, make sure your devices have firmware version 2.2.17 or higher.

### Paxcounter Module Config Values

#### Enabled

Whether the Module is enabled.

#### Update Interval

The interval in seconds of how often we can send a message to the mesh when a state change is detected.

### Paxcounter Module Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info No Paxcounter Module config options are available for Android. :::

:::info No Paxcounter Module config options are available on the iOS, iPadOS and macOS app. :::

:::info

All Paxcounter Module config options are available in the python CLI version 2.2.16 and higher.

:::

Example commands are below:

```shell
meshtastic --set paxcounter.enabled true
meshtastic --set paxcounter.enabled false
```

```shell
meshtastic --set paxcounter.paxcounter_update_interval 900
```

```shell
meshtastic --get paxcounter
```

:::info

No Paxcounter module config options are available in the Web UI.

:::
