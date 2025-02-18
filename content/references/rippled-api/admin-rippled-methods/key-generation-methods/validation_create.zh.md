---
html: validation_create.html
parent: key-generation-methods.html
blurb: Generate keys for a new rippled validator.
---
# validation_create
[[Source]](https://github.com/ripple/rippled/blob/315a8b6b602798a4cff4d8e1911936011e12abdb/src/ripple/rpc/handlers/ValidationCreate.cpp "Source")

Use the `validation_create` command to generate [cryptographic keys a `rippled` server can use to identify itself to the network](peer-protocol.html#node-key-pair). Similar to the [wallet_propose method][], this method only generates a set of keys in the proper format. It does not any makes changes to the XRP Ledger data or server configuration.

_The `validation_create` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._

You can configure your server to use the generated key pair to sign validations (validation key pair) or regular peer-to-peer communications ([node key pair](peer-protocol.html#node-key-pair)).

**Tip:** For configuring a robust validator, you should use the `validator-keys` tool (included in the `rippled` RPM) to generate validator tokens (which can be rotated) with an offline master key. For more information, see [Validator Setup](run-rippled-as-a-validator.html#3-enable-validation-on-your-rippled-server).


### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 0,
    "command": "validation_create",
    "secret": "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
}
```

*JSON-RPC*

```json
{
    "method": "validation_create",
    "params": [
        {
            "secret": "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
        }
    ]
}
```

*Commandline*

```sh
#Syntax: validation_create [secret]
rippled validation_create "BAWL MAN JADE MOON DOVE GEM SON NOW HAD ADEN GLOW TIRE"
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`  | Type   | Description                                              |
|:---------|:-------|:---------------------------------------------------------|
| `secret` | String | _(Optional)_ Use this value as a seed to generate the credentials. The same secret always generates the same credentials. You can provide the seed in [RFC-1751](https://tools.ietf.org/html/rfc1751) format or the XRP Ledger's [base58][] format. If omitted, generate a random seed. |

**Note:** The security of your validator depends on the entropy of your seed. Do not use a secret value for real business purposes unless it is generated with a strong source of randomness. Ripple recommends omitting the `secret` when generating new credentials for the first time.

### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*JSON-RPC*

```json
{
   "result" : {
      "status" : "success",
      "validation_key" : "FAWN JAVA JADE HEAL VARY HER REEL SHAW GAIL ARCH BEN IRMA",
      "validation_public_key" : "n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG",
      "validation_seed" : "ssZkdwURFMBXenJPbrpE14b6noJSu"
   }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "status" : "success",
      "validation_key" : "FAWN JAVA JADE HEAL VARY HER REEL SHAW GAIL ARCH BEN IRMA",
      "validation_public_key" : "n9Mxf6qD4J55XeLSCEpqaePW4GjoCR5U1ZeGZGJUCNe3bQa4yQbG",
      "validation_seed" : "ssZkdwURFMBXenJPbrpE14b6noJSu"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                 | Type   | Description                               |
|:------------------------|:-------|:------------------------------------------|
| `validation_key`        | String | The secret key for these validation credentials, in [RFC-1751](https://tools.ietf.org/html/rfc1751) format. |
| `validation_public_key` | String | The public key for these validation credentials, in the XRP Ledger's [base58][] encoded string format. |
| `validation_seed`       | String | The secret key for these validation credentials, in the XRP Ledger's [base58][] encoded string format. |

### Possible Errors

* 任何[通用错误类型][]。
* `badSeed` - The request provided an invalid seed value. This usually means that the seed value appears to be a valid string of a different format, such as an account address or validation public key.

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
