---
html: transaction_entry.html
parent: transaction-methods.html
blurb: Retrieve info about a transaction from a particular ledger version.
---
# transaction_entry
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/TransactionEntry.cpp "Source")

`transaction_entry`方法从特定分类帐版本检索单个交易的信息(相比之下，[tx method][]在所有分类帐中搜索指定的事务。我们建议改用这种方法。）

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 4,
  "command": "transaction_entry",
  "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
  "ledger_index": 56865245
}
```

*JSON-RPC*

```json
{
    "method": "transaction_entry",
    "params": [
        {
            "tx_hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
            "ledger_index": 56865245
        }
    ]
}
```

*Commandline*

```sh
#Syntax: transaction_entry transaction_hash ledger_index|ledger_hash
rippled transaction_entry C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9 56865245
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#transaction_entry)

请求包括以下参数:

| `Field`        | Type                       | Description                    |
|:---------------|:---------------------------|:-------------------------------|
| `ledger_hash`  | String                     | _(Optional)_ A 20-byte hex string for the ledger version to use. (See [Specifying Ledgers][]) |
| `ledger_index` | String or Unsigned Integer | _(Optional)_ The [ledger index][] of the ledger to use, or a shortcut string to choose a ledger automatically. (See [Specifying Ledgers][]) |
| `tx_hash`      | String                     | Unique hash of the transaction you are looking up |

