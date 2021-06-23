---
html: ledger.html # Watch carefully for clashes w/ this filename
parent: ledger-methods.html
blurb: Get info about a ledger version.
---
# ledger
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerHandler.cpp "Source")

检索有关公众的信息 [ledger](ledgers.html).

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 14,
    "command": "ledger",
    "ledger_index": "validated",
    "full": false,
    "accounts": false,
    "transactions": false,
    "expand": false,
    "owner_funds": false
}
```

*JSON-RPC*

```json
{
    "method": "ledger",
    "params": [
        {
            "ledger_index": "validated",
            "accounts": false,
            "full": false,
            "transactions": false,
            "expand": false,
            "owner_funds": false
        }
    ]
}
```

*Commandline*

```
#Syntax: ledger ledger_index|ledger_hash [full|tx]
# "full" is equivalent to "full": true
# "tx" is equivalent to "transactions": true
rippled ledger validated
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#ledger)
请求可以包含以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `ledger_hash`  | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串。 (See [Specifying Ledgers][]). |
| `ledger_index` | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (See [Specifying Ledgers][]) |
| `full`         | Boolean                    | _(可选)_ **Admin required** 如果为`true`，则返回整个分类帐的完整信息。 如果未指定分类帐版本，则忽略。默认为`false`。 (Equivalent to enabling `transactions`, `accounts`, and `expand`.) **Caution:** 这是一个非常大的数据量——大约几百兆字节！ |
| `accounts`     | Boolean                    | _(可选)_ **Admin required.** 如果为`true`，则返回分类帐中的帐户信息。 如果未指定分类帐版本，则忽略。默认为`false`。 **Caution:** 这将返回大量的数据！ |
| `transactions` | Boolean                    | _(可选)_ 如果为`true`，则返回指定分类帐版本中的交易记录信息。 默认为`false`。如果未指定分类帐版本，则忽略。|
| `expand`       | Boolean                    | _(可选)_ 为事务/帐户信息提供完整的JSON格式的信息，而不仅仅是散列。默认值为`false`。除非您请求事务、帐户或两者，否则将忽略。 |
| `owner_funds`  | Boolean                    | _(可选)_ 如果`true`，则在响应中OfferCreate事务的元数据中包含`owner_funds`字段。默认为`false`。除非包含事务并且`expand`为true，否则将忽略。 |
| `binary`       | Boolean                    | _(可选)_ 如果`true`和`transactions`和`expand`也都是`true`，则返回二进制格式（十六进制字符串）而不是JSON格式的事务信息。  |
| `queue`        | Boolean                    | _(可选)_ 如果`true`，并且命令正在请求`current`分类帐，则在结果中包含[排队事务](transaction-cost.html#queued-transactions)数组。



## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 14,
  "result": {
    "ledger": {
      "accepted": true,
      "account_hash": "53BD4650A024E27DEB52DBB6A52EDB26528B987EC61C895C48D1EB44CEDD9AD3",
      "close_flags": 0,
      "close_time": 638329241,
      "close_time_human": "2020-Mar-24 01:40:41.000000000 UTC",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
      "ledger_hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
      "ledger_index": "54300932",
      "parent_close_time": 638329240,
      "parent_hash": "DF68B3BCABD31097634BABF0BDC87932D43D26E458BFEEFD36ADF2B3D94998C0",
      "seqNum": "54300932",
      "totalCoins": "99991024049648900",
      "total_coins": "99991024049648900",
      "transaction_hash": "50B3A8FE2C5620E43AA57564209AEDFEA3E868CFA2F6E4AB4B9E55A7A62AAF7B"
    },
    "ledger_hash": "1723099E269C77C4BDE86C83FA6415D71CF20AA5CB4A94E5C388ED97123FB55B",
    "ledger_index": 54300932,
    "validated": true
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
    "ledger": {
      "accepted": true,
      "account_hash": "B258A8BB4743FB74CBBD6E9F67E4A56C4432EA09E5805E4CC2DA26F2DBE8F3D1",
      "close_flags": 0,
      "close_time": 638329271,
      "close_time_human": "2020-Mar-24 01:41:11.000000000 UTC",
      "close_time_resolution": 10,
      "closed": true,
      "hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
      "ledger_hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
      "ledger_index": "54300940",
      "parent_close_time": 638329270,
      "parent_hash": "AE996778246BC81F85D5AF051241DAA577C23BCA04C034A7074F93700194520D",
      "seqNum": "54300940",
      "totalCoins": "99991024049618156",
      "total_coins": "99991024049618156",
      "transaction_hash": "FC6FFCB71B2527DDD630EE5409D38913B4D4C026AA6C3B14A3E9D4ED45CFE30D"
    },
    "ledger_hash": "3652D7FD0576BC452C0D2E9B747BDD733075971D1A9A1D98125055DEF428721A",
    "ledger_index": 54300940,
    "status": "success",
    "validated": true
  }
}
```

*Commandline*

```json
Loading: "/etc/opt/ripple/rippled.cfg"
2020-Mar-24 01:42:42.622264591 UTC HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "ledger" : {
         "accepted" : true,
         "account_hash" : "6B3101BE8F1431C5AC5B43D9731F1F3A747D24B3BEF89B687F0F3039E10EB65A",
         "close_flags" : 0,
         "close_time" : 638329360,
         "close_time_human" : "2020-Mar-24 01:42:40.000000000 UTC",
         "close_time_resolution" : 10,
         "closed" : true,
         "hash" : "C88A0EEC0E785A4C3E99F2A8B8EE0D7BDF3DE6C786C39B1B01547F6DAE5A4B7F",
         "ledger_hash" : "C88A0EEC0E785A4C3E99F2A8B8EE0D7BDF3DE6C786C39B1B01547F6DAE5A4B7F",
         "ledger_index" : "54300962",
         "parent_close_time" : 638329352,
         "parent_hash" : "96D2D70DC540BA4614A00C77FCFDED20E7D58AF3238E36655C38C407A56982A3",
         "seqNum" : "54300962",
         "totalCoins" : "99991024049218063",
         "total_coins" : "99991024049218063",
         "transaction_hash" : "47AC79011652D2A56AE04D3DD618C60A6669E3F94308C803554E890D2BD94481"
      },
      "ledger_hash" : "C88A0EEC0E785A4C3E99F2A8B8EE0D7BDF3DE6C786C39B1B01547F6DAE5A4B7F",
      "ledger_index" : 54300962,
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

