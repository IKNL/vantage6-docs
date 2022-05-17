---
description: The only VPN that is currently compatible
---

# EduVPN

_Please note that EduVPN is an **optional** component. It enables the use of advanced algorithms that require node-to-node communication. Most algorithms will work without it._

[EduVPN ](https://www.eduvpn.org/)provides an API for the OpenVPN server, which is required for automated certificate retrieval by the nodes. Like vantage6, it is an open source platform.&#x20;

{% hint style="info" %}
We are considering to eliminate the need for an EduVPN server by implementing a vantage6 API for OpenVPN.
{% endhint %}

The following documentation shows you how to install EduVPN:

* [Debian](https://github.com/eduvpn/documentation/blob/v2/DEPLOY\_DEBIAN.md)
* [Centos](https://github.com/eduvpn/documentation/blob/v2/DEPLOY\_CENTOS.md)
* [Fedora](https://github.com/eduvpn/documentation/blob/v2/DEPLOY\_FEDORA.md)

After the installation is done, you need to configure the server to:

1. Enable client-to-client communication. This can be achieved in the configuration file by the `clientToClient` setting (see [here](https://github.com/eduvpn/documentation/blob/v2/PROFILE\_CONFIG.md)).
2. Do not block LAN communication (set `blockLan` to `false`). This allows your docker subnetworks to continue to communicate, which is required for vantage6 to function normally.
3. Enable [**port sharing**](https://github.com/eduvpn/documentation/blob/v2/PORT\_SHARING.md) (Optional). This may be useful if the nodes are behind a strict firewall. Port sharing allows nodes to connect to the VPN server only using outgoing `tcp/443`. Be aware that [TCP meltdown](https://openvpn.net/faq/what-is-tcp-meltdown/) can occur when using the TCP protocol for VPN.
4. Create an application account.

{% hint style="warning" %}
EduVPN allows to listen to multiple protocols (UDP/TCP) and ports at the same time. Be aware that all nodes need to be connected using the same protocol and port in order to communicate with each other.&#x20;
{% endhint %}

## Example configuration

{% tabs %}
{% tab title="/etc/vpn-server-api/config.php" %}
This server listens to `TCP/443` only. Make sure you set `clientToClient` to true and `blockLan` to false. The `range` needs to be supplied to the node configuration files. Also note that the server configured below uses [port-sharing](https://github.com/eduvpn/documentation/blob/v2/PORT\_SHARING.md).

```php
// /etc/vpn-server-api/config.php
<?php

return [
    // List of VPN profiles
    'vpnProfiles' => [
        'internet' => [
            // The number of this profile, every profile per instance has a
            // unique number
            // REQUIRED
            'profileNumber' => 1,

            // The name of the profile as shown in the user and admin portals
            // REQUIRED
            'displayName' => 'vantage6 :: vpn service',

            // The IPv4 range of the network that will be assigned to clients
            // REQUIRED
            'range' => '10.76.0.0/16',

            // The IPv6 range of the network that will be assigned to clients
            // REQUIRED
            'range6' => 'fd58:63db:3245:d20d::/64',

            // The hostname the VPN client(s) will connect to
            // REQUIRED
            'hostName' => 'eduvpn.vantage6.ai',

            // The address the OpenVPN processes will listen on
            // DEFAULT = '::'
            'listen' => '::',

            // The IP address to use for connecting to OpenVPN processes
            // DEFAULT = '127.0.0.1'
            'managementIp' => '127.0.0.1',

            // Whether or not to route all traffic from the client over the VPN
            // DEFAULT = false
            'defaultGateway' => true,

            // Block access to local LAN when VPN is active
            // DEFAULT = false
            'blockLan' => false,

            // IPv4 and IPv6 routes to push to the client, only used when
            // defaultGateway is false
            // DEFAULT = []
            'routes' => [],

            // IPv4 and IPv6 address of DNS server(s) to push to the client
            // DEFAULT  = []
            // Quad9 (https://www.quad9.net)
            'dns' => ['9.9.9.9', '2620:fe::fe'],

            // Whether or not to allow client-to-client traffic
            // DEFAULT = false
            'clientToClient' => true,

            // Whether or not to enable OpenVPN logging
            // DEFAULT = false
            'enableLog' => false,

            // Whether or not to enable ACLs for controlling who can connect
            // DEFAULT = false
            'enableAcl' => false,

            // The list of permissions to allow access, requires enableAcl to
            // be true
            // DEFAULT  = []
            'aclPermissionList' => [],

            // The protocols and ports the OpenVPN processes should use, MUST
            // be either 1, 2, 4, 8 or 16 proto/port combinations
            // DEFAULT = ['udp/1194', 'tcp/1194']
            'vpnProtoPorts' => [
                'tcp/1195',
            ],

            // List the protocols and ports exposed to the VPN clients. Useful
            // for OpenVPN port sharing. When empty (or missing), uses list
            // from vpnProtoPorts
            // DEFAULT = []
            'exposedVpnProtoPorts' => [
                'tcp/443',
            ],

            // Hide the profile from the user portal, i.e. do not allow the
            // user to choose it
            // DEFAULT = false
            'hideProfile' => false,

            // Protect to TLS control channel with PSK
            // DEFAULT = tls-crypt
            'tlsProtection' => 'tls-crypt',
            //'tlsProtection' => false,
        ],
    ],

    // API consumers & credentials
    'apiConsumers' => [
        'vpn-user-portal' => '***',
        'vpn-server-node' => '***',
    ],
];
```
{% endtab %}

{% tab title="/etc/vpn-user-portal/config.php" %}
We need to add an API user. The username, `vantage6-user` in this case, and the `client_secret` have to be added to the vantage6-server configuration file.

```php
...
'Api' => [
  'consumerList' => [
    'vantage6-user' => [
      'redirect_uri_list' => [
        'http://localhost',
      ],
      'display_name' => 'vantage6',
      'require_approval' => false,
      'client_secret' => '***'
    ]
  ]
...
```
{% endtab %}
{% endtabs %}
