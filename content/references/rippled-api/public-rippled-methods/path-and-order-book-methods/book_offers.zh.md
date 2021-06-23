---
html: book_offers.html
parent: path-and-order-book-methods.html
blurb: Get info about offers to exchange two currencies.
---
# book_offers
[[Source]](https://github.com/ripple/rippled/blob/master/src/ripple/rpc/handlers/BookOffers.cpp "Source")

`book_offers`方法检索报价列表，也称为[order book](http://www.investopedia.com/terms/o/order-book.asp)，两种货币之间

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 4,
  "command": "book_offers",
  "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
  "taker_gets": {
    "currency": "XRP"
  },
  "taker_pays": {
    "currency": "USD",
    "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B"
  },
  "limit": 10
}
```

*JSON-RPC*

```json
{
    "method": "book_offers",
    "params": [
        {
            "taker": "r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59",
            "taker_gets": {
                "currency": "XRP"
            },
            "taker_pays": {
                "currency": "USD",
                "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
            },
            "limit": 10
        }
    ]
}
```

*Commandline*

```sh
#Syntax: book_offers taker_pays taker_gets [taker [ledger [limit] ] ]
rippled book_offers 'USD/rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B' 'EUR/rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B'
```

<!-- MULTICODE_BLOCK_END -->

[Try it! >](websocket-api-tool.html#book_offers)

请求包括以下参数:

| `Field`        | Type                                       | Description                    |
|:---------------|:-------------------------------------------|:-------------------------------|
| `ledger_hash`  | String                                     | _(可选)_ 用于分类帐版本的20字节十六进制字符串(参见[分类帐][]） |
| `ledger_index` | String or Unsigned Integer                 | _(可选)_ 要使用的分类帐的[分类帐索引][]，或用于自动选择分类帐的快捷方式字符串(参见[分类帐][]） |
| `limit`        | Unsigned Integer                           | _(可选)_ 如果提供了，服务器在结果中提供的服务不会超过这么多。返回的结果总数可能小于限制，因为服务器忽略了无资金支持的提供。 |
| `taker`        | String                                     | _(可选)_ 要用作透视图的帐户的[地址][]。 此帐户提供的[无资金优惠](offers.html#lifecycle-of-an-offer)始终包含在响应中。（您可以使用此功能查找自己的订单以取消订单。） |
| `taker_gets`   | Object                                     | 指定接受报价的帐户将接收的货币，作为具有`currency`和`issuer`字段（XRP省略issuer）的对象，例如[currency amounts][currency Amount]。 |
| `taker_pays`   | Object                                     | 指定接受报价的帐户将支付的货币，作为具有`currency`和`issuer`字段（XRP省略issuer）的对象，例如[currency amounts][currency Amount]。 |

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
  "id": 11,
  "status": "success",
  "type": "response",
  "result": {
    "ledger_current_index": 7035305,
    "offers": [
      {
        "Account": "rM3X3QSr8icjTGpaF52dozhbT2BZSXJQYM",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D55055E4C405218EB",
        "BookNode": "0000000000000000",
        "Flags": 0,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000AE0",
        "PreviousTxnID": "6956221794397C25A53647182E5C78A439766D600724074C99D78982E37599F1",
        "PreviousTxnLgrSeq": 7022646,
        "Sequence": 264542,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "17.90363633316433"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "27.05340557506234"
        },
        "index": "96A9104BF3137131FF8310B9174F3B37170E2144C813CA2A1695DF2C5677E811",
        "quality": "1.511056473200875"
      },
      {
        "Account": "rhsxKNyN99q6vyYCTHNTC1TqWCeHr7PNgp",
        "BookDirectory": "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D5505DCAA8FE12000",
        "BookNode": "0000000000000000",
        "Flags": 131072,
        "LedgerEntryType": "Offer",
        "OwnerNode": "0000000000000001",
        "PreviousTxnID": "8AD748CD489F7FF34FCD4FB73F77F1901E27A6EFA52CCBB0CCDAAB934E5E754D",
        "PreviousTxnLgrSeq": 7007546,
        "Sequence": 265,
        "TakerGets": {
          "currency": "EUR",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "2.542743233917848"
        },
        "TakerPays": {
          "currency": "USD",
          "issuer": "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
          "value": "4.19552633596446"
        },
        "index": "7001797678E886E22D6DE11AF90DF1E08F4ADC21D763FAFB36AF66894D695235",
        "quality": "1.65"
      }
    ]
  }
}
```

*JSON-RPC*

```json
200 OK

{
    "result": {
        "ledger_current_index": 8696243,
        "offers": [],
        "status": "success",
        "validated": false
    }
}
```

*Commandline*

```json
{
   "result" : {
      "ledger_current_index" : 56867201,
      "offers" : [
         {
            "Account" : "rnixnrMHHvR7ejMpJMRCWkaNrq3qREwMDu",
            "BookDirectory" : "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D56038D7EA4C68000",
            "BookNode" : "0000000000000000",
            "Flags" : 131072,
            "LedgerEntryType" : "Offer",
            "OwnerNode" : "0000000000000000",
            "PreviousTxnID" : "E43ADD1BD4AC2049E0D9DE6BC279B7FD95A99C8DE2C4694A4A7623F6D9AAAE29",
            "PreviousTxnLgrSeq" : 47926685,
            "Sequence" : 219,
            "TakerGets" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "2.459108753792364"
            },
            "TakerPays" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "24.59108753792364"
            },
            "index" : "3087B4828C6B5D8595EA325D69C0F396C57452893647799493A38F2C164990AB",
            "owner_funds" : "2.872409153061363",
            "quality" : "10"
         },
         {
            "Account" : "rKwjWCKBaASEvtHCxtvReNd2i9n8DxSihk",
            "BookDirectory" : "7E5F614417C2D0A7CEFEB73C4AA773ED5B078DE2B5771F6D56038D7EA4C68000",
            "BookNode" : "0000000000000000",
            "Flags" : 131072,
            "LedgerEntryType" : "Offer",
            "OwnerNode" : "0000000000000000",
            "PreviousTxnID" : "B63B2ECD124FE6B02BC2998929517266BD221A02FEE51DDE4992C1BCB7E86CD3",
            "PreviousTxnLgrSeq" : 43166305,
            "Sequence" : 19,
            "TakerGets" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "3.52"
            },
            "TakerPays" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "35.2"
            },
            "index" : "89865F2C70D1140796D9D249AC2ED765AE2D007A52DEC6D6D64CCB1A77A6EB7F",
            "owner_funds" : "3.523192614770459",
            "quality" : "10",
            "taker_gets_funded" : {
               "currency" : "EUR",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "3.516160294182094"
            },
            "taker_pays_funded" : {
               "currency" : "USD",
               "issuer" : "rvYAfWj5gh67oV6fW32ZzP3Aw4Eubs59B",
               "value" : "35.16160294182094"
            }
         }
      ],
      "status" : "success",
      "validated" : false
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`                | Type                      | Description             |
|:-----------------------|:--------------------------|:------------------------|
| `ledger_current_index` | Number - [Ledger Index][] | _(如果提供了`ledger_current_index`，则省略)_ 用于检索此信息的当前进行中分类帐版本的[分类帐索引][]。 |
| `ledger_index`         | Number - [Ledger Index][] | _(如果提供了`ledger_current_index`，则省略)_ 按请求检索此数据时使用的分类帐版本的分类帐索引。 |
| `ledger_hash`          | String - [Hash][]         | _(可以省略)_ 按请求检索此数据时使用的分类帐版本的标识哈希。 |
| `offers`               | Array                     | 提供对象数组，每个对象都有[Offer object](offer.html)的字段 |

除了标准的Offer字段外，以下字段可能包含在`offers`数组的成员中:

| `Field`             | Type                             | Description         |
|:--------------------|:---------------------------------|:--------------------|
| `owner_funds`       | String                           | 出价方可交易的`目标`货币的金额(XRP表示为液滴；任何其他货币都表示为十进制值。）如果一个交易者在同一账簿中有多个报价，则只有排名最高的报价包含此字段。 |
| `taker_gets_funded` | String (XRP) or Object (non-XRP) | （仅包含在部分资助的报价中）考虑到报价的资金状况，买方可获得的最大货币金额。 |
| `taker_pays_funded` | String (XRP) or Object (non-XRP) | （仅包括在部分资助的报价中）考虑到报价的资金状况，买方将支付的最高货币金额。 |
| `quality`           | String                           | 汇率，即`taker_支付`的比率除以`taker_获得`。为公平起见，具有相同质量的报价将自动采用先进先出的方式(换言之，如果多人提出以相同的汇率兑换货币，则首先接受最早的报价。） |

## Possible Errors

* 任何[通用错误类型][].
* `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。
* `lgrNotFound` - `ledger_hash`或`ledger_index`指定的分类帐不存在,或者它确实存在，但服务器没有它。
* `srcCurMalformed` - 请求中的 `taker_pays` 字段格式不正确。
* `dstAmtMalformed` - 请求中的 `taker_gets` 字段格式不正确。
* `srcIsrMalformed` - 请求中 `issuer` 字段中的 `taker_pays` 字段无效.
* `dstIsrMalformed` - 请求中 `issuer` 字段中的 `taker_gets` 字段无效.
* `badMarket` - 所需的订单簿不存在；例如，为自己兑换一种货币。


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
