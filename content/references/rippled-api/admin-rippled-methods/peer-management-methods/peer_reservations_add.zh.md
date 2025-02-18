---
html: peer_reservations_add.html
parent: peer-management-methods.html
blurb: Add a reserved slot for a specific peer server.
---
# peer_reservations_add
[[Source]](https://github.com/ripple/rippled/blob/4a1148eb2849513dd1e7ae080288fd47ab57a376/src/ripple/rpc/handlers/Reservations.cpp#L36 "Source")

The `{{currentpage.name}}` method adds or updates a reserved slot for a specific peer server in the XRP Ledger [peer-to-peer network](peer-protocol.html). [New in: rippled 1.4.0][]

_The `{{currentpage.name}}` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._


### Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "peer_reservations_add_example_1",
    "command": "{{currentpage.name}}",
    "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99",
    "description": "Ripple s1 server 'WOOL'"
}
```

*JSON-RPC*

```json
{
    "method": "{{currentpage.name}}",
    "params": [{
      "public_key": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99",
      "description": "Ripple s1 server 'WOOL'"
    }]
}
```

*Commandline*

```sh
#Syntax: {{currentpage.name}} <public_key> [<description>]
rippled {{currentpage.name}} n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99 "Ripple s1 server 'WOOL'"
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`       | Type   | Description                                         |
|:--------------|:-------|:----------------------------------------------------|
| `public_key`  | String | The [node public key][] of the peer reservation to add a reservation for, in [base58][]. |
| `description` | String | _(Optional)_ A custom description for the peer reservation. The server truncates descriptions longer than 64 characters when it restarts. |



### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": "peer_reservations_add_example_1",
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    }
  },
  "status": "success",
  "type": "response"
}
```

*JSON-RPC*

```json
{
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    },
    "status": "success"
  }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  "result": {
    "previous": {
      "description": "Maecenas atavis edite regibus, O et praesidium et dulce decus meum, Sunt quos curriculo pulverem Olympicum Collegisse iuvat metaque fervidis Evitata rotis palmaque nobilis Terrarum dominos evehit ad deos; Hunc, si mobilium turba Quiritium Certat tergeminis tollere honoribus; Illum, si proprio condidit horreo, Quidquid de Libycis verritur areis.",
      "node": "n9Jt8awsPzWLjBCNKVEEDQnw4bQEPjezfcQ4gttD1UzbLT1FoG99"
    },
    "status": "success"
  }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`    | Type   | Description                                            |
|:-----------|:-------|:-------------------------------------------------------|
| `previous` | Object | _(May be omitted)_ The previous entry for the same [node public key][], if there was already a reservation with the same node public key. This object is formatted as a **Peer Reservation Object**, as described below. |

If there was not a previous entry for the same [node public key][], the `result` object is empty.

#### Peer Reservation Object

If the `previous` field is provided, it shows the previous status of this peer reservation, with the following fields:

{% include '_snippets/peer_reservation_object.md' %}
<!--_ -->


### Possible Errors

- 任何[通用错误类型][]。
- `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
- `publicMalformed` - The `public_key` field of the request is not valid. It must be a valid node public key in [base58][] format.
- `reportingUnsupported` - ([Reporting Mode][] servers only) This method is not available in Reporting Mode.

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
