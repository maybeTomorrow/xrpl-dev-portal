---
html: validator_info.html
parent: status-and-debugging-methods.html
blurb: Get the server's validation settings, if configured as a validator.
---
# validator_info
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/ValidatorInfo.cpp "Source")

The `{{currentpage.name}}` method returns the current validator settings of the server, if it is configured as a validator.

_The `{{currentpage.name}}` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._


### Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "command": "{{currentpage.name}}"
}
```

*JSON-RPC*

```json
{
    "method": "{{currentpage.name}}"
}
```

*Commandline*

```sh
#Syntax: {{currentpage.name}}
rippled {{currentpage.name}}
```

<!-- MULTICODE_BLOCK_END -->

The request does not accept any parameters.


### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "result": {
    "domain": "mduo13.com",
    "ephemeral_key": "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
    "manifest": "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
    "master_key": "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
    "seq": 1
  },
  "status": "success",
  "type": "response"
}
```

*JSON-RPC*

```json
200 OK

{
  "result": {
    "domain": "mduo13.com",
    "ephemeral_key": "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
    "manifest": "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
    "master_key": "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
    "seq": 1,
    "status": "success"
  }
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "domain" : "mduo13.com",
      "ephemeral_key" : "n9KnrcCmL5psyKtk2KWP6jy14Hj4EXuZDg7XMdQJ9cSDoFSp53hu",
      "manifest" : "JAAAAAFxIe002KClGBUlRA7h5J2Y5B7Xdlxn1Z5OxY7ZC2UmqUIikHMhAkVIeB7McBf4NFsBceQQlScTVUWMdpYzwmvs115SUGDKdkcwRQIhAJnKfYWnPsBsATIIRfgkAAK+HE4zp8G8AmOPrHmLZpZAAiANiNECVQTKktoD7BEoEmS8jaFBNMgRdcG0dttPurCAGXcKbWR1bzEzLmNvbXASQPjO6wxOfhtWsJ6oMWBg8Rs5STAGvQV2ArI5MG3KbpFrNSMxbx630Ars9d9j1ORsUS5v1biZRShZfg9180JuZAo=",
      "master_key" : "nHBk5DPexBjinXV8qHn7SEKzoxh2W92FxSbNTPgGtQYBzEF4msn9",
      "seq" : 1,
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field` | Type   | Description                                               |
|:--------|:-------|:----------------------------------------------------------|
| `domain` | String | _(May be omitted)_ The domain name associated with this validator, if one has been configured.
| `ephemeral_key` | String | _(May be omitted)_ The public key of the ephemeral key pair this server uses to sign validation messages, in [base58][]. This key changes if the validator's configured token changes. |
| `manifest` | String | _(May be omitted)_ The public "manifest" corresponding to this validator's configured token, [serialized to binary](serialization.html) and then encoded in base64. This field does not contain any private information. |
| `master_key` | String | The public key of this validator's master key pair, in [base58][]. This key uniquely identifies the validator and remains the same if the validator rotates ephemeral keys. If the server is configured using a `[validation_seed]` instead of a `[validator_token]`, this is the only field in the response. |
| `seq` | Number | _(May be omitted)_ A sequence number for this validator's configured validation token and settings. This number increases whenever the validator operator updates the validator's token to rotate ephemeral keys or change settings. |

For more information on validator tokens and key rotation, see the [validator-keys-tool Guide](https://github.com/ripple/validator-keys-tool/blob/master/doc/validator-keys-tool-guide.md).


### Possible Errors

- 任何[通用错误类型][]。
- `invalidParams` - The server returns this error with `"error_message" : "not a validator"` if the server is not [configured as a validator](run-rippled-as-a-validator.html).

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
