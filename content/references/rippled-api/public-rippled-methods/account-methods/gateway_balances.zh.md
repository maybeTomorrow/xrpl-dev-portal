---
html: gateway_balances.html
parent: account-methods.html
blurb: Calculate total amounts issued by an account.
---
# gateway_balances
[[Source]](https://github.com/ripple/rippled/blob/9111ad1a9dc37d49d085aa317712625e635197c0/src/ripple/rpc/handlers/GatewayBalances.cpp "Source")

`gateway_balances`命令计算给定帐户发出的总余额，可以选择排除[operational addresses](issuing-and-operational-addresses.html)持有的金额 . 

## Request Format
请求格式的示例:

{% include '_snippets/no-cli-syntax.md' %}

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "example_gateway_balances_1",
    "command": "gateway_balances",
    "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
    "strict": true,
    "hotwallet": ["rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ","ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt"],
    "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
    "method": "gateway_balances",
    "params": [
        {
            "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
            "hotwallet": [
                "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ",
                "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt"
            ],
            "ledger_index": "validated",
            "strict": true
        }
    ]
}
```

*Commandline*
```sh
rippled json gateway_balances ' {"account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q", "hotwallet": ["rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ", "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt"],"ledger_index": "validated","strict": true} '
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `account`      | String                     | 要检查的[地址][]。这应该是[发布地址](issuing-and-operational-addresses.html) |
| `strict`       | Boolean                    | _(可选)_ 如果为true，则仅接受account参数的地址或公钥。默认为false。 |
| `hotwallet`    | String or Array            | _(可选)_ 要从已发行余额中排除的[操作地址]（issuing and operational addresses.html），或此类地址的数组。 |
| `ledger_hash`  | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[Specifying Ledgers][]） |
| `ledger_index` | String or Unsigned Integer | _(可选)_ 要使用的分类帐版本的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (参见 [Specifying Ledgers][]) |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 3,
  "status": "success",
  "type": "response",
  "result": {
    "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
    "assets": {
      "r9F6wk8HkXrgYWoJ7fsv4VrUBVoqDVtzkH": [
        {
          "currency": "BTC",
          "value": "5444166510000000e-26"
        }
      ],
      "rPFLkxQk6xUGdGYEykqe7PR25Gr7mLHDc8": [
        {
          "currency": "EUR",
          "value": "4000000000000000e-27"
        }
      ],
      "rPU6VbckqCLW4kb51CWqZdxvYyQrQVsnSj": [
        {
          "currency": "BTC",
          "value": "1029900000000000e-26"
        }
      ],
      "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ": [
        {
          "currency": "BTC",
          "value": "4000000000000000e-30"
        }
      ],
      "rwmUaXsWtXU4Z843xSYwgt1is97bgY8yj6": [
        {
          "currency": "BTC",
          "value": "8700000000000000e-30"
        }
      ]
    },
    "balances": {
      "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ": [
        {
          "currency": "EUR",
          "value": "29826.1965999999"
        }
      ],
      "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt": [
        {
          "currency": "USD",
          "value": "13857.70416"
        }
      ]
    },
    "ledger_hash": "61DDBF304AF6E8101576BF161D447CA8E4F0170DDFBEAFFD993DC9383D443388",
    "ledger_index": 14483195,
    "obligations": {
      "BTC": "5908.324927635318",
      "EUR": "992471.7419793958",
      "GBP": "4991.38706013193",
      "USD": "1997134.20229482"
    },
    "validated": true
  }
}
```

*JSON-RPC*

```json
200 OK
{
    "result": {
        "account": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
        "assets": {
            "r9F6wk8HkXrgYWoJ7fsv4VrUBVoqDVtzkH": [
                {
                    "currency": "BTC",
                    "value": "5444166510000000e-26"
                }
            ],
            "rPFLkxQk6xUGdGYEykqe7PR25Gr7mLHDc8": [
                {
                    "currency": "EUR",
                    "value": "4000000000000000e-27"
                }
            ],
            "rPU6VbckqCLW4kb51CWqZdxvYyQrQVsnSj": [
                {
                    "currency": "BTC",
                    "value": "1029900000000000e-26"
                }
            ],
            "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ": [
                {
                    "currency": "BTC",
                    "value": "4000000000000000e-30"
                }
            ],
            "rwmUaXsWtXU4Z843xSYwgt1is97bgY8yj6": [
                {
                    "currency": "BTC",
                    "value": "8700000000000000e-30"
                }
            ]
        },
        "balances": {
            "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ": [
                {
                    "currency": "EUR",
                    "value": "29826.1965999999"
                }
            ],
            "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt": [
                {
                    "currency": "USD",
                    "value": "13857.70416"
                }
            ]
        },
        "ledger_hash": "980FECF48CA4BFDEC896692C31A50D484BDFE865EC101B00259C413AA3DBD672",
        "ledger_index": 14483212,
        "obligations": {
            "BTC": "5908.324927635318",
            "EUR": "992471.7419793958",
            "GBP": "4991.38706013193",
            "USD": "1997134.20229482"
        },
        "status": "success",
        "validated": true
    }
}
```

*Commandline*
```json
{
   "result" : {
      "account" : "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
      "assets" : {
         "r9F6wk8HkXrgYWoJ7fsv4VrUBVoqDVtzkH" : [
            {
               "currency" : "BTC",
               "value" : "5444166510000000e-26"
            }
         ],
         "rPU6VbckqCLW4kb51CWqZdxvYyQrQVsnSj" : [
            {
               "currency" : "BTC",
               "value" : "1029900000000000e-26"
            }
         ],
         "rpR95n1iFkTqpoy1e878f4Z1pVHVtWKMNQ" : [
            {
               "currency" : "BTC",
               "value" : "4000000000000000e-30"
            }
         ],
         "rwmUaXsWtXU4Z843xSYwgt1is97bgY8yj6" : [
            {
               "currency" : "BTC",
               "value" : "8700000000000000e-30"
            }
         ]
      },
      "balances" : {
         "rKm4uWpg9tfwbVSeATv4KxDe6mpE9yPkgJ" : [
            {
               "currency" : "EUR",
               "value" : "144816.1965999999"
            }
         ],
         "ra7JkEzrgeKHdzKgo4EUUVBnxggY4z37kt" : [
            {
               "currency" : "USD",
               "value" : "6677.38614"
            }
         ]
      },
      "frozen_balances" : {
         "r4keXr5myiU4iTLh68ZqZ2CgsJ8dM9FSW6" : [
            {
               "currency" : "BTC",
               "value" : "0.091207822800868"
            }
         ]
      },
      "ledger_hash" : "6C789EAF25A931565E5936042EED037F287F3348B61A70777649552E0385B0E4",
      "ledger_index" : 57111383,
      "obligations" : {
         "BTC" : "1762.700511879441",
         "EUR" : "813792.4267005104",
         "GBP" : "4974.337212333351",
         "USD" : "6739710.218284974"
      },
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                      | Description             |
|:-----------------------|:--------------------------|:------------------------|
| `account`              | String - [Address][]      | The address of the account that issued the balances. |
| `obligations`          | Object                    | (Omitted if empty) Total amounts issued to addresses not excluded, as a map of currencies to the total value issued. |
| `balances`             | Object                    | _(Omitted if empty)_ Amounts issued to the `hotwallet` addresses from the request. The keys are addresses and the values are arrays of currency amounts they hold. |
| `assets`               | Object                    | _(Omitted if empty)_ Total amounts held that are issued by others. In the recommended configuration, the [issuing address](issuing-and-operational-addresses.html) should have none. |
| `ledger_hash`          | String - [Hash][]         | _(May be omitted)_ The identifying hash of the ledger version that was used to generate this response. |
| `ledger_index`         | Number - [Ledger Index][] | _(May be omitted)_ The ledger index of the ledger version that was used to generate this response. |
| `ledger_current_index` | Number - [Ledger Index][] | _(Omitted if `ledger_current_index` is provided)_ The [ledger index][] of the current in-progress ledger version, which was used to retrieve this information. |

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `invalidHotWallet` - `hotwallet`字段中指定的一个或多个地址不是该帐户从请求中发出的持有货币的帐户的[Address][]。
* `actNotFound` - 在请求的`帐户`字段中指定的[地址][]与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
