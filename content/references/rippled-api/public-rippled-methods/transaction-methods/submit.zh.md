---
html: submit.html
parent: transaction-methods.html
blurb: Send a transaction to the network.
---
# submit
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Submit.cpp "Source")

`submit`方法应用[transaction](transaction-formats.html)并将其发送到网络，以便确认并包含在将来的分类帐中。

此命令有两种模式:

* 仅提交模式将已签名的序列化事务作为二进制blob，并按原样将其提交到网络。由于签名的事务对象是不可变的，因此提交后不能修改或自动填充事务的任何部分。
* Sign-and-submit模式接受JSON格式的事务对象，以与[sign method][]相同的方式完成并签署事务，然后提交已签名的事务。我们建议仅在测试和开发时使用此模式。

要尽可能可靠地发送事务，您应该提前构造并[sign][设计方法]，将其保留在断电后仍可以访问的位置，然后`tx_blob`提交。提交后，使用[tx method][]命令监视网络，以查看事务是否已成功应用；如果发生重新启动或其他问题，您可以安全地重新提交`tx_blob`事务：它不会被应用两次，因为它与旧事务具有相同的序列号。
 

## Submit-Only Mode

仅提交请求包含以下参数:

| `Field`     | Type    | Description                                          |
|:------------|:--------|:-----------------------------------------------------|
| `tx_blob`   | String  | Hex representation of the signed transaction to submit. This can be a [multi-signed transaction](multi-signing.html). |
| `fail_hard` | Boolean | (Optional, defaults to false) If true, and the transaction fails locally, do not retry or relay the transaction to other servers |

### Request Format

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 3,
    "command": "submit",
    "tx_blob": "1200002280000000240000001E61D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000B732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7447304502210095D23D8AF107DF50651F266259CC7139D0CD0C64ABBA3A958156352A0D95A21E02207FCF9B77D7510380E49FF250C21B57169E14E9B4ACFD314CEDC79DDD0A38B8A681144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754"
}
```

*JSON-RPC*

```json
{
    "method": "submit",
    "params": [
        {
            "tx_blob": "1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100D184EB4AE5956FF600E7536EE459345C7BBCF097A84CC61A93B9AF7197EDB98702201CEA8009B7BEEBAA2AACC0359B41C427C1C5B550A4CA4B80CF2174AF2D6D5DCE81144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754"
        }
    ]
}
```

*Commandline*

```sh
#Syntax: submit tx_blob
submit 1200002280000000240000000361D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA968400000000000000A732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100D184EB4AE5956FF600E7536EE459345C7BBCF097A84CC61A93B9AF7197EDB98702201CEA8009B7BEEBAA2AACC0359B41C427C1C5B550A4CA4B80CF2174AF2D6D5DCE81144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#submit)


## Sign-and-Submit Mode

此模式签署事务并立即提交。此模式用于测试。您不能对[multi-signed transactions](multi-signing.html)使用此模式。

_By default, sign-and-submit mode is [admin-only](admin-rippled-methods.html)._ It can be used as a public method if the server has [enabled public signing](enable-public-signing.html).

您可以通过以下方式提供用于签署事务的密钥:

