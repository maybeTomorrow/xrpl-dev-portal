---
html: xrp-ledger-toml.html
parent: references.html
blurb: Provide machine-readable information about yourself to other XRP Ledger users. #TODO:translate
curated_anchors:
    - name: Serving the File
      anchor: "#serving-the-file"
    - name: Contents
      anchor: "#contents"
    - name: CORS Setup
      anchor: "#cors-setup"
    - name: Domain Verification
      anchor: "#domain-verification"
    - name: Account Verification
      anchor: "#account-verification"
---
# xrp-ledger.toml File

如果您运行XRP分类账验证程序或为您的业务使用XRP分类账，您可以在机器可读的**`xrp Ledger.toml`**文件中向世界提供有关您使用XRP分类账的信息。脚本和应用程序可以使用`xrp ledger.toml`文件中包含的信息，以便更好地理解并在XRP ledger中表示您。在某些情况下，人类也会发现读取同一个文件很有用。

`xrp ledger.toml`文件的一个主要用例是[域验证]（#域验证）。

#### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

## Serving the File

`xrp ledger.toml`文件将由web服务器提供服务。该文件应位于以下URL:

```
https://{DOMAIN}/.well-known/xrp-ledger.toml
```

`{域}`是您的域名，包括所有子域。例如，您可以从以下任一URL提供文件:

```
https://example.com/.well-known/xrp-ledger.toml
https://xrp.services.example.com/.well-known/xrp-ledger.toml
```

### Protocol

