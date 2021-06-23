---
html: ledger_current.html
parent: ledger-methods.html
blurb: Get the current working ledger version.
---
# ledger_current
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerCurrent.cpp "Source")

`ledger current`方法返回当前正在进行的[leger](ledgers.html)的唯一标识符。此命令对于测试非常有用，因为返回的分类帐仍在变化中。

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "id": 2,
   "command": "ledger_current"
}
```

*JSON-RPC*

```json
{
    "method": "ledger_current",
    "params": [
        {}
    ]
}
```

*Commandline*

```sh
#Syntax: ledger_current
rippled ledger_current
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger_current)

The request contains no parameters.


## Response Format
成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 6643240
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "ledger_current_index": 8696233,
        "status": "success"
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_current_index" : 56844050,
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                                | Description   |
|:-----------------------|:------------------------------------|:--------------|
| `ledger_current_index` | Unsigned Integer - [Ledger Index][] | 此分类帐版本的分类帐索引。 |

未提供`ledger_hash`字段，因为当前分类帐的哈希值随其内容不断变化。

## Possible Errors

* 任何[通用错误类型][]。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
