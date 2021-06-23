---
html: account_channels.html
parent: account-methods.html
blurb: Get a list of payment channels where the account is the source of the channel.
---
# account_channels
[[Source]](https://github.com/ripple/rippled/blob/release/src/ripple/rpc/handlers/AccountChannels.cpp "Source")

_(Added by the [PayChan amendment][]. [New in: rippled 0.33.0][])_

`account_channels`方法返回有关帐户付款渠道的信息。这只包括指定帐户是通道源而不是目标的通道(频道的“源”和“所有者”是相同的。）检索到的所有信息都与特定版本的分类帐有关。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "command": "account_channels",
  "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
  "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "account_channels",
    "params": [{
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "ledger_index": "validated"
    }]
}
```

*Commandline*

```bash
#Syntax: account_channels <account> [<destination_account>] [<ledger>]
rippled account_channels rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn validated
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| Field                 | Type                       | Description             |
|:----------------------|:---------------------------|:------------------------|
| `account`             | String                     | 帐户的唯一标识符，通常是帐户的[地址][]。该请求返回频道，其中该帐户是频道的所有者/源。 |
| `destination_account` | String                     | _(可选)_ 帐户的唯一标识符，通常是帐户的[地址][]。如果提供，则将结果筛选到目的地为此帐户的支付通道。 |
| `ledger_hash`         | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[Specifying Ledgers][]) |
| `ledger_index`        | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串(参见[Specifying Ledgers][] |
| `limit`               | Integer                    | _(可选)_ 限制要检索的事务数。不能小于10或大于400。默认值为200。|
| `marker`              | [Marker][]                 | _(可选)_ 来自上一个分页响应的值。继续检索停止响应的数据。 |

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
    "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
    "channels": [
      {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "amount": "100000000",
        "balance": "1000000",
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "destination_tag": 20170428,
        "expiration": 547073182,
        "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
        "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
        "settle_delay": 86400
      }
    ],
    "ledger_hash": "F168208EECDAA57DDAC32780CDD8330FA3E89F0E84D27A9052AA2F88681EBD08",
    "ledger_index": 37230642,
    "validated": true
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "amount": "100000000",
            "balance": "0",
            "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "destination_tag": 20170428,
            "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
            "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "settle_delay": 86400
        }],
        "ledger_hash": "B9D3D80EDF4083A06B2D51202E0BFB63C46FC0985E015D06767C21A62853BF6D",
        "ledger_index": 37230600,
        "status": "success",
        "validated": true
    }
}
```

*Commandline*

```json
200 OK

{
    "result": {
        "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
        "channels": [{
            "account": "rN7n7otQDd6FczFgLdSqtcsAUxDkw6fzRH",
            "amount": "100000000",
            "balance": "0",
            "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
            "destination_account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "destination_tag": 20170428,
            "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
            "public_key_hex": "023693F15967AE357D0327974AD46FE3C127113B1110D6044FD41E723689F81CC6",
            "settle_delay": 86400
        }],
        "ledger_hash": "B9D3D80EDF4083A06B2D51202E0BFB63C46FC0985E015D06767C21A62853BF6D",
        "ledger_index": 37230600,
        "status": "success",
        "validated": true
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| Field          | Type                     | Description                      |
|:---------------|:-------------------------|:---------------------------------|
| `account`      | String                   | 支付渠道的来源/所有者的地址。这与请求的`帐户`字段相对应。 |
| `channels`     | Array of Channel Objects | 此`帐户`拥有的支付渠道。 |
| `ledger_hash`  | String                   | 用于生成此响应的分类帐版本的标识[哈希][]。 |
| `ledger_index` | Number                   | 用于生成此响应的分类帐版本的[分类帐索引][]。[New in: rippled 0.90.0][] |
| `validated`    | Boolean                  | _(可以省略)_ 如果为`true`，则此响应中的信息来自已验证的分类帐版本。否则，信息可能会更改。  |
| `limit`        | Number                   | _(可以省略)_ 此请求实际返回的通道对象数限制。 |
| `marker`       | [Marker][]               | _(可以省略)_ 服务器定义的分页值。将此消息传递给下一个调用以继续获取此调用中断的结果。此页之后没有其他页时省略。 |

每个通道对象都有以下字段:

| Field                 | Type             | Description                       |
|:----------------------|:-----------------|:----------------------------------|
| `account`             | String           | 频道的所有者，作为[地址][]。 |
| `amount`              | String           | 分配给此通道的[XRP的总量，分为][]滴。 |
| `balance`             | String           | 自所用分类帐版本起，从该渠道支付的[XRP, in drops][](您可以通过从`amount`中减去`balance`来计算通道中剩余的XRP量。） |
| `channel_id`          | String           | 此通道的唯一ID，作为64个字符的十六进制字符串。这也是分类帐状态数据中通道对象]（paychannel.html#paychannel id格式）的[ID。 |
| `destination_account` | String           | 频道的目标帐户，作为[地址][]。只有此帐户才能在通道打开时接收XRP。|
| `settle_delay`        | Unsigned Integer | 支付通道的所有者请求关闭支付通道后，支付通道必须保持打开的秒数。 |
| `public_key`          | String           | _(可以省略)_ XRP分类账的[base58][]格式的支付渠道的公钥。必须使用匹配的密钥对兑换针对此通道的签名声明。 |
| `public_key_hex`      | String           | _(可以省略)_ 支付通道的十六进制格式公钥（如果在通道创建时指定了公钥）。必须使用匹配的密钥对兑换针对此通道的签名声明。 |
| `expiration`          | Unsigned Integer | _(可以省略)_ 此通道设置为过期时的时间，[从Ripple Epoch开始，以秒为单位][]。此过期日期是可变的。如果这是在最近验证的分类帐的关闭时间之前，则通道将过期。|
| `cancel_after`        | Unsigned Integer | _(可以省略)_ 如果在通道创建时指定了一个时间，则此通道的不可变过期时间[从Ripple Epoch开始，以秒为单位][]。如果这是在最近验证的分类帐的关闭时间之前，则通道将过期。 |
| `source_tag`          | Unsigned Integer | _(可以省略)_ 一个32位无符号整数，如果在通道创建时指定了一个，则用作通过此支付通道进行支付的[源标记](become-an-xrp-ledger-gateway.html#source-and-destination-tags)。这表示支付渠道的发起人或源帐户的其他目的。按照惯例，如果您从该渠道退回付款，则应在退货付款的`DestinationTag`中指定此值。|
| `destination_tag`     | Unsigned Integer | _(可以省略)_ 如果在通道创建时指定了一个32位无符号整数，则该整数将用作通过此通道付款的[目标标记](become-an-xrp-ledger-gateway.html#source-and-destination-tags)。这表明支付渠道的受益人或其他目的地帐户。|

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 请求的`帐户`字段中指定的地址与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger哈希`或`ledger索引`指定的分类帐不存在，或者确实存在，但服务器没有它。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
