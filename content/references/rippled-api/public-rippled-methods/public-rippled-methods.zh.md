---
html: public-rippled-methods.html
parent: rippled-api.html
blurb: Get data from the XRP Ledger and submit transactions using these public API methods.
---
# Public rippled Methods

使用以下公共API方法直接与`Ripple`服务器通信。公共方法不一定是针对一般公共的，但是连接到服务器的任何客户机都可以使用它们。将公共方法看作是运行服务器的组织的成员或客户的方法。


## [Account Methods](account-methods.html)

XRP分类账中的账户代表XRP持有人和交易发送者。使用这些方法处理帐户信息。

* **[`account_channels`](account_channels.html)** - 获取支付渠道列表，其中帐户是渠道的来源。
* **[`account_currencies`](account_currencies.html)** - 获取帐户可以发送或接收的货币列表。
* **[`account_info`](account_info.html)** - 获取有关帐户的基本数据。
* **[`account_lines`](account_lines.html)** - 获取有关帐户信任线的信息。
* **[`account_objects`](account_objects.html)** - 获取帐户拥有的所有分类帐对象。
* **[`account_offers`](account_offers.html)** - 获取有关帐户货币兑换优惠的信息。
* **[`account_tx`](account_tx.html)** - 获取有关帐户交易的信息。
* **[`gateway_balances`](gateway_balances.html)** - 计算一个帐户发行的总金额。
* **[`noripple_check`](noripple_check.html)** - 获取对帐户默认Ripple和No Ripple设置的建议更改。


## [Ledger Methods](ledger-methods.html)

分类帐版本包含标题、事务树和状态树，其中包含帐户设置、信任行、余额、事务和其他数据。使用这些方法检索分类帐信息。

* **[`ledger`](ledger.html)** - 获取有关分类帐版本的信息。
* **[`ledger_closed`](ledger_closed.html)** - 获取最新关闭的分类帐版本。
* **[`ledger_current`](ledger_current.html)** - 获取当前工作分类帐版本。
* **[`ledger_data`](ledger_data.html)** - 获取分类帐版本的原始内容。
* **[`ledger_entry`](ledger_entry.html)** - 从分类帐版本中获取一个元素。


## [Transaction Methods](transaction-methods.html)

事务是唯一可以修改XRP分类帐的共享状态的东西。XRP分类账上的所有业务都采用交易的形式。使用这些方法处理事务。


* **[`sign`](sign.html)** - 对交易进行加密签名。
* **[`sign_for`](sign_for.html)** - 参与多重签名。
* **[`submit`](submit.html)** - 向网络发送事务。
* **[`submit_multisigned`](submit_multisigned.html)** - 向网络发送多签名事务。
* **[`transaction_entry`](transaction_entry.html)** - 从特定分类帐版本检索有关交易的信息。
* **[`tx`](tx.html)** - 从手头的所有账本中检索有关交易的信息。
* **[`tx_history`](tx_history.html)** - 检索有关所有最近事务的信息。


## [Path and Order Book Methods](path-and-order-book-methods.html)

路径定义了付款在从发送方到接收方的过程中通过中间步骤的方式。路径通过订单簿连接发送方和接收方，实现跨货币支付。使用这些方法处理路径和其他书籍。

* **[`book_offers`](book_offers.html)** - 获取有关交换两种货币的报价的信息。
* **[`deposit_authorized`](deposit_authorized.html)** - 查看一个帐户是否有权直接向另一个帐户付款。
* **[`path_find`](path_find.html)** - 在两个帐户之间查找付款路径并接收更新。
* **[`ripple_path_find`](ripple_path_find.html)** - 找到两个账户之间的支付路径，一次。


## [Payment Channel Methods](payment-channel-methods.html)

支付渠道是一种促进双方重复、单向支付或临时信贷的工具。使用这些方法处理支付渠道。

* **[`channel_authorize`](channel_authorize.html)** - Sign a claim for money from a payment channel.
* **[`channel_verify`](channel_verify.html)** - Check a payment channel claim's signature.


## [Subscription Methods](subscription-methods.html)

使用这些方法使服务器能够在各种事件发生时将更新推送到您的客户端，这样您就可以立即知道并做出反应_仅WebSocket API_

* **[`subscribe`](subscribe.html)** - Listen for updates about a particular subject.
* **[`unsubscribe`](unsubscribe.html)** - Stop listening for updates about a particular subject.


## [Server Info Methods](server-info-methods.html)

使用这些方法可检索有关`Ripple`服务器当前状态的信息。

* **[`fee`](fee.html)** - Get information about transaction cost.
* **[`server_info`](server_info.html)** - Retrieve status of the server in human-readable format.
* **[`server_state`](server_state.html)** - Retrieve status of the server in machine-readable format.
- **[`manifest`](manifest.html)** - Retrieve the latest ephemeral public key information about a known validator.

## [Utility Methods](utility-methods.html)

使用这些方法执行方便的任务，例如ping和随机数生成。

* **[`json`](json.html)** - 用作运行其他命令的代理。接受命令的参数作为JSON值_仅限命令行_
* **[`ping`](ping.html)** - 确认与服务器的连接。
* **[`random`](random.html)** - 生成一个随机数。


## Deprecated Methods

`owner_info`命令已弃用。改用[`account_objects`](account_objects.html)。
