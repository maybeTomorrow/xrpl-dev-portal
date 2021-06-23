---
html: ping.html
parent: utility-methods.html
blurb: Confirm connectivity with the server.
---
# ping
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Ping.cpp "Source")

`ping`命令返回一个确认，以便客户端可以测试连接状态和延迟。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "command": "ping"
}
```

*JSON-RPC*

```json
{
    "method": "ping",
    "params": [
        {}
    ]
}
```

*Commandline*

```sh
#Syntax: ping
rippled ping
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ping)

The request includes no parameters.

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "result": {},
    "status": "success",
    "type": "response"
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "status": "success"
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果不包含任何字段。客户机可以测量从请求到响应的往返时间作为延迟。

## Possible Errors

* Any of the [universal error types][].

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}			
{% include '_snippets/tx-type-links.md' %}			
{% include '_snippets/rippled_versions.md' %}
