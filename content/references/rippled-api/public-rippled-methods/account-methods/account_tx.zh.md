---
html: account_tx.html
parent: account-methods.html
blurb: Get info about an account's transactions.
---
# account_tx
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/AccountTx.cpp "Source")

`account_tx`方法检索涉及指定帐户的事务列表.

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "command": "account_tx",
  "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
  "ledger_index_min": -1,
  "ledger_index_max": -1,
  "binary": false,
  "limit": 2,
  "forward": false
}
```

*JSON-RPC*

```json
{
    "method": "account_tx",
    "params": [
        {
            "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
            "binary": false,
            "forward": false,
            "ledger_index_max": -1,
            "ledger_index_min": -1,
            "limit": 2
        }
    ]
}
```

*Commandline*

```sh
# Syntax: account_tx account [ledger_index_min [ledger_index_max]] [limit] [offset] [binary] [count] [descending]
# For binary/count/descending, use the parameter name for true and omit for false.
rippled -- account_tx rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w -1 -1 2 0 binary descending
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#account_tx)

请求包括以下参数:

| `Field`            | Type                                       | Description |
|:-------------------|:-------------------------------------------|:-----------|
| `account`          | String                                     | 帐户的唯一标识符，通常是帐户的地址。 |
| `ledger_index_min` | Integer                                    | _(可选)_ 用于指定要包含交易记录的最早分类帐。值`-1`指示服务器使用可用的最早已验证的分类帐版本。 |
| `ledger_index_max` | Integer                                    | _(可选)_ 用于指定要包含交易记录的最新分类帐。值`-1`指示服务器使用可用的最新已验证的分类帐版本。 |
| `ledger_hash`      | String                                     | _(可选)_ 用于仅从单个分类帐中查找交易记录。 (参见[Specifying Ledgers][].) |
| `ledger_index`     | String or Unsigned Integer                 | _(可选)_ 用于仅从单个分类帐中查找交易记录。  (参见[Specifying Ledgers][].) |
| `binary`           | Boolean                                    | _(可选)_ 默认为`false`。如果设置为`true`，则将事务作为十六进制字符串而不是JSON返回。 |
| `forward`          | Boolean                                    | _(可选)_ 默认为`false`。如果设置为`true`，则首先返回用最旧分类帐编制索引的值。否则，结果将首先使用最新的分类帐编制索引(结果的每一页可能不是内部排序的，而是整体排序的。） |
| `limit`            | Integer                                    | _(可选)_ 默认值不同。限制要检索的事务数。服务器不需要接受此值。 |
| `marker`           | [Marker][] | 来自上一个分页响应的值。继续检索停止响应的数据。即使服务器的可用分类帐范围发生变化，该值也是稳定的。 |

**必须至少使用以下字段之一** 在请求中: `ledger_index`, `ledger_hash`, `ledger_index_min`, or `ledger_index_max`.

不再支持以下旧字段: `offset`, `count`, `ledger_min`, `ledger_max`. [Removed in: rippled 1.7.0][]


### Iterating over queried data

与其他分页方法一样，可以使用`标记符`字段返回多页数据。在两次请求之间的时间内，`"ledger index min":-1`和`"ledger index max":-1`可能会更改，以引用与以前不同的分类帐版本。`标记符`字段可以安全地分页，即使请求中的分类帐范围发生更改，只要标记符未指示请求中指定的分类帐范围之外的点


## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 2,
  "result": {
    "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
    "ledger_index_max": 57111999,
    "ledger_index_min": 55886305,
    "limit": 2,
    "marker": {
      "ledger": 57111981,
      "seq": 16
    },
    "transactions": [
      {
        "meta": {
          "AffectedNodes": [
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                  "Balance": "3732969177079",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 702817
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                "PreviousFields": {
                  "Balance": "3713891690008"
                },
                "PreviousTxnID": "D58864C16344ADCC15995C7986CFC607CB693E88F84D2E019F0A35FB29749202",
                "PreviousTxnLgrSeq": 57111994
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                  "Balance": "40010160",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 466334
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9",
                "PreviousFields": {
                  "Balance": "19117497271",
                  "Sequence": 466333
                },
                "PreviousTxnID": "F6B8274D3D419A95A59681E5F55578084C395FF9051924360CA3EA745F5581E8",
                "PreviousTxnLgrSeq": 57111993
              }
            }
          ],
          "TransactionIndex": 25,
          "TransactionResult": "tesSUCCESS",
          "delivered_amount": "19077487071"
        },
        "tx": {
          "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
          "Amount": "19077487071",
          "Destination": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
          "DestinationTag": 1,
          "Fee": "40",
          "Flags": 2147483648,
          "LastLedgerSequence": 57112020,
          "Sequence": 466333,
          "SigningPubKey": "0381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A01",
          "TransactionType": "Payment",
          "TxnSignature": "3045022100CFC5FD057C7C685C690637AD1E639E2642BBC00EFD8E06E3F6C72FA924BC99D40220317D0708E814F69F874D641B6732E37A53B1220B493B2B8390D9EF51E8062515",
          "date": 649200260,
          "hash": "46BF0B576677B0DEA2D94591424A57A2DE8E3D89383631E16F40D09A513C656C",
          "inLedger": 57111998,
          "ledger_index": 57111998
        },
        "validated": true
      },
      {
        "meta": {
          "AffectedNodes": [
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                  "Balance": "3713891690008",
                  "Flags": 131072,
                  "OwnerCount": 0,
                  "Sequence": 702817
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                "PreviousFields": {
                  "Balance": "3714441690048",
                  "Sequence": 702816
                },
                "PreviousTxnID": "FDD5007913B39027BAF10B31144DBC1F7DC147528DF31FF048A06DC5D3108BD6",
                "PreviousTxnLgrSeq": 57111981
              }
            },
            {
              "ModifiedNode": {
                "FinalFields": {
                  "Account": "r9dU6Z7P2i7MrDi1VUZ7uyq6J77eg86YtB",
                  "Balance": "2629998983",
                  "Flags": 0,
                  "OwnerCount": 0,
                  "Sequence": 10
                },
                "LedgerEntryType": "AccountRoot",
                "LedgerIndex": "27B96FE681B33825CC95DA197358B30D3A1721F2125F2D76022D46B2418ABA0A",
                "PreviousFields": {
                  "Balance": "2079998983"
                },
                "PreviousTxnID": "44A47AC04C0C7237C32BE9A532B578D07641705D3A59DB9B3C5B6225001E39B7",
                "PreviousTxnLgrSeq": 56613857
              }
            }
          ],
          "TransactionIndex": 16,
          "TransactionResult": "tesSUCCESS",
          "delivered_amount": "550000000"
        },
        "tx": {
          "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
          "Amount": "550000000",
          "Destination": "r9dU6Z7P2i7MrDi1VUZ7uyq6J77eg86YtB",
          "Fee": "40",
          "Flags": 2147483648,
          "LastLedgerSequence": 57112016,
          "Sequence": 702816,
          "SigningPubKey": "020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A",
          "TransactionType": "Payment",
          "TxnSignature": "3045022100D589029EF63F9E528F6100C7A36D26AFFF84085EC9AC16DA8E30E11F390D4E87022011466E0FE4A90B89142EE47E535545EEA4A2D65E0BD234DFB447721218B59C9B",
          "date": 649200241,
          "hash": "D58864C16344ADCC15995C7986CFC607CB693E88F84D2E019F0A35FB29749202",
          "inLedger": 57111994,
          "ledger_index": 57111994
        },
        "validated": true
      }
    ],
    "validated": true
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
        "account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
        "ledger_index_max": 57112019,
        "ledger_index_min": 56248229,
        "limit": 2,
        "marker": {
            "ledger": 57112007,
            "seq": 13
        },
        "status": "success",
        "transactions": [
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                                    "Balance": "3732290013101",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 702820
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                                "PreviousFields": {
                                    "Balance": "3732745656171",
                                    "Sequence": 702819
                                },
                                "PreviousTxnID": "7C031FD5B710E3C048EEF31254089BEEC505900BCC9A842257A0319453333998",
                                "PreviousTxnLgrSeq": 57112010
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "raLPjTYeGezfdb6crXZzcC8RkLBEwbBHJ5",
                                    "Balance": "4231510602153",
                                    "Flags": 0,
                                    "OwnerCount": 0,
                                    "Sequence": 96486
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "39DC5D448DECEFC3CD20818788E3DA891CA943935E8D7B12FCB5B5871FCB1638",
                                "PreviousFields": {
                                    "Balance": "4231054959123"
                                },
                                "PreviousTxnID": "33D2014C832610293730028CA37857AC183BFCE3E42B9979C491FB8B82B3E9DC",
                                "PreviousTxnLgrSeq": 57112004
                            }
                        }
                    ],
                    "TransactionIndex": 12,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "455643030"
                },
                "tx": {
                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                    "Amount": "455643030",
                    "Destination": "raLPjTYeGezfdb6crXZzcC8RkLBEwbBHJ5",
                    "DestinationTag": 18240312,
                    "Fee": "40",
                    "Flags": 2147483648,
                    "LastLedgerSequence": 57112037,
                    "Sequence": 702819,
                    "SigningPubKey": "020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A",
                    "TransactionType": "Payment",
                    "TxnSignature": "30450221008602B2E390C0C7B65182C6DBC86292052C1961B2BEFB79C2C8431722C0ADB911022024B74DCF910A4C8C95572CF662EB7F5FF67E1AC4D7B9B7BFE2A8EE851EC16576",
                    "date": 649200322,
                    "hash": "08EF5BDA2825D7A28099219621CDBECCDECB828FEA202DEB6C7ACD5222D36C2C",
                    "inLedger": 57112015,
                    "ledger_index": 57112015
                },
                "validated": true
            },
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                                    "Balance": "3732745656171",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 702819
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574",
                                "PreviousFields": {
                                    "Balance": "3732246155784"
                                },
                                "PreviousTxnID": "CCBCCB528F602007C937C496F0828C118E073DF180084CCD3646EC1E414844E4",
                                "PreviousTxnLgrSeq": 57112007
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                                    "Balance": "236476361",
                                    "Flags": 131072,
                                    "OwnerCount": 0,
                                    "Sequence": 466335
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9",
                                "PreviousFields": {
                                    "Balance": "735976788",
                                    "Sequence": 466334
                                },
                                "PreviousTxnID": "C528B32DD588EFAE2FE833E8AA92E6AE2DF2C8DB3DB8C6C4F334AD37B253D72A",
                                "PreviousTxnLgrSeq": 57112010
                            }
                        }
                    ],
                    "TransactionIndex": 33,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "499500387"
                },
                "tx": {
                    "Account": "rw2ciyaNshpHe7bCHo4bRWq6pqqynnWKQg",
                    "Amount": "499500387",
                    "Destination": "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
                    "DestinationTag": 1,
                    "Fee": "40",
                    "Flags": 2147483648,
                    "LastLedgerSequence": 57112032,
                    "Sequence": 466334,
                    "SigningPubKey": "0381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A01",
                    "TransactionType": "Payment",
                    "TxnSignature": "3045022100C7EA1701FE48C75508EEBADBC9864CD3FFEDCEB48AB99AEA960BFA360AE163ED0220453C9577502924C9E1A9A450D4B950A44016813BC70E1F16A65A402528D730B7",
                    "date": 649200302,
                    "hash": "7C031FD5B710E3C048EEF31254089BEEC505900BCC9A842257A0319453333998",
                    "inLedger": 57112010,
                    "ledger_index": 57112010
                },
                "validated": true
            }
        ],
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "account" : "rLNaPoKeeBjZe2qs6x52yVPZpZ8td4dc6w",
      "ledger_index_max" : 57112094,
      "ledger_index_min" : 57105464,
      "limit" : 2,
      "marker" : {
         "ledger" : 57112074,
         "seq" : 9
      },
      "status" : "success",
      "transactions" : [
         {
            "ledger_index" : 57112090,
            "meta" : "201C0000002EF8E51100612503677617551E0297F38EF4FED7004E074D246B4EA3E550D9AE0F61BE40E08D3432091D52CE56140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574E624000AB96E624000037771BFD270E1E7220002000024000AB96F2D0000000062400003776C784A418114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911E1E1E311006156F7D315E0E992B1F1AC66B309C9D68961AA327FE770101B74D4C975F8C5DEC96AE8240367761A624000000005478807811403C95DC0C7CE402E8044A5F13304108013CE9963E1E1F1031000",
            "tx_blob" : "120000228000000024000AB96E201B036776306140000000054788076840000000000000287321020A46D8D02AC780C59853ACA309EAA92E7D8E02DD72A0B6AC315A7D18A6C3276A74463044022054811EEF61ACCFA1B5FC6BB05D2FA49CF5174062740370328382E6EA557C0E6A0220480584D487638C333A87CA37100354BD36209E355E8DB9FE79791A56E24C1F268114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911831403C95DC0C7CE402E8044A5F13304108013CE9963",
            "validated" : true
         },
         {
            "ledger_index" : 57112087,
            "meta" : "201C00000026F8E5110061250367760A556B80EE9A9AD3FC40F471F29DCB80C678375137CE36220718902EF1EDCD375E7156140FA03FE8C39540CA8189BC7A7956795C712BC0A542C6409C041150703C8574E66240000376DEB77118E1E7220002000024000AB96E2D00000000624000037771BFD2708114D2E44C9FAF7BE9C536219800A6E698E4C7D2C911E1E1E511006125036776155591DA498D40AFD90670555F3D719883B48D224B4E4E906C634DEFA21163E8197756CC20FEBEA6D2AF969EC46F2BD92684D9FBABC3F238E841B5E056FE4EBF4379A9E62400071DA26240000001C0D849F8E1E722000200002400071DA32D0000000062400000012DCFE87881146914CB622B8E41E150DE431F48DA244A69809366E1E1F1031000",
            "tx_blob" : "12000022800000002400071DA22E00000001201B0367762D61400000009308615868400000000000002873210381575032E254BF4D699C3D8D6EFDB63B3A71F97475C6F6885BC7DAEEE55D9A0174473045022100E592BCCFD85CCE0B39075EFC66D6BCA594EBB451F12AD5AD9EE533A267F1381B02203635AB46AC110848FC44E797BD19D77A19E10A0F463AA5540B1C62E5D48C81F081146914CB622B8E41E150DE431F48DA244A698093668314D2E44C9FAF7BE9C536219800A6E698E4C7D2C911",
            "validated" : true
         }
      ],
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`            | Type                       | Description                |
|:-------------------|:---------------------------|:---------------------------|
| `account`          | String                     | 标识相关帐户的唯一[地址][] |
| `ledger_index_min` | Integer - [Ledger Index][] | 实际搜索交易记录的最早分类帐的分类帐索引。 |
| `ledger_index_max` | Integer - [Ledger Index][] | 实际搜索交易记录的最新分类帐的分类帐索引。 |
| `limit`            | Integer                    | 请求中使用的`限制`值(这可能与服务器强制执行的实际限制值不同。） |
| `marker`           | [Marker][]                 | 服务器定义的值，指示响应已分页。将此消息传递给下一个呼叫以继续此呼叫中断的位置。 |
| `transactions`     | Array                      | 符合请求条件的事务数组，如下所述。 |
| `validated`        | Boolean                    | 如果包含并设置为`true`，则此响应中的信息来自已验证的分类帐版本。否则，信息可能会更改。 |

**Note:** 服务器的响应值可能不同于您在请求中提供的值`ledger_index_min`和`ledger_index_max`，例如，如果它没有您指定的现有版本。

每个事务对象都包括以下字段，具体取决于它是以JSON还是十六进制字符串（`"binary":true`）格式请求的。

| `Field`        | Type                             | Description              |
|:---------------|:---------------------------------|:-------------------------|
| `ledger_index` | Integer                          | 包含此交易记录的分类帐版本的[分类帐索引][]。 |
| `meta`         | Object (JSON) or String (Binary) | 如果`binary`为True，则这是事务元数据的十六进制字符串。否则，事务元数据将以JSON格式包含。 |
| `tx`           | Object                           | （仅限JSON模式）定义事务的JSON对象 |
| `tx_blob`      | String                           | （仅限二进制模式）表示事务的唯一哈希字符串。 |
| `validated`    | Boolean                          | 交易记录是否包含在已验证的分类帐中。任何尚未在有效分类账中的交易都可能发生变化。 |

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `actMalformed` - 在请求的`帐户`字段中指定的[地址][]格式不正确。
* `lgrIdxMalformed` - 由`ledger_index_min`或`ledger_index_max`指定的分类帐不存在，或者如果它确实存在但服务器没有它。
* `lgrIdxsInvalid` - 请求指定的`ledger_index_max`在`ledger_index_min`之前, 或者服务器没有经过验证的分类帐范围，因为它[未与网络同步](server-doesnt-sync.html).


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
