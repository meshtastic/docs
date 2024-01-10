# Neighbor Info Module Usage

import Tabs from "@theme/Tabs"; import TabItem from "@theme/TabItem";

The Neighbor Info Module is for sending information on each node's 0-hop neighbors to the mesh. Config options are: Enabled and Update Interval.

In order to use it, make sure your devices use firmware version 2.2.0 or higher.

### Neighbor Info Module Config Values

#### Enabled

Enables the Neighbor Info Module.

#### Update Interval

How often the neighbor info is sent to the mesh.

### Neighbor Info Module Client Availability

\<Tabs groupId="settings" defaultValue="cli" values={\[ {label: 'Android', value: 'android'}, {label: 'Apple', value: 'apple'}, {label: 'CLI', value: 'cli'}, {label: 'Web', value: 'web'}, ]}>

:::info All Neighbor Info Module config options are available for Android in app version 2.2.0 and higher.

1. Open the Meshtastic App
2. Navigate to: **Vertical Ellipsis (3 dots top right) > Radio Configuration > Neighbor Info** :::

Not yet implemented.

:::info

All Neighbor Info Module config options are available in the python CLI version 2.2.0 and higher.

:::

Example commands are below:

```shell
meshtastic --set neighbor_info.enabled true
meshtastic --set neighbor_info.enabled false
```

```shell
meshtastic --set neighbor_info.update_interval 600
```

```shell
meshtastic --get neighbor_info
```

:::info

All Neighbor Info module config options are available in the Web UI.

:::
