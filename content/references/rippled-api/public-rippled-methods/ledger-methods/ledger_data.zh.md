---
html: ledger_data.html
parent: ledger-methods.html
blurb: Get the raw contents of a ledger version.
---
# ledger_data
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/LedgerData.cpp "Source")

`ledger_data`方法检索指定分类帐的内容。您可以通过多次调用来检索单个分类帐版本的全部内容。

## Request Format
请求格式的示例:

{% include '_snippets/no-cli-syntax.md' %}

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "id": 2,
   "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
   "command": "ledger_data",
   "limit": 5,
   "binary": true
}
```

*JSON-RPC*

```json
{
    "method": "ledger_data",
    "params": [
        {
            "binary": true,
            "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
            "limit": 5
        }
    ]
}
```

<!-- MULTICODE_BLOCK_END -->


A request can include the following fields:

| `Field`        | Type                                       | Description    |
|:---------------|:-------------------------------------------|:---------------|
| `id`           | (Arbitrary)                                | (仅WebSocket) 在响应延迟或无序时将此请求与其他请求分开的任何标识符。 |
| `ledger_hash`  | String                                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串。 (参见 [Specifying Ledgers][]) |
| `ledger_index` | String or Unsigned Integer                 | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串。 (参见 [Specifying Ledgers][]) |
| `binary`       | Boolean                                    | (可选, 默认 False) 如果设置为true，则将分类帐对象作为哈希十六进制字符串而不是JSON返回。 |
| `limit`        | Integer                                    | (可选, 默认值不同) 限制要检索的分类帐对象数。服务器不需要接受此值。||`marker`|[Marker][]|来自上一个分页响应的值。继续检索停止响应的数据。 |

`分类帐`字段已弃用，可能会被删除，恕不另行通知。

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket (binary:true)*

```json
{
    "id": 2,
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "data": "11006122000000002400000001250062FEA42D0000000055C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA0466240000000354540208114C909F42250CFE8F12A7A1A0DFBD3CBD20F32CD79",
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "data": "11006F22000000002400000003250035788533000000000000000034000000000000000055555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F501071633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C6800064D4838D7EA4C68000000000000000000000000000425443000000000035DD7DF146893456296BF4061FBE68735D28F3286540000000000F42408114A4B8F5F7B644AEDC3447F9459C132EEB016A133B",
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "data": "11006F2200020000240000000A250067395C33000000000000000034000000000000000055A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C5010DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C00064D554C88B43EFA00000000000000000000000000055534400000000000A20B3C85F482532A9578DBB3950B85CA06594D165400000B59B9F780081148366FB9ACD2A0FD822E31112D2EB6F98C317C2C1",
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "data": "1100612200000000240000000125003E742F2D0000000055286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21624000000306DC42008114225BAB89C4A4B94624BB069D6DB3C819F934991C",
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
            },
            {
                "data": "110072220002000025000B65783700000000000000003800000000000000005587591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D03756280000000000000000000000000000000000000004254430000000000000000000000000000000000000000000000000166800000000000000000000000000000000000000042544300000000000A20B3C85F482532A9578DBB3950B85CA06594D167D4C38D7EA4C680000000000000000000000000004254430000000000C795FDF8A637BCAAEDAD1C434033506236C82A2D",
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ]
    },
    "status": "success",
    "type": "response"
}
```

*WebSocket (binary:false)*

```json
{
    "id": 2,
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "Account": "rKKzk9ghA2iuy3imqMXUHJqdRPMtNDGf4c",
                "Balance": "893730848",
                "Flags": 0,
                "LedgerEntryType": "AccountRoot",
                "OwnerCount": 0,
                "PreviousTxnID": "C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA046",
                "PreviousTxnLgrSeq": 6487716,
                "Sequence": 1,
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "Account": "rGryPmNWFognBgMtr9k4quqPbbEcCrhNmD",
                "BookDirectory": "71633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C68000",
                "BookNode": "0000000000000000",
                "Flags": 0,
                "LedgerEntryType": "Offer",
                "OwnerNode": "0000000000000000",
                "PreviousTxnID": "555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F",
                "PreviousTxnLgrSeq": 3504261,
                "Sequence": 3,
                "TakerGets": "1000000",
                "TakerPays": {
                    "currency": "BTC",
                    "issuer": "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
                    "value": "1"
                },
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "Account": "rUy8tW38MW9ma7kSjRgB2GHtTkQAFRyrN8",
                "BookDirectory": "DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C000",
                "BookNode": "0000000000000000",
                "Flags": 131072,
                "LedgerEntryType": "Offer",
                "OwnerNode": "0000000000000000",
                "PreviousTxnID": "A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C",
                "PreviousTxnLgrSeq": 6764892,
                "Sequence": 10,
                "TakerGets": "780000000000",
                "TakerPays": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "5850"
                },
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "Account": "rh3C81VfNDhhWPQWCU8ZGgknvdgNUvRtM9",
                "Balance": "13000000000",
                "Flags": 0,
                "LedgerEntryType": "AccountRoot",
                "OwnerCount": 0,
                "PreviousTxnID": "286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21",
                "PreviousTxnLgrSeq": 4092975,
                "Sequence": 1,
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
            },
            {
                "Balance": {
                    "currency": "BTC",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "BTC",
                    "issuer": "rKUK9omZqVEnraCipKNFb5q4tuNTeqEDZS",
                    "value": "10"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "BTC",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "87591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D0375",
                "PreviousTxnLgrSeq": 746872,
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ]
    },
    "status": "success",
    "type": "response"
}
```

*JSON-RPC (binary:true)*

```json
200 OK

