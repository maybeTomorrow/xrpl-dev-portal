---
html: account_offers.html
parent: account-methods.html
blurb: Get info about an account's currency exchange offers.
---
# account_offers
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountOffers.cpp "Source")

`account_offers` 方法检索由给定的[account](accounts.html) 生成的 [offers](offers.html) 的列表，该列表在特定的[ledger version](ledgers.html).

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 9,
  "command": "account_offers",
  "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM"
}
```

*JSON-RPC*

```json
{
    "method": "account_offers",
    "params": [
        {
            "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM"
        }
    ]
}
```

*Commandline*

```sh
#Syntax: account_offers account [ledger_index] [strict]
rippled account_offers rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM current strict
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_offers)

A request can include the following parameters:

| `Field`        | Type                        | Description                   |
|:---------------|:----------------------------|:------------------------------|
| `account`      | String                      | A unique identifier for the account, most commonly the account's [Address][]. |
| `ledger`       | Unsigned Integer, or String | _(**Deprecated**, Optional)_ A unique identifier for the ledger version to use, such as a ledger index, a hash, or a shortcut such as "validated". |
| `ledger_hash`  | String - [Hash][]           | _(Optional)_ A 20-byte hex string identifying the ledger version to use. |
| `ledger_index` | Number - [Ledger Index][]   | (Optional, defaults to `current`) The [ledger index][] of the ledger to use, or "current", "closed", or "validated" to select a ledger dynamically. (See [Specifying Ledgers][]) |
| `limit`        | Integer                     | (Optional, default varies) Limit the number of transactions to retrieve. The server is not required to honor this value. Must be within the inclusive range 10 to 400. [New in: rippled 0.26.4][] |
| `marker`       | [Marker][]                  | _(Optional)_ Value from a previous paginated response. Resume retrieving data where that response left off. [New in: rippled 0.26.4][] |
| `strict`       | Boolean                    | _(Optional)_ If `true`, then the `account` field only accepts a public key or XRP Ledger address. Otherwise, `account` can be a secret or passphrase (not recommended). The default is `false`. |

The following parameter is deprecated and may be removed without further notice: `ledger`.

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 9,
  "status": "success",
  "type": "response",
  "result": {
    "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM",
    "ledger_current_index": 18539550,
    "offers": [
      {
        "flags": 0,
        "quality": "0.00000000574666765650638",
        "seq": 6577664,
        "taker_gets": "33687728098",
        "taker_pays": {
          "currency": "EUR",
          "issuer": "rhub8VRN55s94qWKDv6jmDy1pUykJzF3wq",
          "value": "193.5921774819578"
        }
      },
      {
        "flags": 0,
        "quality": "7989247009094510e-27",
        "seq": 6572128,
        "taker_gets": "2361918758",
        "taker_pays": {
          "currency": "XAU",
          "issuer": "rrh7rf1gV2pXAoqA8oYbpHd8TKv5ZQeo67",
          "value": "0.01886995237307572"
        }
      },
      ... trimmed for length ...
    ],
    "validated": false
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "account": "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM",
        "ledger_current_index": 18539596,
        "offers": [{
            "flags": 0,
            "quality": "0.000000007599140009999998",
            "seq": 6578020,
            "taker_gets": "29740867287",
            "taker_pays": {
                "currency": "USD",
                "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                "value": "226.0050145327418"
            }
        }, {
            "flags": 0,
            "quality": "7989247009094510e-27",
            "seq": 6572128,
            "taker_gets": "2361918758",
            "taker_pays": {
                "currency": "XAU",
                "issuer": "rrh7rf1gV2pXAoqA8oYbpHd8TKv5ZQeo67",
                "value": "0.01886995237307572"
            }
        }, {
            "flags": 0,
            "quality": "0.00000004059594001318974",
            "seq": 6576905,
            "taker_gets": "3892952574",
            "taker_pays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "158.0380691682966"
            }
        },

        ...

        ],
        "status": "success",
        "validated": false
    }
}
```
*Commandline*

```json
{
   "result" : {
      "account" : "rpP2JgiMyTF5jR5hLG3xHCPi1knBb1v9cM",
      "ledger_current_index" : 57110969,
      "offers" : [
         {
            "flags" : 0,
            "quality" : "1499850014.892974",
            "seq" : 7916201,
            "taker_gets" : {
               "currency" : "BCH",
               "issuer" : "rcyS4CeCZVYvTiKcxj6Sx32ibKwcDHLds",
               "value" : "0.5268598580881351"
            },
            "taker_pays" : "790210766"
         }
      ],
      "status" : "success",
      "validated" : false
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                      | Description             |
|:-----------------------|:--------------------------|:------------------------|
| `account`              | String                    | 唯一的[地址][]，用于标识发出报价的帐户 |
| `offers`               | Array                     | 对象数组，其中每个对象表示此帐户在请求的分类帐版本中未完成的报价。如果提供的数量很大，一次最多只能返回`个限制`。 |
| `ledger_current_index` | Number - [Ledger Index][] | _(如果提供了`ledger_hash`或`ledger_index`，则省略)_ 当前进行中分类帐版本的分类帐索引，在检索此数据时使用。|
| `ledger_index`         | Number - [Ledger Index][] | _(如果提供了`ledger_current_index`，则省略)_ The ledger index of the ledger version that was used when retrieving this data, as requested. [New in: rippled 0.26.4-sp1][] |
| `ledger_hash`          | String - [Hash][]         | _(可以省略)_ 检索此数据时使用的分类帐版本的标识哈希。 |
| `marker`               | [Marker][]                | _(可以省略)_ 服务器定义的值，指示响应已分页。将此消息传递给下一个呼叫以继续此呼叫中断的位置。此页之后没有信息页时省略。 |


每个offer对象包含以下字段:

| `Field`      | Type             | Description                                |
|:-------------|:-----------------|:-------------------------------------------|
| `flags`      | Unsigned integer | 将此提供项的选项设置为位标志。 |
| `seq`        | Unsigned integer | 创建此条目的事务的序列号(事务[序号]（basic data types.html#account sequence）与帐户相关。）|
| `taker_gets` | String or Object | 接受报价的帐户收到的金额，以表示XRP中金额的字符串或货币规范对象的形式(请参阅[Specifying Currency Amounts][Currency Amount]） |
| `taker_pays` | String or Object | 接受报价的帐户提供的金额，以表示XRP中金额的字符串或货币规范对象的形式。(请参阅[Specifying Currency Amounts][Currency Amount]） |
| `quality`    | String           | 报价的汇率，即原始的`taker_pays`除以原始的`taker_gets`。 When executing offers, the offer with the most favorable (lowest) quality is consumed first; offers with the same quality are executed from oldest to newest.|
| `expiration` | Unsigned integer | (可以省略) A time after which this offer is considered unfunded, as the number of [seconds since the Ripple Epoch][]. See also: [Offer Expiration](offers.html#offer-expiration). [New in: rippled 0.30.1][] |

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 在请求的`帐户`字段中指定的[地址][]与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。
* `actMalformed` - 提供的`标记符`字段不正确。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
