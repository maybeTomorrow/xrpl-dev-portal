---
html: ledger_accept.html
parent: server-control-methods.html
blurb: Close and advance the ledger in stand-alone mode.
---
# ledger_accept
[[Source]](https://github.com/ripple/rippled/blob/a61ffab3f9010d8accfaa98aa3cacc7d38e74121/src/ripple/rpc/handlers/LedgerAccept.cpp "Source")

The `ledger_accept` method forces the server to close the current-working ledger and move to the next ledger number. This method is intended for testing purposes only, and is only available when the `rippled` server is running stand-alone mode.

*The `ledger_accept` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users!*

### Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "id": "Accept my ledger!",
   "command": "ledger_accept"
}
```

*Commandline*

```sh
#Syntax: ledger_accept
rippled ledger_accept
```

<!-- MULTICODE_BLOCK_END -->

The request accepts no parameters.

### Response Format

成功响应的示例:
```js
{
  "id": "Accept my ledger!",
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 6643240
  }
}
```

The response follows the [standard format][], with a successful result containing the following field:

| `Field`                | Type             | Description                      |
|:-----------------------|:-----------------|:---------------------------------|
| `ledger_current_index` | Unsigned Integer - [Ledger Index][] | Ledger index of the newly created 'current' ledger |

**Note:** When you close a ledger, `rippled` determines the canonical order of transactions in that ledger and replays them. This can change the outcome of transactions that were provisionally applied to the current ledger.

### Possible Errors

* 任何[通用错误类型][]。
* `notStandAlone` - If the `rippled` server is not currently running in stand-alone mode.

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