返回响应如下 [standard format][], 成功的结果包含有关分类帐的信息，包括以下字段:

| `Field`                        | Type    | Description                       |
|:-------------------------------|:--------|:----------------------------------|
| `ledger`                       | Object  | 此分类帐的完整页眉数据。 |
| `ledger.account_hash`          | String  | 此分类帐中所有帐户状态信息的哈希，以十六进制表示 |
| `ledger.accountState`          | Array   | (除非要求，否则省略) 此分类帐中的所有[帐户状态信息]（分类帐数据格式.html）。 |
| `ledger.close_flags`           | Integer | 与关闭此分类帐有关的标志位图。当前，分类帐只有一个为`标记`定义的标记: **`sLCF_NoConsensusTime`** (value 1). 如果启用此标志，则表示验证器在正确的分类帐关闭时间方面存在冲突，但在其他方面构建相同的分类帐， 因此，他们宣布达成共识，而"同意在截止时间不同意"。在这种情况下，共识分类账包含一个`关闭时间`，该时间比上一个分类账的时间晚1秒。 (在这种情况下，没有正式的关闭时间，但实际的关闭时间可能比指定的`close_time`晚3-6秒。) |
| `ledger.close_time`            | Integer | 此分类帐关闭的时间，以[秒为单位，从Ripple Epoch][]开始 |
| `ledger.close_time_human`      | String  | 以人类可读的格式关闭此分类帐的时间。始终使用UTC时区。|
| `ledger.close_time_resolution` | Integer | 分类帐关闭时间四舍五入到这几秒钟之内。 |
| `ledger.closed`                | Boolean | 是否已关闭此分类帐 |
| `ledger.ledger_hash`           | String  | 整个分类帐的唯一标识哈希。 |
| `ledger.ledger_index`          | String  | 此分类帐的[分类帐索引][]，作为带引号的整数 |
| `ledger.parent_close_time`     | Integer | 上一个分类帐关闭的时间。 |
| `ledger.parent_hash`           | String  | 在此之前的分类帐的唯一标识哈希。 |
| `ledger.total_coins`           | String  | 网络中XRP丢弃的总数，以带引号的整数表示(这会随着交易成本的降低而降低。） |
| `ledger.transaction_hash`      | String  | 此分类帐中包含的交易信息的哈希，以十六进制表示 |
| `ledger.transactions`          | Array   | (除非要求，否则省略) 此分类帐版本中应用的交易记录。 默认情况下，成员是事务的标识[哈希][]字符串。 如果请求将`expand`指定为true，则成员将改为事务的完整表示形式，可以是JSON形式，也可以是二进制形式，具体取决于请求是否将`binary`指定为true。|
| `ledger_hash`                  | String  | 整个分类帐的唯一标识哈希。 |
| `ledger_index`                 | Number  | 此分类帐的[分类帐索引][]。 |
| `queue_data`                   | Array   | (除非设置`queue`，否则省略 ) 描述排队事务的对象数组，顺序与队列相同。 如果请求将`expand`指定为true，则根据请求是否将`binary`指定为true，成员包含事务的完整表示形式（JSON或二进制）。由[FeeEscalation amendment][]添加。|

 

`queue_data`数组的每个成员表示队列中的一个事务。此对象的某些字段可能会被忽略，因为它们尚未计算。该对象的字段如下:

| Field               | Value            | Description                         |
|:--------------------|:-----------------|:------------------------------------|
| `account`           | String           | 此排队事务的发件人的[地址][]。 |
| `tx`                | String or Object | 默认情况下，此字符串包含事务的[散列](basic-data-types.html#hashes) 。如果事务以二进制格式展开，则这是一个对象，其唯一字段为`tx_blob`，将事务的二进制形式包含为十进制字符串。如果事务以JSON格式展开，则这是一个包含[Transation对象](transaction-formats.html) 的对象，该对象在`hash`字段中包含事务的标识哈希。  |
| `retries_remaining` | Number           | 此事务在被删除之前可以重试多少次。 |
| `preflight_result`  | String           | 初步交易检查的初步结果。这总是`tesSUCCESS`。|
| `last_result`       | String           | _(可以省略)_ 如果此事务在获得[retriable (`ter`) result](ter-codes.html)后留在队列中，则这就是它获得的确切的`ter`结果代码。 |
| `auth_change`       | Boolean          | _(可以省略)_ 此事务是否更改此地址的[授权事务的方式](transaction-basics.html#authorizing-transactions). |
| `fee`               | String           | _(可以省略)_ [此事务的事务成本](transaction-cost.html) ，以[drops of XRP][]为单位. |
| `fee_level`         | String           | _(可以省略)_ 相对于此类型事务的最低成本，此事务的事务成本，单位为[费用级别][]. |
| `max_spend_drops`   | String           | _(可以省略)_ 最大数量为[XRP][]中，此事务可能会发送或销毁。 |

如果请求指定了`"owner_funds":true`和扩展事务, 响应的`metaData`对象每个[OfferCreate transaction][]中有一个字段`owner_funds`. 此字段的目的是便于跟踪每个新的已验证分类账的[报价状态](offers.html#lifecycle-of-an-offer)， 此字段的定义与[订购簿订阅流](subscribe.html#order-book-streams)中此字段的版本稍有不同:

| `Field`       | Value  | Description                                         |
|:--------------|:-------|:----------------------------------------------------|
| `owner_funds` | String | 在执行此分类帐中的所有事务后，发送此OfferCreate事务的`帐户`拥有的` TakeTargets`货币的数字金额。 这不会检查货币金额是否为[frozen](freezes.html). |

## Possible Errors

* 任何[通用错误类型][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。
* `noPermission` - 如果将`full`或`accounts`指定为true，但未作为管理员连接到服务器（通常，管理员需要连接到本地端口）。


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
