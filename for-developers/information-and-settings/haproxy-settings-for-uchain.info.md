---
description: UCHAIN.INFO.com uses haproxy for all uchaininfo hosted and external instances.
---

# haproxy Settings for UCHAIN.INFO.com

{% hint style="success" %}
If you would like to add your instance to UCHAIN.INFO.com, make the required changes and submit a PR to [https://github.com/UdotCASH/uchain-info/haproxy-config](https://github.com/UdotCASH/uchain-info/haproxy-config)&#x20;
{% endhint %}

## Instructions

UCHAIN.INFO uses two-part path names for network urls:\
the first part (`org`)  designates the organization/type of the network, \
the second part (`netname`) names the network.&#x20;

**For example:**

* **Eth mainnet:** `https://uchain.info/eth/mainnet` \
  &#x20;(`org=eth`, `netname=mainnet`)
* **Ethereum Classic mainnet:** `https://uchain.info/etc/mainnet` \
  &#x20;(`org=etc`, `netname=mainnet`)
* **Kovan testnet:** `https://uchain.info/eth/kovan` \
  (`org=eth`, `netname=kovan`)

### 1. Add a new `org` for your uchaininfo instance's url

1\) Add a new `acl` under the  #**Check for network type** section in the cfg file.

```
#Check for network type
...
acl is_myorg path_beg -i /myorg
```

2\)  Add a corresponding exception `!is_myorg` to the  **#default redirect** section

```
# default redirect
redirect prefix /poa/xdai if !is_websocket !is_poa !is_etc !is_eth !is_lukso !is_rsk !is_myorg !is_cookie
```

3\) add `!is_myorg` to **every line** in the **#redirect for networks** section

```
#redirect for networks
redirect prefix /poa/core if !is_websocket !is_poa !is_etc !is_eth !is_lukso !is_rsk !is_myorg is_cookie_core
redirect prefix /poa/sokol if !is_websocket !is_poa !is_etc !is_eth !is_lukso !is_rsk !is_myorg is_cookie_sokol
# do this for every line
...
```

### 2. Add a new acl with your netname

In the  **#Check for network name** section add a new `acl` with your `netname` similar to existing ones

```
#Check for network name
...
acl is_mynet path_beg -i /myorg/mynet
```

### 3) Set the cookie and name of rpc/websockets servers

```
#Check for cookies
...
acl is_cookie_mynet hdr_sub(cookie) network=mynet

#Default backends
...
use_backend mynet if is_mynet

#WebSocket backends
...
use_backend mynet_ws if is_cookie_mynet is_websocket
```

### 4. Add new backend sections to the end of the file

Add new `backend_mynet` and `backend_mynet_ws` sections to the end of the file and provide dns names of your uchaininfo instances.

**For example:**

```
backend mynet
    #Proxy mode
    mode http

    #Backend queries should not have network prefix, so we delete it
    acl is_mynet path_beg /myorg/mynet
    reqirep ^([^\ ].*)myorg/mynet[/]?(.*) \1\2 if is_mynet

    #Setting headers to mask our query
    http-request set-header X-Forwarded-Host %[req.hdr(Host)]
    http-request set-header X-Client-IP %[src]
    http-request set-header Host my-uchaininfo-instance.com

    #Specifying cookie to insert
    cookie network insert

    #Server list. Format:
    #server <name> <address>:<port> cookie <network_name> check inter 60000 fastinter 1000 fall 3 rise 3 ssl verify none
    server mynet_server my-uchaininfo-instance.com:443 cookie mynet check inter 60000 fastinter 1000 fall 3 rise 3 ssl verify none

backend mynet_ws
    #Check if WebSocket request is correct
    acl hdr_websocket_key      hdr_cnt(Sec-WebSocket-Key)      eq 1
    acl hdr_websocket_version  hdr_cnt(Sec-WebSocket-Version)  eq 1
    http-request deny if ! hdr_websocket_key ! hdr_websocket_version

    #WebSockets should be forwarded, not http proxied
    option forwardfor

    #Setting headers to mask our query
    http-request set-header Host my-uchaininfo-instance.com
    http-request set-header Origin https://my-uchaininfo-instance.com

    #Specifying cookie to insert
    cookie network insert

    #Server list. Format:
    #server <name> <address>:<port> cookie <network_name> check inter 60000 fastinter 1000 fall 3 rise 3 check inter 60000 fastinter 1000 fall 3 rise 3 ssl verify none maxconn 30000
    server mynet_server my-uchaininfo-instance.com:443 cookie mynet check inter 60000 fastinter 1000 fall 3 rise 3 ssl verify none maxconn 30000
```

{% hint style="warning" %}
Change the following parameters to match your specific case
{% endhint %}

* `backend mynet`:
  * `acl is_...`
  * `reqirep ...`
  * `http-request set-header Host ...`
  * `server mynet_server...` - **don't forget to specify port 443 here**
* `backend mynet_ws`:
  * `http-request set-header Host...`
  * `http-request set-header Origin ...` **don't forget `https` here**
  * `server mynet_server...` - **don't forget port 443 here**

{% hint style="success" %}
Submit a PR to the configuration repo at [https://github.com/UdotCASH/uchain-info/haproxy-config](https://github.com/UdotCASH/uchain-info/haproxy-config)&#x20;
{% endhint %}