* 提供一个`secret`值并省略`key_type`字段。此值可以格式化为XRP分类帐[base58][]种子、RFC-1751、十六进制或字符串密码短语。
* 提供一个`key_type`值，并且正好是`seed`、`seed_hex`或`passphrase`中的一个。忽略`secret`字段(命令行语法不支持。）

请求包括以下参数:

| `Field`        | Type    | Description                                       |
|:---------------|:--------|:--------------------------------------------------|
| `tx_json`      | Object  | [Transaction definition](transaction-formats.html) in JSON format, optionally omitting any auto-fillable fields. |
| `secret`       | String  | _(Optional)_ Secret key of the account supplying the transaction, used to sign it. Do not send your secret to untrusted servers or through unsecured network connections. Cannot be used with `key_type`, `seed`, `seed_hex`, or `passphrase`. |
| `seed`         | String  | _(Optional)_ Secret key of the account supplying the transaction, used to sign it. Must be in the XRP Ledger's [base58][] format. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed_hex`, or `passphrase`. |
| `seed_hex`     | String  | _(Optional)_ Secret key of the account supplying the transaction, used to sign it. Must be in hexadecimal format. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed`, or `passphrase`. |
| `passphrase`   | String  | _(Optional)_ Secret key of the account supplying the transaction, used to sign it, as a string passphrase. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed`, or `seed_hex`. |
| `key_type`     | String  | _(Optional)_ Type of cryptographic key provided in this request. Valid types are `secp256k1` or `ed25519`. Defaults to `secp256k1`. Cannot be used with `secret`. **Caution:** Ed25519 support is experimental. |
| `fail_hard`    | Boolean | _(Optional)_ If `true`, and the transaction fails locally, do not retry or relay the transaction to other servers. The default is `false`. [Updated in: rippled 1.5.0][] |
| `offline`      | Boolean | (Optional, defaults to false) If true, when constructing the transaction, do not try to automatically fill in or validate values. |
| `build_path`   | Boolean | _(Optional)_ If this field is provided, the server [auto-fills](transaction-common-fields.html#auto-fillable-fields) the `Paths` field of a [Payment transaction][] before signing. You must omit this field if the transaction is a [direct XRP payment](direct-xrp-payments.html) or if it is not a Payment-type transaction. **Caution:** The server looks for the presence or absence of this field, not its value. This behavior may change. ([Issue #3272](https://github.com/ripple/rippled/issues/3272)) |
| `fee_mult_max` | Integer | _(Optional)_ Sign-and-submit fails with the error `rpcHIGH_FEE` if the [auto-filled `Fee` value](transaction-common-fields.html#auto-fillable-fields) would be greater than the [reference transaction cost](transaction-cost.html#special-transaction-costs) × `fee_mult_max` ÷ `fee_div_max`. This field has no effect if you explicitly specify the `Fee` field of the transaction. The default is `10`. |
| `fee_div_max`  | Integer | _(Optional)_ Sign-and-submit fails with the error `rpcHIGH_FEE` if the [auto-filled `Fee` value](transaction-common-fields.html#auto-fillable-fields) would be greater than the [reference transaction cost](transaction-cost.html#special-transaction-costs) × `fee_mult_max` ÷ `fee_div_max`. This field has no effect if you explicitly specify the `Fee` field of the transaction. The default is `1`. [New in: rippled 0.30.1][] |

See the [sign method][] for detailed information on how the server automatically fills in certain fields.

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "command": "submit",
  "tx_json" : {
      "TransactionType" : "Payment",
      "Account" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Destination" : "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Amount" : {
         "currency" : "USD",
         "value" : "1",
         "issuer" : "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
      }
   },
   "secret" : "s████████████████████████████",
   "offline": false,
   "fee_mult_max": 1000
}
```

*JSON-RPC*

```json
{
    "method": "submit",
    "params": [
        {
            "offline": false,
            "secret": "s████████████████████████████",
            "tx_json": {
                "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "Amount": {
                    "currency": "USD",
                    "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                    "value": "1"
                },
                "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
                "TransactionType": "Payment"
            },
            "fee_mult_max": 1000
        }
    ]
}
```

*Commandline*

```sh
#Syntax: submit secret json [offline]
rippled submit s████████████████████████████ '{"Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "Amount": { "currency": "USD", "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn", "value": "1" }, "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX", "TransactionType": "Payment", "Fee": "10000"}'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#submit)

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
    "accepted" : true,
    "account_sequence_available" : 362,
    "account_sequence_next" : 362,
    "applied" : true,
    "broadcast" : true,
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "kept" : true,
    "open_ledger_cost": "10",
    "queued" : false,
    "tx_blob": "1200002280000000240000016861D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB7446304402200E5C2DD81FDF0BE9AB2A8D797885ED49E804DBF28E806604D878756410CA98B102203349581946B0DDA06B36B35DBC20EDA27552C1F167BCF5C6ECFF49C6A46F858081144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
    "tx_json": {
      "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
      "Amount": {
        "currency": "USD",
        "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
        "value": "1"
      },
      "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
      "Fee": "10000",
      "Flags": 2147483648,
      "Sequence": 360,
      "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
      "TransactionType": "Payment",
      "TxnSignature": "304402200E5C2DD81FDF0BE9AB2A8D797885ED49E804DBF28E806604D878756410CA98B102203349581946B0DDA06B36B35DBC20EDA27552C1F167BCF5C6ECFF49C6A46F8580",
      "hash": "4D5D90890F8D49519E4151938601EF3D0B30B16CD6A519D9C99102C9FA77F7E0"
    },
    "validated_ledger_index" : 21184416
  }
}
```

*JSON-RPC*

```json
{
    "result": {
        "accepted" : true,
        "account_sequence_available" : 362,
        "account_sequence_next" : 362,
        "applied" : true,
        "broadcast" : true,
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "kept" : true,
        "open_ledger_cost": "10",
        "queued" : false,
        "tx_blob": "1200002280000000240000016961D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F181144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
        "tx_json": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "Amount": {
                "currency": "USD",
                "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "value": "1"
            },
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Fee": "10000",
            "Flags": 2147483648,
            "Sequence": 361,
            "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F1",
            "hash": "5B31A7518DC304D5327B4887CD1F7DC2C38D5F684170097020C7C9758B973847"
        }
    },
    "validated_ledger_index" : 21184416
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
    "result": {
        "accepted" : true,
        "account_sequence_available" : 362,
        "account_sequence_next" : 362,
        "applied" : true,
        "broadcast" : true,
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "kept" : true,
        "open_ledger_cost": "10",
        "queued" : false,
        "tx_blob": "1200002280000000240000016961D4838D7EA4C6800000000000000000000000000055534400000000004B4E9C06F24296074F7BC48F92A97916C6DC5EA9684000000000002710732103AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB74473045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F181144B4E9C06F24296074F7BC48F92A97916C6DC5EA983143E9D4A2B8AA0780F682D136F7A56D6724EF53754",
        "tx_json": {
            "Account": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
            "Amount": {
                "currency": "USD",
                "issuer": "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn",
                "value": "1"
            },
            "Destination": "ra5nK24KXen9AHvsdFTKHSANinZseWnPcX",
            "Fee": "10000",
            "Flags": 2147483648,
            "Sequence": 361,
            "SigningPubKey": "03AB40A0490F9B7ED8DF29D246BF2D6269820A0EE7742ACDD457BEA7C7D0931EDB",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100A7CCD11455E47547FF617D5BFC15D120D9053DFD0536B044F10CA3631CD609E502203B61DEE4AC027C5743A1B56AF568D1E2B8E79BB9E9E14744AC87F38375C3C2F1",
            "hash": "5B31A7518DC304D5327B4887CD1F7DC2C38D5F684170097020C7C9758B973847"
        }
    },
    "validated_ledger_index" : 21184416
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                 | Type    | Description                              |
|:------------------------|:--------|:-----------------------------------------|
| `engine_result`         | String  | Text [result code](transaction-results.html) indicating the preliminary result of the transaction, for example `tesSUCCESS` |
| `engine_result_code`    | Integer | Numeric version of the [result code](transaction-results.html). **Not recommended.** |
| `engine_result_message` | String  | Human-readable explanation of the transaction's preliminary result |
| `tx_blob`               | String  | The complete transaction in hex string format |
| `tx_json`               | Object  | The complete transaction in JSON format  |
| `accepted`              | Boolean | _(Omitted in sign-and-submit mode)_ The value `true` indicates that the transaction was applied, queued, broadcast, or kept for later. The value `false` indicates that none of those happened, so the transaction cannot possibly succeed as long as you do not submit it again and have not already submitted it another time. [New in: rippled 1.5.0][] |
| `account_sequence_available` | Number | _(Omitted in sign-and-submit mode)_ The next [Sequence Number][] available for the sending account after all pending and [queued](transaction-queue.html) transactions. [New in: rippled 1.5.0][] |
| `account_sequence_next` | number  | _(Omitted in sign-and-submit mode)_ The next [Sequence Number][] for the sending account after all transactions that have been provisionally applied, but not transactions in the [queue](transaction-queue.html). [New in: rippled 1.5.0][] |
| `applied`               | Boolean | _(Omitted in sign-and-submit mode)_ The value `true` indicates that this transaction was applied to the open ledger. In this case, the transaction is likely, but not guaranteed, to be validated in the next ledger version. [New in: rippled 1.5.0][] |
| `broadcast`             | Boolean | _(Omitted in sign-and-submit mode)_ The value `true` indicates this transaction was broadcast to peer servers in the peer-to-peer XRP Ledger network. (Note: if the server has no peers, such as in [stand-alone mode][], the server uses the value `true` for cases where it _would_ have broadcast the transaction.) The value `false` indicates the transaction was not broadcast to any other servers. [New in: rippled 1.5.0][] |
| `kept`                  | Boolean | _(Omitted in sign-and-submit mode)_ The value `true` indicates that the transaction was kept to be retried later. [New in: rippled 1.5.0][] |
| `queued`                | Boolean | _(Omitted in sign-and-submit mode)_ The value `true` indicates the transaction was put in the [Transaction Queue](transaction-queue.html), which means it is likely to be included in a future ledger version. [New in: rippled 1.5.0][] |
| `open_ledger_cost`      | String  | _(Omitted in sign-and-submit mode)_ The current [open ledger cost](transaction-cost.html#open-ledger-cost) before processing this transaction. Transactions with a lower cost are likely to be [queued](transaction-queue.html). [New in: rippled 1.5.0][] |
| `validated_ledger_index` | Integer  | _(Omitted in sign-and-submit mode)_ The [ledger index][] of the newest validated ledger at the time of submission. This provides a lower bound on the ledger versions that the transaction can appear in as a result of this request. (The transaction could only have been validated in this ledger version or earlier if it had already been submitted before.) |

**Warning:** 即使WebSocket响应具有`"status":"success"`，表示已成功接收命令，但这并不表示事务已成功执行。许多情况都会妨碍交易的成功处理，例如在支付中缺少连接两个帐户的信任线，或者自交易建立以来分类帐状态发生变化。即使没有错误，也可能需要几秒钟来关闭并验证包含该交易的分类帐版本。有关详细信息，请参阅[完整的事务处理响应列表](transaction-results.html)，在事务处理结果出现在已验证的分类帐版本中之前，不要将其视为最终结果。

**Caution:** 如果此命令导致错误消息，则消息可以包含来自请求的密钥(这只能在“签名并提交”模式下发生。）请确保其他人看不到这些错误。

* 不要将包含密钥的错误写入多人可以看到的日志文件。
* 不要将包含密钥的错误粘贴到公共场所进行调试。
* 不要在网站上显示包含密钥的错误消息，即使是意外情况。


## Possible Errors

* Any of the [universal error types][].
* `amendmentBlocked` - 无法将事务提交到网络，因为`Ripple`服务器正在运行[amendment blocked](amendments.html#amendment-blocked).
* `highFee` - 指定了`fee_mult_max`参数，但服务器当前的费用乘数超过了指定的值(签名和提交模式）
* `internalJson` - 将事务序列化为JSON时发生内部错误。这可能是由事务的许多方面造成的，包括错误的签名或某些字段的格式不正确。
* `internalSubmit` - 提交事务时发生内部错误。这可能是由事务的许多方面造成的，包括错误的签名或某些字段的格式不正确。
* `internalTransaction` - 处理事务时发生内部错误。这可能是由事务的许多方面造成的，包括错误的签名或某些字段的格式不正确。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `invalidTransaction` - 事务格式不正确或无效。
* `noPath` - 事务不包括路径，服务器找不到进行此支付的路径(签名和提交模式）
* `tooBusy` - 事务不包括路径，但服务器太忙，无法立即进行路径查找。如果以管理员身份连接，则不会发生。(仅签名和提交模式)
* `notSupported` - 此服务器不支持签名（仅签名和提交模式。）如果您是服务器管理员，在以管理员(enable-public-signing.html)身份连接时仍然可以访问签名，也可以[启用公共签名]


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
