---
html: account_objects.html
parent: account-methods.html
blurb: Get all ledger objects owned by an account.
---
# account_objects
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountObjects.cpp "Source")

`account objects`命令返回帐户拥有的所有对象的原始[分类帐格式][]。有关帐户的信托额度和余额的更高级别视图，请参见[帐户行方法][]

`account_objects`响应中可能出现的对象类型包括:

- [Offer objects](offer.html) 对于当前处于活动状态、资金不足或过期但尚未删除的订单。 (See [Lifecycle of an Offer](offers.html#lifecycle-of-an-offer) for more information.)
- [RippleState objects](ripplestate.html) 对于此帐户方未处于默认状态的信托行。
- 账户的[SignerList](signerlist.html), 如果账户启用了 [multi-signing](multi-signing.html) 。
- [Escrow objects](escrow.html) 对于尚未执行或取消的保留付款。
- [PayChannel objects](paychannel.html) 开放支付渠道。
- [Check objects](check.html) for pending Checks.
- [DepositPreauth objects](depositpreauth-object.html) 存款预授权。 
- [Ticket objects](known-amendments.html#tickets) :未启用：用于票证。


## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 8,
  "command": "account_objects",
  "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "ledger_index": "validated",
  "type": "state",
  "deletion_blockers_only": false,
  "limit": 10
}
```

*JSON-RPC*

```json
{
    "method": "account_objects",
    "params": [
        {
            "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "ledger_index": "validated",
            "type": "state",
            "deletion_blockers_only": false,
            "limit": 10
        }
    ]
}
```


*Commandline*

```sh
#Syntax: account_objects <account> [<ledger>]
rippled account_objects r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59 validated
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`                  | Type             | Description                    |
|:-------------------------|:-----------------|:-------------------------------|
| `account`                | String           | A unique identifier for the account, most commonly the account's address. |
| `type`                   | String           | _(Optional)_ If included, filter results to include only this type of ledger object. The valid types are: `check` :not_enabled:, `deposit_preauth`, `escrow`, `offer`, `payment_channel`, `signer_list`, `ticket` :not_enabled:, and `state` (trust line). <!-- Author's note: Omitted types that can't be owned by an account --> |
| `deletion_blockers_only` | Boolean          | _(Optional)_ If `true`, the response only includes objects that would block this account from [being deleted](accounts.html#deletion-of-accounts). The default is `false`. [New in: rippled 1.4.0][] |
| `ledger_hash`            | String           | _(Optional)_ A 20-byte hex string for the ledger version to use. (See [Specifying Ledgers][]) |
| `ledger_index`           | String or Number | _(Optional)_ The [ledger index][] of the ledger to use, or a shortcut string to choose a ledger automatically. (See [Specifying Ledgers][]) |
| `limit`                  | Number           | _(Optional)_ The maximum number of objects to include in the results. Must be within the inclusive range `10` to `400` on non-admin connections. The default is `200`. |
| `marker`                 | [Marker][]       | _(Optional)_ Value from a previous paginated response. Resume retrieving data where that response left off. |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": 8,
    "status": "success",
    "type": "response",
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "account_objects": [
            {
                "Balance": {
                    "currency": "ASP",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "ASP",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "ASP",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "10"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "BF7555B0F018E3C5E2A3FF9437A1A5092F32903BE246202F988181B9CED0D862",
                "PreviousTxnLgrSeq": 1438879,
                "index": "2243B0B630EA6F7330B654EFA53E27A7609D9484E535AB11B7F946DF3D247CE9"
            },
            {
                "Balance": {
                    "currency": "XAU",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 3342336,
                "HighLimit": {
                    "currency": "XAU",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "XAU",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "0"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "79B26D7D34B950AC2C2F91A299A6888FABB376DD76CFF79D56E805BF439F6942",
                "PreviousTxnLgrSeq": 5982530,
                "index": "9ED4406351B7A511A012A9B5E7FE4059FA2F7650621379C0013492C315E25B97"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6FE8C824364FB1195BCFEDCB368DFEE3980F7F78D3BF4DC4174BB4C86CF8C5CE",
                "PreviousTxnLgrSeq": 10555014,
                "index": "2DECFAC23B77D5AEA6116C15F5C6D4669EBAEE9E7EE050A40FE2B1E47B6A9419"
            },
            {
                "Balance": {
                    "currency": "MXN",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "481.992867407479"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "MXN",
                    "issuer": "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "MXN",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1000"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "A467BACE5F183CDE1F075F72435FE86BAD8626ED1048EDEFF7562A4CC76FD1C5",
                "PreviousTxnLgrSeq": 3316170,
                "index": "EC8B9B6B364AF6CB6393A423FDD2DDBA96375EC772E6B50A3581E53BFBDFDD9A"
            },
            {
                "Balance": {
                    "currency": "EUR",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0.793598266778297"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "EUR",
                    "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "EUR",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "E9345D44433EA368CFE1E00D84809C8E695C87FED18859248E13662D46A0EC46",
                "PreviousTxnLgrSeq": 5447146,
                "index": "4513749B30F4AF8DA11F077C448128D6486BF12854B760E4E5808714588AA915"
            },
            {
                "Balance": {
                    "currency": "CNY",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 2228224,
                "HighLimit": {
                    "currency": "CNY",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CNY",
                    "issuer": "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
                    "value": "0"
                },
                "LowNode": "0000000000000008",
                "PreviousTxnID": "2FDDC81F4394695B01A47913BEC4281AC9A283CC8F903C14ADEA970F60E57FCF",
                "PreviousTxnLgrSeq": 5949673,
                "index": "578C327DA8944BDE2E10C9BA36AFA2F43E06C8D1E8819FB225D266CBBCFDE5CE"
            },
            {
                "Balance": {
                    "currency": "DYM",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "1.336889190631542"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "DYM",
                    "issuer": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "DYM",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6DA2BD02DFB83FA4DAFC2651860B60071156171E9C021D9E0372A61A477FFBB1",
                "PreviousTxnLgrSeq": 8818732,
                "index": "5A2A5FF12E71AEE57564E624117BBA68DEF78CD564EF6259F92A011693E027C7"
            },
            {
                "Balance": {
                    "currency": "CHF",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-0.3488146605801446"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "CHF",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CHF",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000008C",
                "PreviousTxnID": "722394372525A13D1EAAB005642F50F05A93CF63F7F472E0F91CDD6D38EB5869",
                "PreviousTxnLgrSeq": 2687590,
                "index": "F2DBAD20072527F6AD02CE7F5A450DBC72BE2ABB91741A8A3ADD30D5AD7A99FB"
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
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "BTC",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "0000000000000043",
                "PreviousTxnID": "03EDF724397D2DEE70E49D512AECD619E9EA536BE6CFD48ED167AE2596055C9A",
                "PreviousTxnLgrSeq": 8317037,
                "index": "767C12AF647CDF5FEB9019B37018748A79C50EDAF87E8D4C7F39F78AA7CA9765"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-16.00534471983042"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5000"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000004A",
                "PreviousTxnID": "CFFF5CFE623C9543308C6529782B6A6532207D819795AAFE85555DB8BF390FE7",
                "PreviousTxnLgrSeq": 14365854,
                "index": "826CF5BFD28F3934B518D0BDF3231259CBD3FD0946E3C3CA0C97D2C75D2D1A09"
            }
        ],
        "ledger_hash": "053DF17D2289D1C4971C22F235BC1FCA7D4B3AE966F842E5819D0749E0B8ECD3",
        "ledger_index": 14378733,
        "limit": 10,
        "marker": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93,94A9F05FEF9A153229E2E997E64919FD75AAE2028C8153E8EBDB4440BD3ECBB5",
        "validated": true
    }
}
```

*JSON-RPC*

```json
200 OK
{
    "result": {
        "account": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
        "account_objects": [
            {
                "Balance": {
                    "currency": "ASP",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "ASP",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "ASP",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "10"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "BF7555B0F018E3C5E2A3FF9437A1A5092F32903BE246202F988181B9CED0D862",
                "PreviousTxnLgrSeq": 1438879,
                "index": "2243B0B630EA6F7330B654EFA53E27A7609D9484E535AB11B7F946DF3D247CE9"
            },
            {
                "Balance": {
                    "currency": "XAU",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 3342336,
                "HighLimit": {
                    "currency": "XAU",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "XAU",
                    "issuer": "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
                    "value": "0"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "79B26D7D34B950AC2C2F91A299A6888FABB376DD76CFF79D56E805BF439F6942",
                "PreviousTxnLgrSeq": 5982530,
                "index": "9ED4406351B7A511A012A9B5E7FE4059FA2F7650621379C0013492C315E25B97"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6FE8C824364FB1195BCFEDCB368DFEE3980F7F78D3BF4DC4174BB4C86CF8C5CE",
                "PreviousTxnLgrSeq": 10555014,
                "index": "2DECFAC23B77D5AEA6116C15F5C6D4669EBAEE9E7EE050A40FE2B1E47B6A9419"
            },
            {
                "Balance": {
                    "currency": "MXN",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "481.992867407479"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "MXN",
                    "issuer": "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "MXN",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1000"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "A467BACE5F183CDE1F075F72435FE86BAD8626ED1048EDEFF7562A4CC76FD1C5",
                "PreviousTxnLgrSeq": 3316170,
                "index": "EC8B9B6B364AF6CB6393A423FDD2DDBA96375EC772E6B50A3581E53BFBDFDD9A"
            },
            {
                "Balance": {
                    "currency": "EUR",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0.793598266778297"
                },
                "Flags": 1114112,
                "HighLimit": {
                    "currency": "EUR",
                    "issuer": "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "EUR",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "1"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "E9345D44433EA368CFE1E00D84809C8E695C87FED18859248E13662D46A0EC46",
                "PreviousTxnLgrSeq": 5447146,
                "index": "4513749B30F4AF8DA11F077C448128D6486BF12854B760E4E5808714588AA915"
            },
            {
                "Balance": {
                    "currency": "CNY",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "0"
                },
                "Flags": 2228224,
                "HighLimit": {
                    "currency": "CNY",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CNY",
                    "issuer": "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
                    "value": "0"
                },
                "LowNode": "0000000000000008",
                "PreviousTxnID": "2FDDC81F4394695B01A47913BEC4281AC9A283CC8F903C14ADEA970F60E57FCF",
                "PreviousTxnLgrSeq": 5949673,
                "index": "578C327DA8944BDE2E10C9BA36AFA2F43E06C8D1E8819FB225D266CBBCFDE5CE"
            },
            {
                "Balance": {
                    "currency": "DYM",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "1.336889190631542"
                },
                "Flags": 65536,
                "HighLimit": {
                    "currency": "DYM",
                    "issuer": "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "DYM",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "LowNode": "0000000000000000",
                "PreviousTxnID": "6DA2BD02DFB83FA4DAFC2651860B60071156171E9C021D9E0372A61A477FFBB1",
                "PreviousTxnLgrSeq": 8818732,
                "index": "5A2A5FF12E71AEE57564E624117BBA68DEF78CD564EF6259F92A011693E027C7"
            },
            {
                "Balance": {
                    "currency": "CHF",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-0.3488146605801446"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "CHF",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "0"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "CHF",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000008C",
                "PreviousTxnID": "722394372525A13D1EAAB005642F50F05A93CF63F7F472E0F91CDD6D38EB5869",
                "PreviousTxnLgrSeq": 2687590,
                "index": "F2DBAD20072527F6AD02CE7F5A450DBC72BE2ABB91741A8A3ADD30D5AD7A99FB"
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
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "3"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "BTC",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "0000000000000043",
                "PreviousTxnID": "03EDF724397D2DEE70E49D512AECD619E9EA536BE6CFD48ED167AE2596055C9A",
                "PreviousTxnLgrSeq": 8317037,
                "index": "767C12AF647CDF5FEB9019B37018748A79C50EDAF87E8D4C7F39F78AA7CA9765"
            },
            {
                "Balance": {
                    "currency": "USD",
                    "issuer": "rrrrrrrrrrrrrrrrrrrrBZbvji",
                    "value": "-16.00534471983042"
                },
                "Flags": 131072,
                "HighLimit": {
                    "currency": "USD",
                    "issuer": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
                    "value": "5000"
                },
                "HighNode": "0000000000000000",
                "LedgerEntryType": "RippleState",
                "LowLimit": {
                    "currency": "USD",
                    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
                    "value": "0"
                },
                "LowNode": "000000000000004A",
                "PreviousTxnID": "CFFF5CFE623C9543308C6529782B6A6532207D819795AAFE85555DB8BF390FE7",
                "PreviousTxnLgrSeq": 14365854,
                "index": "826CF5BFD28F3934B518D0BDF3231259CBD3FD0946E3C3CA0C97D2C75D2D1A09"
            }
        ],
        "ledger_hash": "4C99E5F63C0D0B1C2283B4F5DCE2239F80CE92E8B1A6AED1E110C198FC96E659",
        "ledger_index": 14380380,
        "limit": 10,
        "marker": "F60ADF645E78B69857D2E4AEC8B7742FEABC8431BD8611D099B428C3E816DF93,94A9F05FEF9A153229E2E997E64919FD75AAE2028C8153E8EBDB4440BD3ECBB5",
        "status": "success",
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "account" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
      "account_objects" : [
         {
            "Balance" : {
               "currency" : "ASP",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 65536,
            "HighLimit" : {
               "currency" : "ASP",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "ASP",
               "issuer" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
               "value" : "10"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "BF7555B0F018E3C5E2A3FF9437A1A5092F32903BE246202F988181B9CED0D862",
            "PreviousTxnLgrSeq" : 1438879,
            "index" : "2243B0B630EA6F7330B654EFA53E27A7609D9484E535AB11B7F946DF3D247CE9"
         },
         {
            "Balance" : {
               "currency" : "XAU",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 3342336,
            "HighLimit" : {
               "currency" : "XAU",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "XAU",
               "issuer" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "79B26D7D34B950AC2C2F91A299A6888FABB376DD76CFF79D56E805BF439F6942",
            "PreviousTxnLgrSeq" : 5982530,
            "index" : "9ED4406351B7A511A012A9B5E7FE4059FA2F7650621379C0013492C315E25B97"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "5"
            },
            "Flags" : 1114112,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "rMwjYedjc7qqtKYVLiAccJSmCwih4LnE2q",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "5"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "2A5C5D95880A254A2C57BB5332558205BC33B9F2B38D359A170283CB4CBD080A",
            "PreviousTxnLgrSeq" : 39498567,
            "index" : "2DECFAC23B77D5AEA6116C15F5C6D4669EBAEE9E7EE050A40FE2B1E47B6A9419"
         },
         {
            "Balance" : {
               "currency" : "MXN",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "481.992867407479"
            },
            "Flags" : 65536,
            "HighLimit" : {
               "currency" : "MXN",
               "issuer" : "rHpXfibHgSb64n8kK9QWDpdbfqSpYbM9a4",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "MXN",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "1000"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "A467BACE5F183CDE1F075F72435FE86BAD8626ED1048EDEFF7562A4CC76FD1C5",
            "PreviousTxnLgrSeq" : 3316170,
            "index" : "EC8B9B6B364AF6CB6393A423FDD2DDBA96375EC772E6B50A3581E53BFBDFDD9A"
         },
         {
            "Balance" : {
               "currency" : "EUR",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0.793598266778297"
            },
            "Flags" : 1114112,
            "HighLimit" : {
               "currency" : "EUR",
               "issuer" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "EUR",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "1"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "E9345D44433EA368CFE1E00D84809C8E695C87FED18859248E13662D46A0EC46",
            "PreviousTxnLgrSeq" : 5447146,
            "index" : "4513749B30F4AF8DA11F077C448128D6486BF12854B760E4E5808714588AA915"
         },
         {
            "Balance" : {
               "currency" : "CNY",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "CNY",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "3"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "CNY",
               "issuer" : "rnuF96W4SZoCJmbHYBFoJZpR8eCaxNvekK",
               "value" : "0"
            },
            "LowNode" : "0000000000000008",
            "PreviousTxnID" : "2FDDC81F4394695B01A47913BEC4281AC9A283CC8F903C14ADEA970F60E57FCF",
            "PreviousTxnLgrSeq" : 5949673,
            "index" : "578C327DA8944BDE2E10C9BA36AFA2F43E06C8D1E8819FB225D266CBBCFDE5CE"
         },
         {
            "Balance" : {
               "currency" : "DYM",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "1.336889190631542"
            },
            "Flags" : 65536,
            "HighLimit" : {
               "currency" : "DYM",
               "issuer" : "rGwUWgN5BEg3QGNY3RX2HfYowjUTZdid3E",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "DYM",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "3"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "6DA2BD02DFB83FA4DAFC2651860B60071156171E9C021D9E0372A61A477FFBB1",
            "PreviousTxnLgrSeq" : 8818732,
            "index" : "5A2A5FF12E71AEE57564E624117BBA68DEF78CD564EF6259F92A011693E027C7"
         },
         {
            "Balance" : {
               "currency" : "CHF",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-0.3488146605801446"
            },
            "Flags" : 131072,
            "HighLimit" : {
               "currency" : "CHF",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "CHF",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "0"
            },
            "LowNode" : "000000000000008C",
            "PreviousTxnID" : "722394372525A13D1EAAB005642F50F05A93CF63F7F472E0F91CDD6D38EB5869",
            "PreviousTxnLgrSeq" : 2687590,
            "index" : "F2DBAD20072527F6AD02CE7F5A450DBC72BE2ABB91741A8A3ADD30D5AD7A99FB"
         },
         {
            "Balance" : {
               "currency" : "BTC",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 131072,
            "HighLimit" : {
               "currency" : "BTC",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "3"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "BTC",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "0"
            },
            "LowNode" : "0000000000000043",
            "PreviousTxnID" : "03EDF724397D2DEE70E49D512AECD619E9EA536BE6CFD48ED167AE2596055C9A",
            "PreviousTxnLgrSeq" : 8317037,
            "index" : "767C12AF647CDF5FEB9019B37018748A79C50EDAF87E8D4C7F39F78AA7CA9765"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-11.68225001668339"
            },
            "Flags" : 131072,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "5000"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "0"
            },
            "LowNode" : "000000000000004A",
            "PreviousTxnID" : "8C55AFC2A2AA42B5CE624AEECDB3ACFDD1E5379D4E5BF74A8460C5E97EF8706B",
            "PreviousTxnLgrSeq" : 43251698,
            "index" : "826CF5BFD28F3934B518D0BDF3231259CBD3FD0946E3C3CA0C97D2C75D2D1A09"
         },
         {
            "Balance" : {
               "currency" : "BTC",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0.00111"
            },
            "Flags" : 65536,
            "HighLimit" : {
               "currency" : "BTC",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "BTC",
               "issuer" : "rpgKWEmNqSDAGFhy5WDnsyPqfQxbWxKeVd",
               "value" : "10"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "74F2F2A731C1350492FA03F8C67AF6C05EEC391160AACC04BF99329D9EAB0052",
            "PreviousTxnLgrSeq" : 585437,
            "index" : "94A9F05FEF9A153229E2E997E64919FD75AAE2028C8153E8EBDB4440BD3ECBB5"
         },
         {
            "Balance" : {
               "currency" : "BTC",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-0.0008744482690504699"
            },
            "Flags" : 131072,
            "HighLimit" : {
               "currency" : "BTC",
               "issuer" : "rBJ3YjwXi2MGbg7GVLuTXUWQ8DjL7tDXh4",
               "value" : "10"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "BTC",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "95221CD154317176B5748202A2E820D7A336597816E9787C27E3E4F25576877F",
            "PreviousTxnLgrSeq" : 8208104,
            "index" : "BC2AC65D7F9AD5CDAD131DEFE248727CA8A0FC219A33A3264E6202F50B4733C0"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 65536,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "rLEsXccBGNR3UPuPu2hUXPjziKC3qKSBun",
               "value" : "0"
            },
            "HighNode" : "0000000000000002",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "1"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "8C55AFC2A2AA42B5CE624AEECDB3ACFDD1E5379D4E5BF74A8460C5E97EF8706B",
            "PreviousTxnLgrSeq" : 43251698,
            "index" : "C493ABA2619D0FC6355BA862BC8312DF8266FBE76AFBA9636E857F7EAC874A99"
         },
         {
            "Balance" : {
               "currency" : "CNY",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-9.07619790068559"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "CNY",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "100"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "CNY",
               "issuer" : "razqQKzJRdB4UxFPWf5NEpEG3WMkmwgcXA",
               "value" : "0"
            },
            "LowNode" : "0000000000000005",
            "PreviousTxnID" : "8EFE067DA1B2D57C485EFFCF875604548BA990EA00D019B466313D4CEE1E4668",
            "PreviousTxnLgrSeq" : 8284705,
            "index" : "C8554E6CE903505F631703E73D22D2D4D0662FDA9F524290997DF6B4D760C495"
         },
         {
            "Balance" : {
               "currency" : "JPY",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-7.292695098901099"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "JPY",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "JPY",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "0"
            },
            "LowNode" : "0000000000000076",
            "PreviousTxnID" : "CE460D85F9AD93752E8EC8BCF4DFFE91DDF08B8C988836D64CB51EB7B447F672",
            "PreviousTxnLgrSeq" : 5100078,
            "index" : "CB1565898F19916A5EE49CC537B1D43CA075B9B96E82C6892E16EF6DFDEE7865"
         },
         {
            "Balance" : {
               "currency" : "AUX",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 3342336,
            "HighLimit" : {
               "currency" : "AUX",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "AUX",
               "issuer" : "r3vi7mWxru9rJCxETCyA1CHvzL96eZWx5z",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "A6964735A14272742E7E10EB5AB5A7CE693473BBB4DAFA488971A15E181132CF",
            "PreviousTxnLgrSeq" : 5982528,
            "index" : "C61E113C767A9E7B27CD944162FB63EAA24C38C8664E984759821C3ADFFE097E"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0.0004557360418801623"
            },
            "Flags" : 1114112,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "r9vbV3EHvXWjSkeQ6CAcYVPGeq7TuiXY2X",
               "value" : "0"
            },
            "HighNode" : "0000000000000003",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "1"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "F8F178712C0C3DA013A774A598B2A004BE6BA3D4542B8448D221A60D8A0D409A",
            "PreviousTxnLgrSeq" : 38837233,
            "index" : "D43180D7B2EEBB285C9D296590C4D5E5580C814F3026FC4D41FFDF3049FB547F"
         },
         {
            "Balance" : {
               "currency" : "EUR",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-12.41688780720394"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "EUR",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "100"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "0"
            },
            "LowNode" : "000000000000008F",
            "PreviousTxnID" : "1F0436C0688A8156EE2D7745B02C3F39C477A43F6BAD5D46C25A08949EF41739",
            "PreviousTxnLgrSeq" : 5449071,
            "index" : "F6B22B4D6A83B13A7F16E6A4A6EA8D3E26739C9D86C23DAC21C18E74B5E2C8CC"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-35"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "500"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "rfF3PNkwkq1DygW2wum2HK3RGfgkJjdPVD",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "99CB3C30FAEC301621951B944059C3C2EB78FDE7ED2E124A79901B6F2600E868",
            "PreviousTxnLgrSeq" : 4339260,
            "index" : "D23CF25053AC6A5106E9162A20AF52818EDC672CA77AD1625C0407A71D37102F"
         },
         {
            "Balance" : {
               "currency" : "JOE",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "-5"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "JOE",
               "issuer" : "rwUVoVMSURqNyvocPCcvLu3ygJzZyw8qwp",
               "value" : "50"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "JOE",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "F5B48F7631779C6562AF07DC79E0E4E25A0696C38823417BF811DF27C5D88168",
            "PreviousTxnLgrSeq" : 5736288,
            "index" : "FEECE8973F75156412E1604C52B8B9C6BC9EF21FA4A0FAA8B779AC416D039B34"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
               "value" : "100"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "B1A7405C4A698E6A371E5B02836E779A942936AB754865FE82141E5280F09D1B",
            "PreviousTxnLgrSeq" : 5718137,
            "index" : "8DF1456AAB7470A760F6A095C156B457FF1038D43E6B11FD8011C2DF714E4FA1"
         },
         {
            "Balance" : {
               "currency" : "JOE",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "JOE",
               "issuer" : "rE6R3DWF9fBD7CyiQciePF9SqK58Ubp8o2",
               "value" : "100"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "JOE",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "8E488B0E939D4DACD62102A5BFA2FDC63679EFCC56F2FDA2FDF45283674BB711",
            "PreviousTxnLgrSeq" : 5989200,
            "index" : "273BD42DD72E7D84416ED759CEC92DACCD12A4502287E50BECF816233C021ED1"
         },
         {
            "Balance" : {
               "currency" : "015841551A748AD2C1F76FF6ECB0CCCD00000000",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 2228224,
            "HighLimit" : {
               "currency" : "015841551A748AD2C1F76FF6ECB0CCCD00000000",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "10.01037626125837"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "015841551A748AD2C1F76FF6ECB0CCCD00000000",
               "issuer" : "rs9M85karFkCRjvc6KMWn8Coigm9cbcgcx",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "2B3E313FBDE15988425AACA1EA2EAEBBBCB8020E4FBAA7159BA678E1C4F6B4C3",
            "PreviousTxnLgrSeq" : 5982458,
            "index" : "BA92A0B9EB8A75E84C5463BA1A055F2B1C1B7CC20BFDA7B027C685F75E06629D"
         },
         {
            "Balance" : {
               "currency" : "USD",
               "issuer" : "rrrrrrrrrrrrrrrrrrrrBZbvji",
               "value" : "0"
            },
            "Flags" : 131072,
            "HighLimit" : {
               "currency" : "USD",
               "issuer" : "rEhDDUUNxpXgEHVJtC2cjXAgyx5VCFxdMF",
               "value" : "1"
            },
            "HighNode" : "0000000000000000",
            "LedgerEntryType" : "RippleState",
            "LowLimit" : {
               "currency" : "USD",
               "issuer" : "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
               "value" : "0"
            },
            "LowNode" : "0000000000000000",
            "PreviousTxnID" : "B6B410172C0B65575D89E464AF5B99937CC568822929ABF87DA75CBD11911932",
            "PreviousTxnLgrSeq" : 6592159,
            "index" : "2CC2B211F6D1159B5CFD07AF8717A9C51C985E2497B2875C192EE87266AB0F81"
         }
      ],
      "ledger_hash" : "06BF4DBE7D57FBFEAFD70D4CA7B00ED6EF404B94C446767063A0EF58A937FC4E",
      "ledger_index" : 56843766,
      "status" : "success",
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                      | Description             |
|:-----------------------|:--------------------------|:------------------------|
| `account`              | String                    | 此请求对应的帐户的唯一[地址][] |
| `account_objects`      | Array                     | 此帐户拥有的对象数组。每个对象都是原始的[分类帐格式][]. |
| `ledger_hash`          | String                    | (可以省略) 用于生成此响应的分类帐的标识哈希。 |
| `ledger_index`         | Number - [Ledger Index][] | _(可以省略)_ 用于生成此响应的分类帐版本的分类帐索引。 |
| `ledger_current_index` | Number - [Ledger Index][] | _(可以省略)_ 用于生成此响应的当前进行中分类帐版本的分类帐索引。 |
| `limit`                | Number                    | _(可以省略)_ 此请求中使用的限制（如果有）。 |
| `marker`               | [Marker][]                | 服务器定义的值，指示响应已分页。将此消息传递给下一个呼叫以继续此呼叫中断的位置。此页之后没有其他页时省略。 |
| `validated`            | Boolean                   | 如果包含并设置为`true`，则此响应中的信息来自已验证的分类帐版本。否则，信息可能会更改。 |

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actNotFound` - 在请求的`帐户`字段中指定的[地址][]与分类帐中的帐户不对应。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
