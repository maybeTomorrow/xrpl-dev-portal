---
html: noripple_check.html
parent: account-methods.html
blurb: Get recommended changes to an account's Default Ripple and No Ripple settings.
---
# noripple_check
[[Source]](https://github.com/ripple/rippled/blob/9111ad1a9dc37d49d085aa317712625e635197c0/src/ripple/rpc/handlers/NoRippleCheck.cpp "Source")

与建议的设置相比，`noripple_check`命令提供了一种快速检查[帐户的默认Ripple字段及其信任行](rippling.html)的无Ripple标志状态的方法。


## Request Format
请求格式的示例:

{% include '_snippets/no-cli-syntax.md' %}

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 0,
    "command": "noripple_check",
    "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
    "role": "gateway",
    "ledger_index": "current",
    "limit": 2,
    "transactions": true
}
```

*JSON-RPC*

```json
{
    "method": "noripple_check",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "current",
            "limit": 2,
            "role": "gateway",
            "transactions": true
        }
    ]
}
```

<!-- MULTICODE_BLOCK_END -->


请求包括以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `account`      | String                     | 帐户的唯一标识符，通常是帐户的地址。 |
| `role`         | String                     | 地址是指`网关`还是`用户`。建议取决于帐户的角色。发卡机构必须启用默认Ripple，并且必须在所有信任行上禁用No Ripple。用户应该禁用默认Ripple，并且应该在所有信任行上启用No Ripple。 |
| `transactions` | Boolean                    | _(可选)_ 如果`true`，则包含一个建议的[transactions]（transaction formats.html）数组作为JSON对象，您可以签名并提交该数组以解决问题。默认为false。 |
| `limit`        | Unsigned Integer           | _(可选)_ 要包含在结果中的最大信任线问题数。默认为300。 |
| `ledger_hash`  | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见 [Specifying Ledgers][]) |
| `ledger_index` | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (参见 [Specifying Ledgers][]) |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 0,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 14342939,
    "problems": [
      "You should immediately set your default ripple flag",
      "You should clear the no ripple flag on your XAU line to r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
      "You should clear the no ripple flag on your USD line to rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q"
    ],
    "transactions": [
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Sequence": 1406,
        "SetFlag": 8,
        "TransactionType": "AccountSet"
      },
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Flags": 262144,
        "LimitAmount": {
          "currency": "XAU",
          "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
          "value": "0"
        },
        "Sequence": 1407,
        "TransactionType": "TrustSet"
      },
      {
        "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "Fee": 10000,
        "Flags": 262144,
        "LimitAmount": {
          "currency": "USD",
          "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
          "value": "5"
        },
        "Sequence": 1408,
        "TransactionType": "TrustSet"
      }
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
        "ledger_current_index": 14380381,
        "problems": [
            "You should immediately set your default ripple flag",
            "You should clear the no ripple flag on your XAU line to r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
            "You should clear the no ripple flag on your USD line to rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q"
        ],
        "status": "success",
        "transactions": [
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Sequence": 1406,
                "SetFlag": 8,
                "TransactionType": "AccountSet"
            },
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "XAU",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "0"
                },
                "Sequence": 1407,
                "TransactionType": "TrustSet"
            },
            {
                "Account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                "Fee": 10000,
                "Flags": 262144,
                "LimitAmount": {
                    "currency": "USD",
                    "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                    "value": "5"
                },
                "Sequence": 1408,
                "TransactionType": "TrustSet"
            }
        ],
        "validated": false
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type   | Description                                |
|:-----------------------|:-------|:-------------------------------------------|
| `ledger_current_index` | Number | 用于计算这些结果的分类帐的[分类帐索引][]。 |
| `problems`             | Array  | 字符串数组，具有人类可读的问题描述。如果帐户的默认Ripple设置不符合建议，则这包括最多一个条目，对于没有Ripple设置不符合建议的信任行，还包括最多`limit`个条目。 |
| `transactions`         | Array  | (可以省略) 如果请求将`transactions`指定为`true`，则这是一个JSON对象数组，每个JSON对象都是[transation](transaction-formats.html)的JSON格式，应该可以修复所描述的问题之一。此数组的长度与`problems`数组的长度相同，并且每个条目都用于修复该数组中同一索引处描述的问题。|

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 在请求的`帐户`字段中指定的[地址][]与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