{
    "result": {
        "ledger_hash": "842B57C1CC0613299A686D3E9F310EC0422C84D3911E5056389AA7E5808A93C8",
        "ledger_index": "6885842",
        "marker": "0002A590029B53BE7857EFF9985F770EC792CE483720EB5E963C4D6A607D43DF",
        "state": [
            {
                "data": "11006122000000002400000001250062FEA42D0000000055C204A65CF2542946289A3358C67D991B5E135FABFA89F271DBA7A150C08CA0466240000000354540208114C909F42250CFE8F12A7A1A0DFBD3CBD20F32CD79",
                "index": "00001A2969BE1FC85F1D7A55282FA2E6D95C71D2E4B9C0FDD3D9994F3C00FF8F"
            },
            {
                "data": "11006F22000000002400000003250035788533000000000000000034000000000000000055555B93628BF3EC318892BB7C7CDCB6732FF53D12B6EEC4FAF60DD1AEE1C6101F501071633D7DE1B6AEB32F87F1A73258B13FC8CC32942D53A66D4F038D7EA4C6800064D4838D7EA4C68000000000000000000000000000425443000000000035DD7DF146893456296BF4061FBE68735D28F3286540000000000F42408114A4B8F5F7B644AEDC3447F9459C132EEB016A133B",
                "index": "000037C6659BB98F8D09F2F4CFEB27DE8EFEAFE54DD9E1C13AECDF5794B0C0F5"
            },
            {
                "data": "11006F2200020000240000000A250067395C33000000000000000034000000000000000055A160BC41A45B6BB118DF23D77E4FF23C723431B917F50DCB41319ECC2821F34C5010DFA3B6DDAB58C7E8E5D944E736DA4B7046C30E4F460FD9DE4C1AA535D3D0C00064D554C88B43EFA00000000000000000000000000055534400000000000A20B3C85F482532A9578DBB3950B85CA06594D165400000B59B9F780081148366FB9ACD2A0FD822E31112D2EB6F98C317C2C1",
                "index": "0000A8791F78CC9B39200E12A9BDAACCF40A72A512FA815525CFC9BA772990F7"
            },
            {
                "data": "1100612200000000240000000125003E742F2D0000000055286498B513710CFEB2D723A554C7557983D1952DF4DEE342C40DCB43067C9A21624000000306DC42008114225BAB89C4A4B94624BB069D6DB3C819F934991C",
                "index": "0000B717320558E2DE1A3B9FDB24E9A695BF05D1A44E4A4683212BB1DD0FBA23"
            },
            {
                "data": "110072220002000025000B65783700000000000000003800000000000000005587591A63051645F37B85D1FBA55EE69B1C96BFF16904F5C99F03FB93D42D03756280000000000000000000000000000000000000004254430000000000000000000000000000000000000000000000000166800000000000000000000000000000000000000042544300000000000A20B3C85F482532A9578DBB3950B85CA06594D167D4C38D7EA4C680000000000000000000000000004254430000000000C795FDF8A637BCAAEDAD1C434033506236C82A2D",
                "index": "000103996A3BAD918657F86E12A67D693E8FC8A814DA4B958A244B5F14D93E58"
            }
        ],
        "status": "success"
    }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`        | Type                                | Description           |
|:---------------|:------------------------------------|:----------------------|
| `ledger_index` | Unsigned Integer - [Ledger Index][] | 此分类帐版本的分类帐索引。|
| `ledger_hash`  | String - [Hash][]                   | 此分类帐版本的唯一标识哈希。 |
| `state`        | Array                               | 包含分类帐状态树数据的JSON对象数组，定义如下。 |
| `marker`       | [Marker][]                          | 服务器定义的值，指示响应已分页。将此消息传递给下一个呼叫以继续此呼叫中断的位置。 |

`state`数组中每个对象的格式取决于请求中是否将`binary`设置为true。每个`状态`对象可能包括以下字段:

| `Field`             | Type      | Description                                |
|:--------------------|:----------|:-------------------------------------------|
| `data`              | String    | _(仅在以下情况下包括 `"binary":true`)_ Hex representation of the requested data |
| `LedgerEntryType`   | String    | _(仅在以下情况下包括 `"binary":false`)_ String indicating what type of ledger object this object represents. See [ledger object types](ledger-object-types.html) for the full list. |
| (Additional fields) | (Various) | _(仅在以下情况下包括 `"binary":false`)_ Additional fields describing this object, depending on which [ledger object type](ledger-object-types.html) it is. |
| `index`             | String    | Unique identifier for this ledger entry, as hex. |

## Possible Errors

* Any of the [universal error types][]
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `lgrNotFound` - 仅在以下`ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
