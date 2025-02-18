---
html: stop.html
parent: server-control-methods.html
blurb: Shut down the rippled server.
---
# stop
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Stop.cpp "Source")

Gracefully shuts down the server.

*The `stop` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users!*

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 0,
    "command": "stop"
}
```

*JSON-RPC*

```json
{
    "method": "stop",
    "params": [
        {}
    ]
}
```

*Commandline*

```sh
#Syntax: stop
rippled stop
```

<!-- MULTICODE_BLOCK_END -->

The request includes no parameters.

### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*JSON-RPC*

```json
{
   "result" : {
      "message" : "ripple server stopping",
      "status" : "success"
   }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "message" : "ripple server stopping",
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`   | Type   | Description                          |
|:----------|:-------|:-------------------------------------|
| `message` | String | `ripple server stopping` on success. |

### Possible Errors

* 任何[通用错误类型][]。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