**Note:** This method does not support retrieving information from the current in-progress ledger. You must specify a ledger version in either `ledger_index` or `ledger_hash`.

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 4,
  "result": {
    "ledger_hash": "793E56131D8D4ABFB27FA383BFC44F2978B046E023FF46C588D7E0C874C2472A",
    "ledger_index": 56865245,
    "metadata": {
      "AffectedNodes": [
        {
          "ModifiedNode": {
            "FinalFields": {
              "ExchangeRate": "4F04C66806CF7400",
              "Flags": 0,
              "RootIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "TakerGetsCurrency": "0000000000000000000000000000000000000000",
              "TakerGetsIssuer": "0000000000000000000000000000000000000000",
              "TakerPaysCurrency": "000000000000000000000000434E590000000000",
              "TakerPaysIssuer": "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "Balance": "10404767991",
              "Flags": 0,
              "OwnerCount": 3,
              "Sequence": 5037711
            },
            "LedgerEntryType": "AccountRoot",
            "LedgerIndex": "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
            "PreviousFields": {
              "Balance": "10404768003",
              "Sequence": 5037710
            },
            "PreviousTxnID": "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
            "PreviousTxnLgrSeq": 56865244
          }
        },
        {
          "DeletedNode": {
            "FinalFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "BookNode": "0000000000000000",
              "Flags": 0,
              "OwnerNode": "0000000000000000",
              "PreviousTxnID": "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
              "PreviousTxnLgrSeq": 56865244,
              "Sequence": 5037708,
              "TakerGets": "15000000000",
              "TakerPays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "20160.75"
              }
            },
            "LedgerEntryType": "Offer",
            "LedgerIndex": "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
          }
        },
        {
          "ModifiedNode": {
            "FinalFields": {
              "Flags": 0,
              "IndexNext": "0000000000000000",
              "IndexPrevious": "0000000000000000",
              "Owner": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "RootIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
            },
            "LedgerEntryType": "DirectoryNode",
            "LedgerIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
          }
        },
        {
          "CreatedNode": {
            "LedgerEntryType": "Offer",
            "LedgerIndex": "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
            "NewFields": {
              "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
              "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
              "Sequence": 5037710,
              "TakerGets": "15000000000",
              "TakerPays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "20160.75"
              }
            }
          }
        }
      ],
      "TransactionIndex": 0,
      "TransactionResult": "tesSUCCESS"
    },
    "tx_json": {
      "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
      "Fee": "12",
      "Flags": 0,
      "LastLedgerSequence": 56865248,
      "OfferSequence": 5037708,
      "Sequence": 5037710,
      "SigningPubKey": "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
      "TakerGets": "15000000000",
      "TakerPays": {
        "currency": "CNY",
        "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
        "value": "20160.75"
      },
      "TransactionType": "OfferCreate",
      "TxnSignature": "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
      "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
    },
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
        "ledger_hash": "793E56131D8D4ABFB27FA383BFC44F2978B046E023FF46C588D7E0C874C2472A",
        "ledger_index": 56865245,
        "metadata": {
            "AffectedNodes": [
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "ExchangeRate": "4F04C66806CF7400",
                            "Flags": 0,
                            "RootIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "TakerGetsCurrency": "0000000000000000000000000000000000000000",
                            "TakerGetsIssuer": "0000000000000000000000000000000000000000",
                            "TakerPaysCurrency": "000000000000000000000000434E590000000000",
                            "TakerPaysIssuer": "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "Balance": "10404767991",
                            "Flags": 0,
                            "OwnerCount": 3,
                            "Sequence": 5037711
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
                        "PreviousFields": {
                            "Balance": "10404768003",
                            "Sequence": 5037710
                        },
                        "PreviousTxnID": "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
                        "PreviousTxnLgrSeq": 56865244
                    }
                },
                {
                    "DeletedNode": {
                        "FinalFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "BookNode": "0000000000000000",
                            "Flags": 0,
                            "OwnerNode": "0000000000000000",
                            "PreviousTxnID": "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
                            "PreviousTxnLgrSeq": 56865244,
                            "Sequence": 5037708,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        },
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Flags": 0,
                            "IndexNext": "0000000000000000",
                            "IndexPrevious": "0000000000000000",
                            "Owner": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "RootIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                        },
                        "LedgerEntryType": "DirectoryNode",
                        "LedgerIndex": "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                    }
                },
                {
                    "CreatedNode": {
                        "LedgerEntryType": "Offer",
                        "LedgerIndex": "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
                        "NewFields": {
                            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                            "BookDirectory": "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                            "Sequence": 5037710,
                            "TakerGets": "15000000000",
                            "TakerPays": {
                                "currency": "CNY",
                                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                                "value": "20160.75"
                            }
                        }
                    }
                }
            ],
            "TransactionIndex": 0,
            "TransactionResult": "tesSUCCESS"
        },
        "status": "success",
        "tx_json": {
            "Account": "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
            "Fee": "12",
            "Flags": 0,
            "LastLedgerSequence": 56865248,
            "OfferSequence": 5037708,
            "Sequence": 5037710,
            "SigningPubKey": "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
            "TakerGets": "15000000000",
            "TakerPays": {
                "currency": "CNY",
                "issuer": "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                "value": "20160.75"
            },
            "TransactionType": "OfferCreate",
            "TxnSignature": "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
            "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
        },
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_hash" : "793E56131D8D4ABFB27FA383BFC44F2978B046E023FF46C588D7E0C874C2472A",
      "ledger_index" : 56865245,
      "metadata" : {
         "AffectedNodes" : [
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "ExchangeRate" : "4F04C66806CF7400",
                     "Flags" : 0,
                     "RootIndex" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "TakerGetsCurrency" : "0000000000000000000000000000000000000000",
                     "TakerGetsIssuer" : "0000000000000000000000000000000000000000",
                     "TakerPaysCurrency" : "000000000000000000000000434E590000000000",
                     "TakerPaysIssuer" : "CED6E99370D5C00EF4EBF72567DA99F5661BFB3A"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "Balance" : "10404767991",
                     "Flags" : 0,
                     "OwnerCount" : 3,
                     "Sequence" : 5037711
                  },
                  "LedgerEntryType" : "AccountRoot",
                  "LedgerIndex" : "1DECD9844E95FFBA273F1B94BA0BF2564DDF69F2804497A6D7837B52050174A2",
                  "PreviousFields" : {
                     "Balance" : "10404768003",
                     "Sequence" : 5037710
                  },
                  "PreviousTxnID" : "4DC47B246B5EB9CCE92ABA8C482479E3BF1F946CABBEF74CA4DE36521D5F9008",
                  "PreviousTxnLgrSeq" : 56865244
               }
            },
            {
               "DeletedNode" : {
                  "FinalFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "BookDirectory" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "BookNode" : "0000000000000000",
                     "Flags" : 0,
                     "OwnerNode" : "0000000000000000",
                     "PreviousTxnID" : "8F5FF57B404827F12BDA7561876A13C3E3B3095CBF75334DBFB5F227391A660C",
                     "PreviousTxnLgrSeq" : 56865244,
                     "Sequence" : 5037708,
                     "TakerGets" : "15000000000",
                     "TakerPays" : {
                        "currency" : "CNY",
                        "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                        "value" : "20160.75"
                     }
                  },
                  "LedgerEntryType" : "Offer",
                  "LedgerIndex" : "26AAE6CA8D29E28A47C92ADF22D5D96A0216F0551E16936856DDC8CB1AAEE93B"
               }
            },
            {
               "ModifiedNode" : {
                  "FinalFields" : {
                     "Flags" : 0,
                     "IndexNext" : "0000000000000000",
                     "IndexPrevious" : "0000000000000000",
                     "Owner" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "RootIndex" : "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
                  },
                  "LedgerEntryType" : "DirectoryNode",
                  "LedgerIndex" : "47FAF5D102D8CE655574F440CDB97AC67C5A11068BB3759E87C2B9745EE94548"
               }
            },
            {
               "CreatedNode" : {
                  "LedgerEntryType" : "Offer",
                  "LedgerIndex" : "8BAEE3C7DE04A568E96007420FA11ABD0BC9AE44D35932BB5640E9C3FB46BC9B",
                  "NewFields" : {
                     "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
                     "BookDirectory" : "02BAAC1E67C1CE0E96F0FA2E8061020536CEDD043FEB0FF54F04C66806CF7400",
                     "Sequence" : 5037710,
                     "TakerGets" : "15000000000",
                     "TakerPays" : {
                        "currency" : "CNY",
                        "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
                        "value" : "20160.75"
                     }
                  }
               }
            }
         ],
         "TransactionIndex" : 0,
         "TransactionResult" : "tesSUCCESS"
      },
      "status" : "success",
      "tx_json" : {
         "Account" : "rhhh49pFH96roGyuC4E5P4CHaNjS1k8gzM",
         "Fee" : "12",
         "Flags" : 0,
         "LastLedgerSequence" : 56865248,
         "OfferSequence" : 5037708,
         "Sequence" : 5037710,
         "SigningPubKey" : "03B51A3EDF70E4098DA7FB053A01C5A6A0A163A30ED1445F14F87C7C3295FCB3BE",
         "TakerGets" : "15000000000",
         "TakerPays" : {
            "currency" : "CNY",
            "issuer" : "rKiCet8SdvWxPXnAgYarFUXMh1zCPz432Y",
            "value" : "20160.75"
         },
         "TransactionType" : "OfferCreate",
         "TxnSignature" : "3045022100A5023A0E64923616FCDB6D664F569644C7C9D1895772F986CD6B981B515B02A00220530C973E9A8395BC6FE2484948D2751F6B030FC7FB8575D1BFB406368AD554D9",
         "hash" : "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9"
      },
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`        | Type                      | Description                     |
|:---------------|:--------------------------|:--------------------------------|
| `ledger_index` | Number - [Ledger Index][] | 找到交易记录的分类帐版本的分类帐索引；这与请求中的相同。 |
| `ledger_hash`  | String - [Hash][]         | _(可以省略)_ 在中找到交易的分类帐版本的标识哈希；这与请求中的相同。 |
| `metadata`     | Object                    | [transaction metadata](transaction-metadata.html), 它详细地显示了交易的确切结果。 |
| `tx_json`      | Object                    | [Transaction object](transaction-formats.html) 的JSON表示|

服务器找不到事务可能有几个原因:

* 事务不存在
* 交易记录存在，但不在指定的分类帐版本中
* 服务器没有可用的指定分类帐版本。另一个手头有正确版本的服务器可能有不同的响应。

## Possible Errors

* 任何[通用错误类型][]。
* `fieldNotFoundTransaction` - 请求中省略了`tx_hash`字段
* `notYetImplemented` - 请求中未指定分类帐版本。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在，或者确实存在，但服务器没有它。
* `transactionNotFound` - 在指定的分类帐中找不到请求中指定的交易记录(它可能在不同的分类帐版本中，或者根本不可用。）



{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
