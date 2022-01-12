---
html: channel_authorize.html
parent: payment-channel-methods.html
blurb: Sign a claim for money from a payment channel.
---
# channel_authorize
[[Source]](https://github.com/ripple/rippled/blob/d4a56f223a3b80f64ff70b4e90ab6792806929ca/src/ripple/rpc/handlers/PayChanClaim.cpp#L41 "Source")

_(Added by the [PayChan amendment][] to be enabled. [New in: rippled 0.33.0][])_

`channel_authorize`方法创建一个签名，可用于从支付渠道兑换特定金额的HWA。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "channel_authorize_example_id1",
    "command": "channel_authorize",
    "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
    "seed": "s████████████████████████████",
    "key_type": "secp256k1",
    "amount": "1000000",
}
```

*JSON-RPC*

```json
POST http://localhost:5005/
Content-Type: application/json

{
    "method": "channel_authorize",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "seed": "s████████████████████████████",
        "key_type": "secp256k1",
        "amount": "1000000"
    }]
}
```

*Commandline*

```sh
#Syntax: channel_authorize <private_key> [<key_type>] <channel_id> <drops>
rippled channel_authorize s████████████████████████████ secp256k1 5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3 1000000
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| Field | Type | Description |
|-------|------|-------------|
| `channel_id` | String | The unique ID of the payment channel to use.
| `secret` | String | _(Optional)_ The secret key to use to sign the claim. This must be the same key pair as the public key specified in the channel. Cannot be used with `seed`, `seed_hex`, or `passphrase`. [Updated in: rippled 1.4.0][] |
| `seed`         | String  | _(Optional)_ The secret seed to use to sign the claim. This must be the same key pair as the public key specified in the channel. Must be in the XRP Ledger's [base58][] format. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed_hex`, or `passphrase`. [New in: rippled 1.4.0][] |
| `seed_hex`     | String  | _(Optional)_ The secret seed to use to sign the claim. This must be the same key pair as the public key specified in the channel. Must be in hexadecimal format. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed`, or `passphrase`. [New in: rippled 1.4.0][] |
| `passphrase`   | String  | _(Optional)_ A string passphrase to use to sign the claim. This must be the same key pair as the public key specified in the channel. The [key derived from this passphrase](cryptographic-keys.html#key-derivation) must match the public key specified in the channel. If provided, you must also specify the `key_type`. Cannot be used with `secret`, `seed`, or `seed_hex`. [New in: rippled 1.4.0][] |
| `key_type` | String | _(Optional)_ The [signing algorithm](cryptographic-keys.html#signing-algorithms) of the cryptographic key pair provided. Valid types are `secp256k1` or `ed25519`. The default is `secp256k1`. [New in: rippled 1.4.0][] |
| `amount` | String | Cumulative amount of XRP, in drops, to authorize. If the destination has already received a lesser amount of XRP from this channel, the signature created by this method can be redeemed for the difference. |

The request **must** specify exactly one of `secret`, `seed`, `seed_hex`, or `passphrase`.

**Warning:** Do not send secret keys to untrusted servers or through unsecured network connections. (This includes the `secret`, `seed`, `seed_hex`, or `passphrase` fields of this request.) You should only use this method on a secure, encrypted network connection to a server you run or fully trust with your funds. Otherwise, eavesdroppers could use your secret key to sign claims and take all the money from this payment channel and anything else using the same key pair. See [Set Up Secure Signing](set-up-secure-signing.html) for instructions.

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "channel_authorize_example_id1",
    "status": "success"
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
    }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "status": "success"
    }
}
```

*Commandline*

```json
{
    "result": {
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "status": "success"
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| Field | Type | Description |
|-------|------|-------------|
| `signature` | String | The signature for this claim, as a hexadecimal value. To process the claim, the destination account of the payment channel must send a [PaymentChannelClaim transaction][] with this signature, the exact Channel ID, XRP amount, and public key of the channel. |

## Possible Errors

* 任何[通用错误类型][]。
* `badKeyType` - 请求中的`key_type`参数不是有效的密钥类型。(有效类型为 `secp256k1` 或 `ed25519`.) [New in: rippled 1.4.0][]
* `badSeed` - 请求中的`secret`不是有效的密钥。
* `channelAmtMalformed` - 请求中的`amount`不是有效的[HWA amount][HWA，在drops]中。
* `channelMalformed` - 请求中的`通道id`不是有效的通道id。通道id应该是256位（64个字符）的十六进制字符串。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
