---
html: random.html
parent: utility-methods.html
blurb: Generate a random number.
---
# random
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Random.cpp "Source")

`random`命令提供一个随机数，用作客户端生成随机数的熵源。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "command": "random"
}
```

*JSON-RPC*

```json
{
    "method": "random",
    "params": [
        {}
    ]
}
```

*Commandline*

```sh
#Syntax: random
rippled random
```

<!-- MULTICODE_BLOCK_END -->

请求不包含任何参数。

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "result": {
        "random": "8ED765AEBBD6767603C2C9375B2679AEC76E6A8133EF59F04F9FC1AAA70E41AF"
    },
    "status": "success",
    "type": "response"
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "random": "4E57146AA47BC6E88FDFE8BAA235B900126C916B6CC521550996F590487B837A",
        "status": "success"
    }
}
```

*Commandline*

```json
{
   "result" : {
      "random" : "DB7C23C7F224CD410912E68A997BE0FD0FA7175A4C74B829BE5A80ED0DBAA0C5",
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`  | Type   | Description               |
|:---------|:-------|:--------------------------|
| `random` | String | 随机256位十六进制值。 |

## Possible Errors

* 任何[通用错误类型][]。
* `internal` - 发生了一些内部错误，可能与随机数生成器有关。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}			
{% include '_snippets/tx-type-links.md' %}			
{% include '_snippets/rippled_versions.md' %}
