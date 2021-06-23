---
html: account_lines.html
parent: account-methods.html
blurb: Get info about an account's trust lines.
---
# account_lines
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountLines.cpp "Source")

`account_lines`方法返回有关帐户的信托行的信息，包括所有非XRP货币和资产的余额。检索到的所有信息都与特定版本的分类帐有关。

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "command": "account_lines",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
}
```

*JSON-RPC*

```json
{
    "method": "account_lines",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59"
        }
    ]
}
```

*Commandline*

```sh
#Syntax: account_lines <account> [<peer>] [<ledger_index>|<ledger_hash>]
rippled account_lines r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_lines)

请求接受以下参数:

| `Field`        | Type                                       | Description    |
|:---------------|:-------------------------------------------|:---------------|
| `account`      | String                                     | 帐户的唯一标识符，通常是帐户的[地址][]。 |
| `ledger_hash`  | String                                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[Specifying Ledgers][]) |
| `ledger_index` | String or Unsigned Integer                 | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串(参见[Specifying Ledgers][]） |
| `peer`         | String                                     | _(可选)_ 第二个帐户的[地址][]。如果提供，只显示连接两个帐户的信任线。|
| `limit`        | Integer                                    | (可选, 默认值不同) 限制要检索的信任行数。服务器不需要接受此值。必须在10到400之间。  |
| `marker`       | [Marker][] | _(可选)_ 来自上一个分页响应的值。继续检索停止响应的数据。  |

以下参数已弃用，可能会被删除，恕不另行通知: `ledger` 和 `peer_index`.

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
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                "balance": "3.497605752725159",
                "currency": "USD",
                "limit": "5",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
            }
        ]
    }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "lines": [
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "ASP",
                "limit": "0",
                "limit_peer": "10",
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                "balance": "0",
                "currency": "XAU",
                "limit": "0",
                "limit_peer": "0",
                "no_ripple": true,
                "no_ripple_peer": true,
                "quality_in": 0,
                "quality_out": 0
            },
            {
                "account": "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
                "balance": "0",
                "currency": "015841551A748AD2C1F76FF6ECB0CCCD00000000",
                "limit": "10.01037626125837",
                "limit_peer": "0",
                "no_ripple": true,
                "quality_in": 0,
                "quality_out": 0
            }
        ],
        "status": "success"
    }
}
```

*Commandline*
```json
{
   "result" : {
      "account" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "ledger_current_index" : 56867265,
      "lines" : [
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "ASP",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "XAU",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
            "balance" : "5",
            "currency" : "USD",
            "limit" : "5",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
            "balance" : "481.992867407479",
            "currency" : "MXN",
            "limit" : "1000",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "balance" : "0.793598266778297",
            "currency" : "EUR",
            "limit" : "1",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
            "balance" : "0",
            "currency" : "CNY",
            "limit" : "3",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
            "balance" : "1.336889190631542",
            "currency" : "DYM",
            "limit" : "3",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "0.3488146605801446",
            "currency" : "CHF",
            "limit" : "0",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "0",
            "currency" : "BTC",
            "limit" : "3",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "11.68225001668339",
            "currency" : "USD",
            "limit" : "5000",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rpgKWEmNqSDAGFhy5WDnsyPqfQxbWxKeVd",
            "balance" : "-0.00111",
            "currency" : "BTC",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rBJ3YjwXi2MGbg7GVLuTXUWQ8DjL7tDXh4",
            "balance" : "-0.0008744482690504699",
            "currency" : "BTC",
            "limit" : "0",
            "limit_peer" : "10",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "1",
            "limit_peer" : "0",
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
            "balance" : "9.07619790068559",
            "currency" : "CNY",
            "limit" : "100",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "7.292695098901099",
            "currency" : "JPY",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "balance" : "0",
            "currency" : "AUX",
            "limit" : "0",
            "limit_peer" : "0",
            "no_ripple" : true,
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
            "balance" : "0.0004557360418801623",
            "currency" : "USD",
            "limit" : "1",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            "balance" : "12.41688780720394",
            "currency" : "EUR",
            "limit" : "100",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rfF3PNkwkq1DygW2wum2HK3RGfgkJjdPVD",
            "balance" : "35",
            "currency" : "USD",
            "limit" : "500",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rwUVoVMSURqNyvocPCcvLu3ygJzZyw8qwp",
            "balance" : "-5",
            "currency" : "JOE",
            "limit" : "0",
            "limit_peer" : "50",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "0",
            "limit_peer" : "100",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
            "balance" : "0",
            "currency" : "JOE",
            "limit" : "0",
            "limit_peer" : "100",
            "no_ripple_peer" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
            "balance" : "0",
            "currency" : "015841551A748AD2C1F76FF6ECB0CCCD00000000",
            "limit" : "10.01037626125837",
            "limit_peer" : "0",
            "no_ripple" : true,
            "quality_in" : 0,
            "quality_out" : 0
         },
         {
            "account" : "rEhDDUUNxpXgEHVJtC2cjXAgyx5VCFxdMF",
            "balance" : "0",
            "currency" : "USD",
            "limit" : "0",
            "limit_peer" : "1",
            "quality_in" : 0,
            "quality_out" : 0
         }
      ],
      "status" : "success",
      "validated" : false
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含帐户地址和信任行对象数组。具体来说，result对象包含以下字段:

| `Field`                | Type                       | Description            |
|:-----------------------|:---------------------------|:-----------------------|
| `account`              | String                     | 此请求对应的帐户的唯一[地址][]。这是用于信托额度的"透视帐户"。 |
| `lines`                | Array                      | 信任线对象的数组，如下所述。如果信任行的数目很大，则一次最多只能返回`limit`条。 |
| `ledger_current_index` | Integer - [Ledger Index][] | _(如果提供了`ledger_hash`或`ledger_index`，则省略)_ 当前打开的分类帐的分类帐索引，检索此信息时使用该索引。 |
| `ledger_index`         | Integer - [Ledger Index][] | _(如果改为提供`ledger_current_index`，则省略)_检索此数据时使用的分类帐版本的分类帐索引。 |
| `ledger_hash`          | String - [Hash][]          | _(可以省略)_ 识别哈希值是检索此数据时使用的分类帐版本。  |
| `marker`               | [Marker][]                 | 服务器定义的值，指示响应已分页。将此消息传递给下一个呼叫以继续此呼叫中断的位置。此页之后没有其他页时省略。[] |

每个信任行对象都有以下字段的一些组合:

| `Field`          | Type             | Description                            |
|:-----------------|:-----------------|:---------------------------------------|
| `account`        | String           | 此信托额度对应方的唯一[地址][]。 |
| `balance`        | String           | 表示当前针对此行持有的数字余额。正平衡意味着透视账户持有价值；负余额意味着透视账户欠值。 |
| `currency`       | String           | [货币代码][]，标识此信任行可以持有的货币。 |
| `limit`          | String           | 此帐户愿意欠对等帐户的给定货币的最大金额 |
| `limit_peer`     | String           | 对方账户愿意欠对方账户的最大货币金额 |
| `quality_in`     | Unsigned Integer | 该账户对该信托额度的收入余额进行估值的比率，即每10亿个单位中该值的比率(例如，值5亿表示0.5:1的比率。）作为特例，0被视为1:1的比率。 |
| `quality_out`    | Unsigned Integer | 该账户对该信托额度上的传出余额进行估值的比率，以每10亿个单位中该值的比率表示(例如，值5亿表示0.5:1的比率。）作为特例，0被视为1:1的比率。 |
| `no_ripple`      | Boolean          | _(可以省略)_ 如果`true`，则此帐户已为此信任行启用[No Ripple flag](rippling.html)。如果存在且`false`，则此帐户已禁用“无Ripple”标志，但由于该帐户还启用了默认Ripple标志，因此该标志不被视为默认状态]（RippleEstate.html#用于所有者保留）。如果省略，帐户将禁用此信任行的“无涟漪”标志，并禁用默认涟漪。|
| `no_ripple_peer` | Boolean          | _(可以省略)_ 如果`true`，则对等帐户已为此信任行启用[No Ripple标志]（rippling.html）。如果存在且`false`，则此帐户已禁用“无Ripple”标志，但由于该帐户还启用了默认Ripple标志，因此该标志不被视为[the default state](ripplestate.html#contributing-to-the-owner-reserve)。如果省略，帐户将禁用此信任行的“无涟漪”标志，并禁用默认涟漪。 |
| `authorized`     | Boolean          | _(可以省略)_ 如果为`true`, 此帐户已[授权此信任行](authorized-trust-lines.html)。默认值为`false`。|
| `peer_authorized`| Boolean          | _(可以省略)_ 如果`true`，则对等帐户已[授权此信任行](authorized-trust-lines.html)。默认值为`false`。|
| `freeze`         | Boolean          | _(可以省略)_ 如果`true`，则此帐户已[Frozed](freezes.html)此信任行。默认值为`false`。 |
| `freeze_peer`    | Boolean          | _(可以省略)_ 如果`true`，则对等帐户已冻结[]（freezes.html）此信任行。默认值为`false`。 |

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 在请求的`帐户`字段中指定的[地址][]与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。
* `actMalformed` - 如果提供的`marker`字段不可接受。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
