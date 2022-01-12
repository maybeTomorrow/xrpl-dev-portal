---
html: tx.html
parent: transaction-methods.html
blurb: Retrieve info about a transaction from all the ledgers on hand.
---
# tx
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/Tx.cpp "Source")

`tx`方法检索单个[事务](transaction-formats.html)的信息通过[identifying hash][]

## Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "command": "tx",
  "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
  "binary": false
}
```
*JSON-RPC*

```json
{
    "method": "tx",
    "params": [
        {
            "transaction": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
            "binary": false
        }
    ]
}
```
*Commandline*

```sh
#Syntax: tx transaction [binary]
rippled tx C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9 false
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#tx)

请求包括以下参数:

| `Field`       | Type    | Description                                        |
|:--------------|:--------|:---------------------------------------------------|
| `transaction` | String  | 事务的256位哈希，十六进制。      |
| `binary`      | Boolean | _(可选)_ 如果`true`，则将事务数据和元数据作为二进制[序列化]（serialization.html）返回到十六进制字符串。如果`false`，则将事务数据和元数据作为JSON返回。默认值为`false`。 |
| `min_ledger`  | Number  | _(可选)_ 与`max_ledger`一起使用，以指定从该分类帐（含）开始的最多1000[分类帐索引][分类帐索引]的范围。如果服务器[找不到事务]（#找不到响应），它将确认是否能够搜索此范围内的所有分类帐。 |
| `max_ledger`  | Number  | _(可选)_ 与`最小分类帐`一起使用，可指定一个范围，最多为1000[分类帐索引][分类帐索引]，以该分类帐（含）结尾。 如果服务器[找不到事务](#not-found-response)，它将确认是否能够搜索请求范围内的所有分类帐。|

**Caution:** 此命令可以成功地找到交易记录，即使它包含在`最小分类帐`到`最大分类帐`范围之外的分类帐中。

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 1,
  "result": {
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
    "date": 648248020,
    "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
    "inLedger": 56865245,
    "ledger_index": 56865245,
    "meta": {
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
        "date": 648248020,
        "hash": "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
        "inLedger": 56865245,
        "ledger_index": 56865245,
        "meta": {
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
        "validated": true
    }
}
```

*Commandline*

```json
{
   "result" : {
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
      "date" : 648248020,
      "hash" : "C53ECF838647FA5A4C780377025FEC7999AB4182590510CA461444B207AB74A9",
      "inLedger" : 56865245,
      "ledger_index" : 56865245,
      "meta" : {
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
      "validated" : true
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含[transation对象]（transaction formats.html）的字段以及以下附加字段:

| `Field`        | Type                             | Description              |
|:---------------|:---------------------------------|:-------------------------|
| `hash`         | String                           | 事务的SHA-512哈希 |
| `inLedger`     | Number                           | _(已弃用)_  `ledger_index`的别名. |
| `ledger_index` | Number                           | 包含此交易记录的分类帐的[分类帐索引][]。 |
| `meta`         | Object (JSON) or String (binary) | [Transaction metadata](transaction-metadata.html), 它描述了交易的结果。 |
| `validated`    | Boolean                          | 如果`为真`，则此数据来自已验证的分类帐版本；如果省略或设置为`false`，则此数据不是最终数据。 |
| (Various)      | (Various)                        | [事务对象](transaction-formats.html)中的其他字段 |


### Not Found Response

如果服务器找不到事务，它将返回一个`txnotfound`错误，这可能意味着两件事:

- 该交易记录尚未包含在任何分类帐版本中，并且尚未执行。
- 该事务包含在服务器没有可用的分类帐版本中。

这意味着一个`txn单独找到的`不足以知道事务的[最终结果](finality-of-results.html).

为了进一步缩小可能性，您可以使用请求中的`min_ledger`和`max_ledger`字段提供一系列分类帐进行搜索。如果您提供**这两个**，则`txnNotFound`响应包含以下字段:

| `Field`        | Type      | Description                              |
|:---------------|:----------|:-----------------------------------------|
| `searched_all` | Boolean   | _(除非请求提供了`min_ledger`和`max_ledger`，否则省略)_ 如果`为真`，则服务器可以搜索所有指定的分类帐版本，而事务不在其中。如果`false`，则服务器没有所有指定的分类帐版本可用，因此不确定其中一个是否包含该事务。 |

`txnNotFound`响应的一个示例，该响应完全搜索了请求的分类帐范围:

<!-- MULTICODE_BLOCK_START -->

_WebSocket_

```json
{
  "error": "txnNotFound",
  "error_code": 29,
  "error_message": "Transaction not found.",
  "id": 1,
  "request": {
    "binary": false,
    "command": "tx",
    "id": 1,
    "max_ledger": 54368673,
    "min_ledger": 54368573,
    "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
  },
  "searched_all": true,
  "status": "error",
  "type": "response"
}
```

_JSON-RPC_

```json
200 OK

{
  "result": {
    "error": "txnNotFound",
    "error_code": 29,
    "error_message": "Transaction not found.",
    "request": {
      "binary": false,
      "command": "tx",
      "max_ledger": 54368673,
      "min_ledger": 54368573,
      "transaction": "E08D6E9754025BA2534A78707605E0601F03ACE063687A0CA1BDDACFCD1698C7"
    },
    "searched_all": true,
    "status": "error"
  }
}
```

<!-- MULTICODE_BLOCK_END -->

## Possible Errors

* 任何[通用错误类型][]。
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `txnNotFound` - 交易记录不存在，或者它是`rippled`不可用的分类帐版本的一部分。
* `excessiveLgrRange` - 请求的`min_ledger`和`max_ledger`字段相距超过1000。
* `invalidLgrRange` - 指定的`min_ledger`大于`max_ledger`，或者其中一个参数不是有效的分类帐索引。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
