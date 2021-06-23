---
html: account_info.html
parent: account-methods.html
blurb: Get basic data about an account.
---
# account_info
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountInfo.cpp "Source")

`account_info`命令检索有关帐户、其活动和XRP余额的信息。检索到的所有信息都与特定版本的分类帐有关。

## Request Format

帐户信息请求的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "command": "account_info",
  "account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
  "strict": true,
  "ledger_index": "current",
  "queue": true
}
```

*JSON-RPC*

```json
{
    "method": "account_info",
    "params": [
        {
            "account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "strict": true,
            "ledger_index": "current",
            "queue": true
        }
    ]
}
```

*Commandline*

```sh
#Syntax: account_info account [ledger_index|ledger_hash] [strict]
rippled account_info rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn validated strict
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_info)

请求包含以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `account`      | String                     | 帐户的唯一标识符，通常是帐户的[地址][]。 |
| `ledger_hash`  | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串。 (参见 [Specifying Ledgers][]) |
| `ledger_index` | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串(参见[分类帐][]） |
| `queue`        | Boolean                    | _(可选)_ 如果`true`，并且启用了[FeeEscalation amendment][]，则还将返回有关与此帐户关联的排队事务的统计信息。只能在查询当前未结分类账的数据时使用。 无法从处于[报告模式][]的服务器获得。 |
| `signer_lists` | Boolean                    | _(可选)_ 如果`true`，并且启用了[MultiSign amendment][]，则还将返回与此帐户关联的任何[SignerList objects](signerlist.html)。 |
| `strict`       | Boolean                    | _(可选)_ 如果`true`，则`account`字段只接受公钥或XRP分类帐地址。否则，`帐户`可以是机密或密码短语（不推荐）。默认值为`false`。 |

下列字段已弃用，不应提供：`ident`，`ledger`。

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 5,
    "status": "success",
    "type": "response",
    "result": {
        "account_data": {
            "Account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "Balance": "999999999960",
            "Flags": 8388608,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "4294BEBE5B569A18C0A2702387C9B1E7146DC3A5850C1E87204951C6FDAA4C42",
            "PreviousTxnLgrSeq": 3,
            "Sequence": 6,
            "index": "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
        },
        "ledger_current_index": 4,
        "queue_data": {
            "auth_change_queued": true,
            "highest_sequence": 10,
            "lowest_sequence": 6,
            "max_spend_drops_total": "500",
            "transactions": [
                {
                    "auth_change": false,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 6
                },
                ... (trimmed for length) ...
                {
                    "LastLedgerSequence": 10,
                    "auth_change": true,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 10
                }
            ],
            "txn_count": 5
        },
        "validated": false
    }
}
```

*JSON-RPC*

```json
{
    "result": {
        "account_data": {
            "Account": "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
            "Balance": "999999999960",
            "Flags": 8388608,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 0,
            "PreviousTxnID": "4294BEBE5B569A18C0A2702387C9B1E7146DC3A5850C1E87204951C6FDAA4C42",
            "PreviousTxnLgrSeq": 3,
            "Sequence": 6,
            "index": "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
        },
        "ledger_current_index": 4,
        "queue_data": {
            "auth_change_queued": true,
            "highest_sequence": 10,
            "lowest_sequence": 6,
            "max_spend_drops_total": "500",
            "transactions": [
                {
                    "auth_change": false,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 6
                },
                ... (trimmed for length) ...
                {
                    "LastLedgerSequence": 10,
                    "auth_change": true,
                    "fee": "100",
                    "fee_level": "2560",
                    "max_spend_drops": "100",
                    "seq": 10
                }
            ],
            "txn_count": 5
        },
        "status": "success",
        "validated": false
    }
}
```

*Commandline*

```json
{
   "result" : {
      "account_data" : {
         "Account" : "rG1QQv2nh2gr7RCZ1P8YYcBUKCCN633jCn",
         "Balance" : "9986",
         "Flags" : 1114112,
         "LedgerEntryType" : "AccountRoot",
         "OwnerCount" : 0,
         "PreviousTxnID" : "0705FE3F52057924C288296EF0EBF668E0C1A3646FBA8FAF9B73DCC0A797B4B2",
         "PreviousTxnLgrSeq" : 51948740,
         "RegularKey" : "rhLkGGNZdjSpnHJw4XAFw1Jy7PD8TqxoET",
         "Sequence" : 192220,
         "index" : "92FA6A9FC8EA6018D5D16532D7795C91BFB0831355BDFDA177E86C8BF997985F"
      },
      "ledger_hash" : "8169428EDF7F046F817CE44F5F1DF23AD9FAEFFA2CBA7645C3254D66AA79B46E",
      "ledger_index" : 56843712,
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，结果包含请求的帐户、其数据及其应用的分类帐，如下字段所示:

| `Field`                | Type    | Description                               |
|:-----------------------|:--------|:------------------------------------------|
| `account_data`         | Object  | [AccountRoot ledger object](accountroot.html)与此帐户的信息一起存储在分类帐中。 |
| `signer_lists`         | Array   | _(除非指定的请求`signer_list`并且至少有一个SignerList与帐户关联，否则忽略。)_ [SignerList ledger objects](signerlist.html)的数组与[Multi-Signing](multi-signing.html)的此帐户关联。由于帐户最多只能拥有一个SignerList，因此此数组必须只有一个成员（如果存在）。 |
| `ledger_current_index` | Integer | _(如果改为提供`ledger_index`，则省略)_ 当前进行中分类帐的[分类帐索引][]，检索此信息时使用该索引。 |
| `ledger_index`         | Integer | _(如果改为提供`ledger_current_index`，则省略)_ 检索此信息时使用的分类帐版本的[分类帐索引][]。此信息不包含比此更新的分类帐版本的任何更改。 |
| `queue_data`           | Object  | _(除非将`queue`指定为`true`并查询当前未结分类帐，否则忽略。)_ 有关此帐户发送的[queued transactions](transaction-cost.html#queued-transactions)的信息。此信息描述本地`Ripple`服务器的状态，该服务器可能与[peer-to-peer XRP Ledger network](consensus-network.html)中的其他服务器不同。某些字段可能会被忽略，因为值是由排队机制延迟计算的。 |
| `validated`            | Boolean | 如果此数据来自已验证的分类帐版本，则为True；如果省略或设置为false，则此数据不是最终数据。 |

`queue_data`参数（如果存在）包含以下字段:

| `Field`                 | Type    | Description                              |
|:------------------------|:--------|:-----------------------------------------|
| `txn_count`             | Integer | 来自此地址的排队事务数。 |
| `auth_change_queued`    | Boolean | (可以省略) 队列中的事务是否更改此地址的[授权事务的方式](transaction-basics.html#authorizing-transactions)。如果`为真`，则在该事务被执行或从队列中删除之前，此地址不能对进一步的事务进行排队。 |
| `lowest_sequence`       | Integer | (可以省略) 此地址排队的事务中最低的[Sequence Number][]。|
| `highest_sequence`      | Integer | (可以省略) 此地址排队的事务中最高的[Sequence Number][]。|
| `max_spend_drops_total` | String  | (可以省略) 如果队列中的每个事务都使用尽可能多的XRP，则可以从此地址借记的[drops of XRP][]的整数金额。|
| `transactions`          | Array   | (可以省略) 有关此地址中每个排队事务的信息。|

`transactions`数组中的每个对象（如果存在）可以包含以下任何或所有字段:

| `Field`           | Type    | Description                                    |
|:------------------|:--------|:-----------------------------------------------|
| `auth_change`     | Boolean | 此事务是否更改此地址的[授权事务的方式](transaction-basics.html#authorizing-transactions). |
| `fee`             | String  | 此事务的[事务处理成本](transaction-cost.html)，以[drops of XRP][]为单位。 |
| `fee_level`       | String  | 相对于此类型交易的最低成本，此交易的交易成本，单位为[费用级别][]。 |
| `max_spend_drops` | String  | 此事务可以发送或销毁的最大金额为[XRP, in drops][]次。 |
| `seq`             | Integer | 此事务的[Sequence Number][]。 |

## Possible Errors

* 任何[通用错误类型][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。 例如，请求将`queue`指定为`true`，但指定的`ledger\u索引`不是当前打开的分类帐。
* `actNotFound` - 请求的`帐户`字段中指定的地址与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。

[fee levels]: transaction-cost.html#fee-levels
{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
