---
html: ledger_closed.html
parent: ledger-methods.html
blurb: Get the latest closed ledger version.
---
# ledger_closed
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerClosed.cpp "Source")

`ledger_closed`方法返回最近关闭的分类帐的唯一标识符(这个分类账还不一定是有效的和不变的）。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "id": 2,
   "command": "ledger_closed"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_closed",
    "params": [
        {}
    ]
}
```

*Commandline*

```sh
#Syntax: ledger_closed
rippled ledger_closed
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_closed)

This method accepts no parameters.

## Response Format
成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_hash": "17ACB57A0F73B5160713E81FE72B2AC9F6064541004E272BD09F257D57C30C02",
    "ledger_index": 6643099
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "ledger_hash": "8B5A0C5F6B198254A6E411AF55C29EE40AA86251D2E78DD0BB17647047FA9C24",
        "ledger_index": 8696231,
        "status": "success"
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_hash" : "6F5D3B97F1CAA8440AFCED3CA10FB9DC6472F64DEBC2EFAE7CAE7FC0123F32DA",
      "ledger_index" : 56843991,
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`        | Type             | Description                              |
|:---------------|:-----------------|:-----------------------------------------|
| `ledger_hash`  | String           | 此分类帐版本的唯一[哈希][]（十六进制）。 |
| `ledger_index` | Unsigned Integer | 此分类帐版本的[分类帐索引][]。          |

## Possible Errors

* 任何[通用错误类型][]。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
