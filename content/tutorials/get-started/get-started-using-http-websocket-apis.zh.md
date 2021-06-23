---
html: get-started-using-http-websocket-apis.html
parent: get-started.html
blurb: Unleash the full power of the XRP Ledger's native APIs.
cta_text: Get Started
---
# Get Started Using HTTP / WebSocket APIs

If you don't have or don't want to use a [client library](client-libraries.html) in your preferred programming language, you can access the XRP Ledger directly through the APIs of its core server software, [`rippled`](the-rippled-server.html). The server provides APIs over JSON-RPC and WebSocket protocols. If you don't [run your own instance of `rippled`](install-rippled.html) you can still use a [public server][public servers].

**Tip:** You can dive right into the API with the [**WebSocket API Tool**](websocket-api-tool.html), or use the [XRP Ledger Explorer](https://livenet.xrpl.org/) to watch the progress of the ledger live.

## Differences Between JSON-RPC and WebSocket

JSON-RPC和WebSocket都是基于HTTP的协议，在大多数情况下，通过这两个协议提供的数据是相同的。主要区别如下:

- JSON-RPC为每个调用使用单独的HTTP请求和响应，类似于RESTfulAPI。您可以使用任何常见的HTTP客户端，如[curl](https://curl.haxx.se/)，[postman](https://www.postman.com/downloads/)
- WebSocket使用持久连接，允许服务器将数据推送到客户端。需要推送消息的函数，如[事件订阅]（subscribe.html），只能使用WebSocket

两个API都可以未加密（`http://`和`ws://`）或使用TLS加密（`https://`和`wss://`）。未加密的连接不应通过开放网络提供服务，但可以在客户端与服务器位于同一台计算机上时使用。


## Admin Access

The API methods are divided into [Public Methods](public-rippled-methods.html) and [Admin Methods](admin-rippled-methods.html) so that organizations can offer public servers for the benefit of the community. To access admin methods, or admin functionality of public methods, you must connect to the API on a **port and IP address marked as admin** in the server's config file.

[example config file](https://github.com/ripple/rippled/blob/f00f263852c472938bf8e993e26c7f96f435935c/cfg/rippled-example.cfg#L1154-L1179) 侦听本地环回网络（127.0.0.1）上的连接，端口5005上使用JSON-RPC（HTTP），端口6006上使用WebSocket（WS），并将所有连接的客户端视为管理员。


## WebSocket API

如果你想试试XRP分类账上的一些方法, 您可以跳过编写自己的WebSocket代码，直接使用API[WebSocket API Tool](websocket-api-tool.html). 稍后，当您要连接到自己的`Ripple`服务器时, you can [build your own client](monitor-incoming-payments-with-websocket.html) 或者使用支持WebSocket的[客户端库](client-libraries.html).

Example WebSocket API request:

```json
{
  "id": "my_first_request",
  "command": "server_info",
  "api_version": 1
}
```

响应将显示服务器的当前状态.

Read more: [Request Formatting >](request-formatting.html) [Response Formatting >](response-formatting.html) [About the server_info method >][server_info method]

## JSON-RPC

You can use any HTTP client (like [RESTED for Firefox](https://addons.mozilla.org/en-US/firefox/addon/rested/), [Postman for Chrome](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en) or [Online HTTP client ExtendsClass](https://extendsclass.com/rest-client-online.html)) to make JSON-RPC calls a `rippled` server. Most programming languages have a library for making HTTP requests built in. <!-- SPELLING_IGNORE: extendsclass -->

Example JSON-RPC request:

```json
POST http://s1.ripple.com:51234/
Content-Type: application/json

{
    "method": "server_info",
    "params": [
        {
            "api_version": 1
        }
    ]
}
```

响应显示服务器的当前状态。

Read more: [Request Formatting >](request-formatting.html#json-rpc-format) [Response Formatting >](response-formatting.html) [About the server_info method >][server_info method]

## Commandline

The commandline interface connects to the same service as the JSON-RPC one, so the public servers and server configuration are the same. By default, the commandline connects to a `rippled` server running on the same machine.

Example commandline request:

```
rippled --conf=/etc/opt/ripple/rippled.cfg server_info
```

Read more: [Commandline Usage Reference >](commandline-usage.html)

**Caution:** 命令行接口仅用于管理目的，不是受支持的API。`rippled`的新版本可能会在没有警告的情况下对命令行API引入突破性的更改！

## Available Methods

For a full list of API methods, see:

- [Public `rippled` Methods](public-rippled-methods.html): Methods available on public servers, including looking up data from the ledger and submitting transactions.
- [Admin `rippled` Methods](admin-rippled-methods.html): Methods for [managing](manage-the-rippled-server.html) the `rippled` server.


## See Also

- **Concepts:**
    - [XRP Ledger Overview](xrp-ledger-overview.html)
    - [Software Ecosystem](software-ecosystem.html)
    - [Parallel Networks](parallel-networks.html)
- **Tutorials:**
    - [Get Started with RippleAPI for JavaScript](get-started-with-rippleapi-for-javascript.html)
    - [Reliable Transaction Submission](reliable-transaction-submission.html)
    - [Manage the rippled Server](manage-the-rippled-server.html)
- **References:**
    - [rippled API Reference](rippled-api.html)
    - [Ripple Data API v2](data-api.html)

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}			
{% include '_snippets/tx-type-links.md' %}			
{% include '_snippets/rippled_versions.md' %}
