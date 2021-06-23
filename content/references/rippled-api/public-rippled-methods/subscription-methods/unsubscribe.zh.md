---
html: unsubscribe.html
parent: subscription-methods.html
blurb: Stop listening for updates about a particular subject.
---
# unsubscribe
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Unsubscribe.cpp "Source")

`unsubscribe` 命令指示服务器停止发送特定订阅或一组订阅的消息。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "Unsubscribe a lot of stuff",
    "command": "unsubscribe",
    "streams": ["ledger","server","transactions","transactions_proposed"],
    "accounts": ["rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1"],
    "accounts_proposed": ["rrpNnNLKrartuEqfJGpqyDwPj1AFPg9vn1"],
    "books": [
        {
            "taker_pays": {
                "currency": "XRP"
            },
            "taker_gets": {
                "currency": "USD",
                "issuer": "rUQTpMqAF5jhykj4FExVeXakrZpiKF6cQV"
            },
            "both": true
        }
    ]
}
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#unsubscribe)

请求中的参数的指定与[subscribe method][]的参数几乎完全相同，只是它们用于定义要终止的订阅。

| `Field`             | Type  | Description                                    |
|:--------------------|:------|:-----------------------------------------------|
| `streams`           | Array | _(Optional)_ Array of string names of generic streams to unsubscribe from, including `ledger`, `server`, `transactions`, and `transactions_proposed`. |
| `accounts`          | Array | _(Optional)_ Array of unique account addresses to stop receiving updates for, in the XRP Ledger's [base58][] format. (This only stops those messages if you previously subscribed to those accounts specifically. You cannot use this to filter accounts out of the general transactions stream.) |
| `accounts_proposed` | Array | _(Optional)_ Like `accounts`, but for `accounts_proposed` subscriptions that included not-yet-validated transactions. |
| `books`             | Array | _(Optional)_ Array of objects defining order books to unsubscribe from, as explained below. |

The `rt_accounts` and `url` parameters, and the `rt_transactions` stream name, are deprecated and may be removed without further notice.

The objects in the `books` array are defined almost like the ones from subscribe, except that they don't have all the fields. The fields they have are as follows:

| `Field`      | Type    | Description                                         |
|:-------------|:--------|:----------------------------------------------------|
| `taker_gets` | Object  | Specification of which currency the account taking the offer would receive, as an object with `currency` and `issuer` fields (omit issuer for XRP), like [currency amounts][Currency Amount]. |
| `taker_pays` | Object  | Specification of which currency the account taking the offer would pay, as an object with `currency` and `issuer` fields (omit issuer for XRP), like [currency amounts][Currency Amount]. |
| `both`       | Boolean | (Optional, defaults to false) If true, remove a subscription for both sides of the order book. |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "Unsubscribe a lot of stuff",
    "result": {},
    "status": "success",
    "type": "response"
}
```

<!-- MULTICODE_BLOCK_END -->

The response follows the [standard format][], with a successful result containing no fields.

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `noPermission` - The request included the `url` field, but you are not connected as an admin.
* `malformedStream` - The `streams` field of the request is not formatted properly.
* `malformedAccount` - One of the addresses in the `accounts` or `accounts_proposed` fields of the request is not a properly-formatted XRP Ledger address.
    * **Note:** You _can_ subscribe to the stream of an address that does not yet have an entry in the global ledger to get a message when that address becomes funded.
* `srcCurMalformed` - One or more `taker_pays` sub-fields of the `books` field in the request is not formatted properly.
* `dstAmtMalformed` - One or more `taker_gets` sub-fields of the `books` field in the request is not formatted properly.
* `srcIsrMalformed` - The `issuer` field of one or more `taker_pays` sub-fields of the `books` field in the request is not valid.
* `dstIsrMalformed` - The `issuer` field of one or more `taker_gets` sub-fields of the `books` field in the request is not valid.
* `badMarket` - `books`字段中不存在一个或多个所需的订单簿；例如，为自己兑换一种货币。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