为了安全起见，必须通过**HTTPS协议**提供内容，使用当前的安全版本[TLS](https://tools.ietf.org/html/rfc8446)，使用由知名证书颁发机构签名的有效证书(注意：TLS以前称为SSL，但这些版本不再安全。）换句话说，托管`xrp-ledger.toml`文件的先决条件是具有正确配置的HTTPS web服务器。

纯HTTP协议易受中间人攻击；例如，已知一些因特网服务修改通过普通HTTP检索的内容以注入它们自己的广告。为防止类似技术误报`xrp-ledger.toml`文件的内容并可能导致脚本行为不正确或具有欺骗性，不应信任通过纯HTTP提供服务的`xrp-ledger.toml`文件的内容。


### Domain

提供`xrp ledger.toml`文件的域是所有权声明。 当文件内容独立存在时，它们就没有那么有用或可信了。 出于实际原因，可能不希望从主域提供文件，因此可以使用任意数量的子域。 设置XRP分类科目]的[`Domain`字段时](accountset.html#domain), 您必须提供完整的域，包括您使用的所有子域。 有关详细信息，请参阅[帐户验证]（#account-verification）。

如果需要，您可以从多个子域提供相同的文件。例如，如果子域`www.example.com` 转到与`example.com`相同的网站，您可以从两个位置提供文件。 如果您的网站需要`www`前缀，请确保在指定域时包含该前缀 （例如，设置XRP分类科目的`域`字段时）。

建议您提供一个与`xrp ledger.toml`文件来自同一域的可读网站。该网站可以提供有关您的身份以及如何使用XRP分类账的更多信息，这有助于建立对您和您的服务的信任。


### Path

符合[RFC5785](https://tools.ietf.org/html/rfc5785)，路径必须以`/.well-known/`开头。文件必须位于路径`/.well-known/xrp-ledger.toml`处（区分大小写，全部小写）。 <!-- SPELLING_IGNORE: rfc5785 -->

如果需要，您可以从不同大小写的路径提供相同的文件，例如`/.well-known/XRP-Ledger.TOML`。 不能根据路径的大写方式提供不同的内容。


### Headers

#### Content-Type

`xrp ledger.toml`文件的建议**内容类型**为**`application/toml`**。但是，使用该文件的应用程序也应接受`text/plain`的内容类型值。

#### CORS

若要允许其他网站上的脚本查询该文件，应为该文件启用[CORS][]。具体来说，服务器在为`xrp ledger.toml`提供服务时应提供以下标头：

```
Access-Control-Allow-Origin: *
```

有关如何配置服务器以提供此标头的信息，请参阅\ors安装程序]（#cors安装程序）。

#### Other Headers

服务器可以根据需要使用其他标准HTTP报头，包括用于压缩、缓存控制、重定向和链接相关资源的报头。

### Generation

`xrp ledger.toml`文件可能是存储在web服务器上的实际文件，也可能是由web服务器按需生成的。后一种情况可能更可取，这取决于文件中提供的内容或网站的配置。



## Contents

`xrp ledger.toml`文件的内容必须用[TOML]格式化(https://github.com/toml-lang/toml). **所有内容都是可选的。**注释是可选的，但鼓励可读性。

示例内容:

```
# Example xrp-ledger.toml file. These contents should not be considered
# authoritative for any real entity or business.
# Note: all fields and all sections are optional.

[METADATA]
modified = 2019-01-22T00:00:00.000Z
expires = 2019-03-01T00:00:00.000Z

[[VALIDATORS]]
public_key = "nHBtDzdRDykxiuv7uSMPTcGexNm879RUUz5GW4h1qgjbtyvWZ1LE"
attestation = "A59AB577E14A7BEC053752ABFE78C3DED6DCEC81A7C41DF1931BC61742BB4FAEAA0D4F1C1EAE5BC74F6D68A3B26C8A223EA2492A5BD18D51F8AC7F4A97DFBE0C"
network = "main"
owner_country = "us"
server_country = "us"
unl = "https://vl.ripple.com"

[[VALIDATORS]]
public_key = "nHB57Sey9QgaB8CubTPvMZLkLAzfJzNMWBCCiDRgazWJujRdnz13"
attestation = "A59AB577E14A7BEC053752FBFE78C3DED6DCEC81A7C41DF1931BC61742BB4FAEAA0D4F1C1EAE5BC74F6D68A3B26C8A223EA249BA5BD18D51F8AC7F4A97DFBE0C"
network = "testnet"
owner_country = "us"
server_country = "us"
unl = "https://vl.testnet.rippletest.net"

# Note: the attestions above are only examples and are not real.

[[ACCOUNTS]]
address = "r3kmLJN5D28dHuH8vZNUZpMC43pEHpaocV"
desc = "Ripple-owned address from old ripple.txt file"
# Note: This doesn't prove ownership of an account unless the
#   "Domain" field of the account in the XRP Ledger matches the
#   domain this file was served from.

[[SERVERS]]
json_rpc = "https://s1.ripple.com:51234/"
ws = "wss://s1.ripple.com/"
peer = "https://s1.ripple.com:51235/"
desc = "General purpose server cluster"

[[SERVERS]]
json_rpc = "https://s2.ripple.com:51234/"
ws = "wss://s2.ripple.com/"
peer = "https://s2.ripple.com:51235/"
desc = "Full-history server cluster"

[[SERVERS]]
json_rpc = "https://s.testnet.rippletest.net:51234/"
ws = "wss://s.testnet.rippletest.net:51233/"
peer = "https://s.testnet.rippletest.net:51235/"
network = "testnet"
desc = "Test Net public server cluster"

[[PRINCIPALS]]
name = "Rome Reginelli" # Primary spec author
email = "rome@example.com" # Not my real email address

[[CURRENCIES]]
code = "LOL"
issuer = "rf1BiGeXwwQoi8Z2ueFYTEXSwuJYfV2Jpn"
network = "testnet"
display_decimals = 2
symbol = "😆" # In practical situations, it may be unwise to use emoji

# End of file
```

### Metadata

元数据部分提供有关`xrp ledger.toml`文件本身的信息。如果存在，则必须将此节显示为一个表，以行`[]`为标题，并使用单\u方括号(`xrp ledger.toml`文件的大多数其他部分使用双括号表示信息数组，但最多有一个`[]`部分。）您可以提供以下任何字段（区分大小写）:

| Field      | Type             | Description                                  |
|:-----------|:-----------------|:---------------------------------------------|
| `modified` | Offset Date-Time | 上次修改`xrp leder.toml`文件的时间。    |
| `expires`  | Offset Date-Time | 如果当前时间等于或大于此时间，`xrp ledger.toml`文件应视为已过期。 |

规范没有定义`域`字段；该字段应根据提供文件的站点确定。

**Tip:** 对于偏移日期时间值，Ripple建议使用偏移量`Z`，并提供高达毫秒的精度(例如，`2019-01-22T22:26:58.027Z`）如果手动编辑文件，则可以通过为小时、分钟、秒和毫秒提供零来估计时间(例如，`2019-01-22T00:00:00.000Z`）

### Validators

validators列表提供有关验证您运行的服务器的信息。如果存在，验证程序列表必须以表数组的形式呈现，每个条目都使用头`[[VALIDATORS]]`，包括双方括号。每个条目描述一个单独的验证服务器。

文件中的第一个\u`[[VALIDATORS]]`条目被视为您的主验证器。如果您为生产XRP账本运行一个或多个验证器，您应该将希望其他人信任的验证器放在第一位。

对于 _每个_ `[[VALIDATORS]]`条目，您可以提供以下任何字段:

| Field        | Type   | Description                                          |
|:-------------|:-------|:-----------------------------------------------------|
| `public_key` | String | 主验证器的主公钥，以XRP分类帐的base58格式编码（通常以`n`开头）。 |
| `attestation`| String | 十六进制的签名消息，指示同一实体运行此验证程序和服务于此TOML文件的域。有关详细信息，请参阅[域验证](xrp-ledger-toml.html#domain-verification).
| `network`  | String | 验证程序遵循哪个网络链。如果省略，客户机应该假设验证器遵循生产XRP分类帐。使用`main`显式指定生产XRP分类帐。使用`testnet`作为Ripple的XRP分类帐测试网。您可以提供其他值来描述其他测试网络或非标准网络链。 |
| `owner_country` | String | 两个字母的ISO-3166-2国家代码，描述您（验证器的所有者）所受的主要法律管辖权。 |
| `server_country` | String | 两个字母的ISO-3166-2国家代码，描述此验证服务器所在的物理位置。 |
| `unl` | String | 一个HTTPS URL，您可以在其中找到此验证程序信任的其他验证程序的列表。如果验证器配置为使用验证器列表站点进行UNL推荐，则这必须与服务器的配置相匹配。对于生产XRP分类帐网络，使用`https://vl.ripple.com` （尾部斜杠可选）。 |


### Accounts

帐户列表提供有关您拥有的XRP分类帐帐户的信息。如果存在，帐户列表必须以表格数组的形式显示，每个条目都使用标题`[[ACCOUNTS]]`，包括双方括号。每个条目描述一个单独的帐户。对于每个u`[[[]]`条目，您可以提供以下任何字段:

| Field     | Type   | Description                                             |
|:----------|:-------|:--------------------------------------------------------|
| `address` | String | 账户的公共地址，以XRP分类账的base58格式编码（通常以`r`开头）。 |
| `network` | String | 主要使用此帐户的网络链。如果省略，客户应该假设该帐户是在生产XRP分类账和其他网络链上申请的。使用`主`作为生产XRP分类帐。使用`testnet`作为Ripple的XRP分类帐测试网。您可以提供其他值来描述其他测试网络或非标准网络链。 |
| `desc`    | String | 对该帐户的用途或使用方法的可读描述。 |

**Caution:** 任何人都可以通过托管一个`xrp ledger.toml`文件来声明对任何帐户的所有权，因此，除非XRP ledger]（accountset.html#domain）中这些帐户的[`Domain`字段也与提供此`xrp ledger.toml`文件的域匹配，否则此处的帐户不应被视为权威帐户。有关详细信息，请参阅[帐户验证]（#帐户验证）。


### Principals

委托人列表提供有关XRP分类帐业务和服务中涉及的人员（或业务实体）的信息。 如果存在，主体列表必须以表数组的形式呈现，每个条目都使用头`[[[]]`，包括双方括号。每个条目描述不同的接触点。对于每个`[[PRINCIPALS]]`条目，您可以提供以下任何字段:

| Field   | Type   | Description                                              |
|:--------|:-------|:---------------------------------------------------------|
| `name`  | String | 这个校长的名字。                              |
| `email` | String | 可以联系此负责人的电子邮件地址。 |

您可以根据需要提供其他联系方式。 (有关自定义字段的信息，请参阅[自定义字段]（#自定义字段）。)


### Servers

“服务器”列表提供有关使用公共访问运行的XRP分类帐服务器（`rippled`）的信息。如果存在，服务器列表必须以表数组的形式显示，每个条目都使用头`[[SERVERS]]`，包括双方括号。每个条目描述不同的服务器或服务器集群。对于每个`[[[]]`条目，您可以提供以下任何字段:

| Field   | Type   | Description                                              |
|:--------|:-------|:---------------------------------------------------------|
| `json_rpc` | String (URL) | 提供公共JSON-RPC API的URL。必须以`http://`或`https://`开头。建议将HTTPS用于公共API。 |
| `ws` | String (URL) | 提供公共WebSocket API的URL。必须以`ws://`或`wss://`开头。建议将WSS用于公共API。 |
| `peer` | String (URL) | 服务器正在侦听XRP Ledger对等协议的URL。其他XRP账本服务器可以通过此URL连接。如果您的服务器提供对等爬网程序响应，则会从此URL向其提供附加了`爬网`的服务。 |
| `network`  | String | 此服务器遵循哪个网络链。如果省略，客户机应该假设服务器遵循生产XRP分类帐。使用`main`显式指定生产XRP分类帐。 使用`testnet`作为Ripple的XRP分类帐测试网。您可以提供其他值来描述其他测试网络或非标准网络链。 |

对于本节中的所有URL，建议使用尾随斜杠。如果省略，客户机应用程序应该假定后面有一个斜杠。


### Currencies

如果您在XRP分类账中发行任何资产、代币或货币，您可以在`[[CURRENCIES]]`列表中提供有关这些资产、代币或货币的信息。如果存在，货币列表必须以表格数组的形式显示，每个条目都使用标题`[[currences]]`，包括双方括号。每个条目描述一个单独的发行货币或资产。对于每个u`[[CURRENCIES]]`条目，您可以提供以下任何字段:

| Field   | Type   | Description                                           |
|:--------|:-------|:------------------------------------------------------|
| `code` | String | XRP分类账中该货币的（区分大小写）股票代码符号。这可以是三位代码、40个字符的十六进制代码或自定义格式（对于知道如何在XRP分类账中表示非标准代码的客户机）。有关XRP分类帐货币代码格式的信息，请参阅[货币代码参考]（currency formats.html#currency codes）。 |
| `display_decimals` | Number | 客户端应用程序应用于显示此货币金额的小数位数。 |
| `issuer` | String | 发行此货币的XRP分类科目的地址，以XRP分类科目的base58格式编码（通常以`r`开头）。您还应该在`[[]]`列表中列出此地址(提醒：这里的地址本身并不具有权威性。有关详细信息，请参阅[帐户验证]（#帐户验证） |
| `network` | String | 发行这种货币的网络链。使用`main`显式指定生产XRP分类帐。如果省略，客户应该假设货币是在生产XRP分类账上发行的。使用`testnet`作为Ripple的XRP分类帐测试网。您可以提供其他值来描述其他测试网络或非标准网络链。 |
| `symbol` | String | 文本符号，如"$"或"€"，如果在Unicode标准中有符号，则应与此资产或货币的金额一起使用。 |


### Custom Fields

`xrp ledger.toml`文件是为XRP ledger的用户准备的，用于向其他用户、脚本和应用程序提供信息。因此，可能有许多种类的信息是有用的，但是在本说明书中没有描述。鼓励用户根据需要在`xrp ledger.toml`文件的任何级别添加其他字段，以传递相关信息。

分析`xrp ledger.toml`文件的工具必须接受包含应用程序不熟悉的任何其他字段的文档。这些工具可以使调用这些字段的高级应用程序可以使用这些附加字段，也可以丢弃这些字段。为了保持与本规范未来版本的向前兼容性，工具还可以丢弃本标准中指定的字段。如果`xrp ledger.toml`文件包含无法识别的字段，则工具不能返回错误。为了检测输入错误，工具可能会对无法识别的字段提供警告，尤其是那些字段名与标准字段名类似的情况。

如果工具识别的字段未按预期格式化，则工具可能会返回错误，即使该字段未在本规范中定义。

创建自定义字段时，请注意所选的字段名。如果您使用非常通用的字段名，其他用户可能会使用相同的名称来表示不同的内容，或者以冲突的方式格式化。如果您使用您认为其他人会发现有用的自定义字段，请为您的字段向本文档的维护人员提供一个规范。


## CORS Setup

必须将web服务器配置为允许`xrp ledger.toml`文件的跨源资源共享（[CORS][]）。此配置取决于您的web服务器。


如果运行apache http服务器，请将以下内容添加到配置文件中:

```
<Location "/.well-known/xrp-ledger.toml">
    Header set Access-Control-Allow-Origin "*"
</Location>
```

Alternatively, you can add the following to a `.htaccess` file in the `/.well-known/` directory of your server:

```
<Files "xrp-ledger.toml">
    Header set Access-Control-Allow-Origin "*"
</Files>
```


If you use nginx, add the following to your config file:

```
location /.well-known/xrp-ledger.toml {
    add_header 'Access-Control-Allow-Origin' '*';
}
```

对于其他web服务器，请参阅[我想将CORS支持添加到我的服务器](https://enable-cors.org/server.html). 如果您使用托管，请参阅您的web主机的文档，了解如何在特定路径上启用CORS(您可能不想为整个网站启用CORS。）

[CORS]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS


## Domain Verification

`xrp ledger.toml`文件的一个用途是验证运行特定域的同一实体是否也运行由验证程序公钥标识的特定验证程序。验证域和验证程序是否由同一实体拥有可以更好地保证验证程序操作员的身份，这是成为受信任验证程序的建议步骤。(有关其他建议，请参阅 [Properties of a Good Validator](run-rippled-as-a-validator.html#1-understand-the-traits-of-a-good-validator).)

域验证需要在域操作员和验证程序之间建立双向链接:

1. The domain claims ownership of the validator:

    - Serve an `xrp-ledger.toml` file, following all the [requirements described in this document](#serving-the-file), from the domain in question.

    - In that `xrp-ledger.toml` file, provide a `[[VALIDATORS]]` entry with the validator's master public key in the `public_key` field.

2. The validator claims ownership of the domain:

    - Ensure that you have access to the validator-keys.json file that you created when first setting up your validator. If you have lost your keys or the keys have been compromised, please [revoke your keys](run-rippled-as-a-validator.html#revoke-validator-keys) and generate new keys.

        Note: Recall that your validator-keys.json file should be stored **in a location not on your validator**.

    - **In a location not on your validator**, build the [validator-keys-tool](https://github.com/ripple/validator-keys-tool).  

    - Run the following command to generate a new validator token that incorporates your domain and update your `xrp-ledger.toml` and `rippled.cfg` files:


            $./validator-keys set_domain example.com

**Warning:** This command updates your validator-keys.json file. Please be sure to store the `validator-keys.json` file in a secure location.

Sample Output:

```
The domain name has been set to: example.com

The domain attestation for validator nHDG5CRUHp17ShsEdRweMc7WsA4csiL7qEjdZbRVTr74wa5QyqoF is:

attestation="A59AB577E14A7BEC053752FBFE78C3DE
             D6DCEC81A7C41DF1931BC61742BB4FAE
             AA0D4F1C1EAE5BC74F6D68A3B26C8A22
             3EA2492A5BD18D51F8AC7F4A97DFBE0C"

You should include it in your xrp-ledger.toml file in the
section for this validator.

You also need to update the rippled.cfg file to add a new
validator token and restart rippled:

# validator public key: nHDG5CRUHp17ShsEdRweMc7WsA4csiL7qEjdZbRVTr74wa5QyqoF

[validator_token]
eyJ2YWxpZGF0aW9uX3NlY3J|dF9rZXkiOiI5ZWQ0NWY4NjYyNDFjYzE4YTI3NDdiNT
QzODdjMDYyNTkwNzk3MmY0ZTcxOTAyMzFmYWE5Mzc0NTdmYT|kYWY2IiwibWFuaWZl
c3QiOiJKQUFBQUFGeEllMUZ0d21pbXZHdEgyaUNjTUpxQzlnVkZLaWxHZncxL3ZDeE
hYWExwbGMyR25NaEFrRTFhZ3FYeEJ3RHdEYklENk9NU1l1TTBGREFscEFnTms4U0tG
bjdNTzJmZGtjd1JRSWhBT25ndTlzQUtxWFlvdUorbDJWMFcrc0FPa1ZCK1pSUzZQU2
hsSkFmVXNYZkFpQnNWSkdlc2FhZE9KYy9hQVpva1MxdnltR21WcmxIUEtXWDNZeXd1
NmluOEhBU1FLUHVnQkQ2N2tNYVJGR3ZtcEFUSGxHS0pkdkRGbFdQWXk1QXFEZWRGdj
VUSmEydzBpMjFlcTNNWXl3TFZKWm5GT3I3QzBrdzJBaVR6U0NqSXpkaXRROD0ifQ==     
```

Update [the contents of your `xrp-ledger.toml` file](#contents) with the `attestation` block, and update the `rippled.cfg` file with the `[validator_token]` block from the sample output.




**Warning:** Your validator token is meant to be kept secret. Do not share it on your `xrp-ledger.toml` file or anywhere else.

## Account Verification

Similar to [Domain Verification](#domain-verification), account verification is the idea of proving that the same entity controls a particular domain and a particular XRP Ledger address. Account verification is not necessary for using the XRP Ledger or providing an `xrp-ledger.toml` file, but you may desire to verify your accounts in the name of transparency.

Account verification requires establishing a two-way link between the domain operator and the address:

1. The domain claims ownership of the address.

    - Serve an `xrp-ledger.toml` file, following all the [requirements described in this document](#serving-the-file), from the domain in question.

    - In that `xrp-ledger.toml` file, provide an `[[ACCOUNTS]]` entry with the address of the account you want to verify. If you issue currency from this address, you may also provide this account in the `issuer` field of a `[[CURRENCIES]]` entry.

2. The address claims ownership by a domain.

    [Set the account's `Domain` field](accountset.html#domain) to match the domain that this `xrp-ledger.toml` file was served from. The domain value (when decoded from ASCII) MUST match _exactly_, including all subdomains such as `www.`. For internationalized domain names, set the `Domain` value to the Punycode of the domain, as described in [RFC3492](https://tools.ietf.org/html/rfc3492). <!-- SPELLING_IGNORE: punycode, rfc3492 -->

    Since setting the `Domain` requires sending a transaction, whoever set the `Domain` value must have possessed the account's secret key when the transaction was sent.

Either of these two links, on their own, SHOULD NOT be considered authoritative. Anyone could host an `xrp-ledger.toml` file claiming ownership of any account, and any account operator could set its `Domain` field to any string it wants. If the two match, it provides strong evidence that the same entity controls both.


## Acknowledgements

This specification is derived from the [original ripple.txt spec](https://web.archive.org/web/20161007113240/https://wiki.ripple.com/Ripple.txt) and draws inspiration from the [stellar.toml file](https://www.stellar.org/developers/guides/walkthroughs/how-to-complete-stellar-toml.html). This specification also incorporates feedback from XRP community members and many past and current Ripple employees. <!-- SPELLING_IGNORE: txt -->
