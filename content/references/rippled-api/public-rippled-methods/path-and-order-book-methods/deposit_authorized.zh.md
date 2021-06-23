---
html: deposit_authorized.html
parent: path-and-order-book-methods.html
blurb: Check whether an account is authorized to send money directly to another.
---
# deposit_authorized
[[Source]](https://github.com/ripple/rippled/blob/817d2339b8632cb2f97d3edd6f7af33aa7631744/src/ripple/rpc/handlers/DepositAuthorized.cpp "Source")

`deposit_authorized` 命令指示一个帐户是否有权直接向另一个帐户发送付款。参见 [Deposit Authorization](depositauth.html) 有关如何要求授权才能将钱汇入您的帐户的信息。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "command": "deposit_authorized",
  "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
  "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
  "ledger_index": "validated"
}
```

*JSON-RPC*

```json
{
  "method": "deposit_authorized",
  "params": [
    {
      "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
      "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
      "ledger_index": "validated"
    }
  ]
}
```

*Commandline*

```bash
#Syntax: deposit_authorized <source_account> <destination_account> [<ledger>]
rippled deposit_authorized rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8 validated
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`               | Type                       | Description             |
|:----------------------|:---------------------------|:------------------------|
| `source_account`      | String - [Address][]       | 可能付款的发送者。 |
| `destination_account` | String - [Address][]       | 可能付款的接受者。 |
| `ledger_hash`         | String                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[分类帐][]） |
| `ledger_index`        | String or Unsigned Integer | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (参见 [分类帐][]) |


## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "result": {
    "deposit_authorized": true,
    "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
    "ledger_hash": "BD03A10653ED9D77DCA859B7A735BF0580088A8F287FA2C5403E0A19C58EF322",
    "ledger_index": 8,
    "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
    "validated": true
  },
  "status": "success",
  "type": "response"
}
```

*JSON-RPC*

```json
{
  "result": {
    "deposit_authorized": true,
    "destination_account": "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
    "ledger_hash": "BD03A10653ED9D77DCA859B7A735BF0580088A8F287FA2C5403E0A19C58EF322",
    "ledger_index": 8,
    "source_account": "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
    "status": "success",
    "validated": true
  }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
2018-Jul-30 20:07:38.771658157 HTTPClient:NFO Connecting to 127.0.0.1:5005

{
   "result" : {
      "deposit_authorized" : true,
      "destination_account" : "rsUiUMpnrgxQp24dJYZDhmV4bE3aBtQyt8",
      "ledger_hash" : "BD03A10653ED9D77DCA859B7A735BF0580088A8F287FA2C5403E0A19C58EF322",
      "ledger_index" : 8,
      "source_account" : "rEhxGqkqPPSxQ3P25J66ft5TwpzV14k2de",
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                      | Description             |
|:-----------------------|:--------------------------|:------------------------|
| `deposit_authorized`   | Boolean                   | 指定的源帐户是否有权将付款直接发送到目标帐户。 如果`true`，则目标帐户不需要[存款授权](depositauth.html)，或者源帐户已预授权。 |
| `destination_account`  | String - [Address][]      | 请求中指定的目标帐户。 |
| `ledger_hash`          | String                    | _(可以省略)_ 用于生成此响应的分类帐的标识哈希。 |
| `ledger_index`         | Number - [Ledger Index][] | _(可以省略)_ 用于生成此响应的分类帐版本的分类帐索引。 |
| `ledger_current_index` | Number - [Ledger Index][] | _(可以省略)_ 用于生成此响应的当前进行中分类帐版本的分类帐索引。 |
| `source_account`       | String - [Address][]      | 请求中指定的源帐户。 |
| `validated`            | Boolean                   | _(可以省略)_ 如果为`true`，则信息来自已验证的分类帐版本。 |

**Note:** `授权的`状态为`true`不能保证付款可以从指定的来源发送到指定的目的地。 例如，目标帐户可能没有指定货币的[信托行](trust-lines-and-issuing.html)，或者可能没有足够的流动性来交付付款。

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actMalformed` - 在请求的`source_account`或`destination_account`字段中指定的[地址][]格式不正确(它可能包含输入错误或长度错误，从而导致校验和失败。）
* `dstActNotFound` - 请求的`destination_account`字段与分类帐中的帐户不对应。
* `lgrNotFound` - 仅在以下`ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。
* `srcActNotFound` - 请求的`source_account`字段与分类帐中的帐户不对应。

{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
