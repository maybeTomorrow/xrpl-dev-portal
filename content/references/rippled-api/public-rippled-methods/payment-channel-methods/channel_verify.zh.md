---
html: channel_verify.html
parent: payment-channel-methods.html
blurb: Check a payment channel claim's signature.
---
# channel_verify
[[Source]](https://github.com/ripple/rippled/blob/d4a56f223a3b80f64ff70b4e90ab6792806929ca/src/ripple/rpc/handlers/PayChanClaim.cpp#L89 "Source")

_(Added by the [PayChan amendment][] to be enabled. [New in: rippled 0.33.0][])_

`channel_verify`方法检查签名的有效性，该签名可用于从支付渠道兑换特定金额的XRP。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 1,
    "command": "channel_verify",
    "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
    "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
    "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
    "amount": "1000000"
}
```

*JSON-RPC*

```json
{
    "method": "channel_verify",
    "params": [{
        "channel_id": "5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3",
        "signature": "304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064",
        "public_key": "aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3",
        "amount": "1000000"
    }]
}
```

*Commandline*

```sh
#Syntax: channel_verify <public_key> <channel_id> <amount> <signature>
rippled channel_verify aB44YfzW24VDEJQ2UuLPV2PvqcPCSoLnL7y5M1EzhdW4LnK5xMS3 5DB01B7FFED6B67E6B0414DED11E051D2EE2B7619CE0EAA6286D67A3A4D5BDB3 1000000 304402204EF0AFB78AC23ED1C472E74F4299C0C21F1B21D07EFC0A3838A420F76D783A400220154FB11B6F54320666E4C36CA7F686C16A3A0456800BBC43746F34AF50290064
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| Field | Type | Description |
|-------|------|-------------|
| `amount` | String | The amount of [XRP, in drops][], the provided `signature` authorizes. |
| `channel_id` | String | The Channel ID of the channel that provides the XRP. This is a 64-character hexadecimal string. |
| `public_key` | String | The public key of the channel and the key pair that was used to create the signature, in hexadecimal or the XRP Ledger's [base58][] format. [Updated in: rippled 0.90.0][] |
| `signature` | String | The signature to verify, in hexadecimal. |

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
        "signature_verified":true
    }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "signature_verified":true,
        "status":"success"
    }
}
```

*Commandline*

```json
{
    "result": {
        "signature_verified":true,
        "status":"success"
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| Field | Type | Description |
|-------|------|-------------|
| `signature_verified` | Boolean | If `true`, the signature is valid for the stated amount, channel, and public key. |

**Caution:** 这并不表示检查通道是否分配了足够的XRP。在认为索赔有效之前，您应该在最新验证的分类账中查找该渠道，并确认该渠道已打开且其`amount`值等于或大于索赔的`amount`。为此，请使用[account_channels method][]。

## Possible Errors

* Any of the [universal error types][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `publicMalformed` - 请求的`public_key`字段不是正确格式的有效公钥。公钥是33字节，必须用base58或十六进制表示。帐户公钥的[base58表示形式以字母`a`](base58-encodings.html)开头。十六进制表示法有66个字符长。
* `channelMalformed` - 请求的`channel_id`字段不是有效的通道id。通道id必须是256位（64个字符）的十六进制字符串。
* `channelAmtMalformed` - `amount`字段中指定的值不是有效的[HWA amount][HWA，以滴]为单位。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
