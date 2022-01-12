---
html: commandline-usage.html
parent: rippled-api.html
blurb: Commandline usage options for the rippled server.
curated_anchors:
    - name: Available Modes
      anchor: "#available-modes"
    - name: Daemon Mode Options
      anchor: "#daemon-mode-options"
    - name: Stand-Alone Mode Options
      anchor: "#stand-alone-mode-options"
    - name: Client Mode Options
      anchor: "#client-mode-options"
    - name: Unit Tests
      anchor: "#unit-tests"
---
# rippled Commandline Usage Reference

`rippled`可执行文件通常作为为XRP账本提供电源的守护程序运行，尽管它也可以在其他模式下运行。此页介绍从命令行运行`rippled`时可以传递的所有选项。

## Available Modes

- **Daemon Mode** - 默认值。连接到XRP分类账以处理交易并建立分类账数据库。
- **Stand-Alone Mode** - 使用 `-a` 或 `--standalone` 选项。 与守护程序模式类似，只是它不连接到其他服务器。您可以使用此模式来测试事务处理或其他功能。
- **Client Mode** - 指定API方法名称以作为JSON-RPC客户端连接到另一个`Ripple`服务器，然后退出。如果可执行文件已在另一个进程中运行，则可以使用此选项查找服务器状态和分类帐数据。
- **Other Usage** - 以下每个命令都会导致`Ripple`可执行文件打印一些信息，然后退出:
    - **Help** - Use `-h` or `--help` to print a usage statement.
    - **Unit Tests** - 使用`-u`或`--unittest`运行单元测试并打印结果摘要。这有助于确认您已成功编译`rippled`。
    - **Version statement** - 使用`--版本`让`打印其版本号，然后退出。

## Generic Options

这些选项适用于大多数模式:

| Option          | Description                                                |
|:----------------|:-----------------------------------------------------------|
| `--conf {FILE}` | 使用`{FILE}`作为配置文件，而不是在默认位置查找配置文件。如果未指定，`rippled`首先检查本地工作目录中的`rippled.cfg`文件。在Linux上，如果找不到该文件，`rippled`接下来检查`$XDG\u CONFIG\u HOME/ripple/ripple.cfg`。（通常，`$XDG\u CONFIG\u HOME`映射到`$HOME/.CONFIG`。） |

### Verbosity Options

以下常规选项会影响写入标准输出和日志文件的信息量：

| Option      | Short Version | Description                                    |
|:------------|:--------------|:-----------------------------------------------|
| `--debug`   |               | **DEPRECATED** 启用跟踪级调试（别名为`--详细`）。改用[log\u level方法][]。 |
| `--silent`  |               | 启动时不要将日志写入标准输出和标准错误。建议在启动`时将`作为systemd单元，以减少冗余日志记录。 |
| `--verbose` | `-v`          | **DEPRECATED** 启用跟踪级调试。改用[log\u level方法][]。 |



## Daemon Mode Options

```bash
rippled [OPTIONS]
```

守护程序模式是`rippled`的默认操作模式。除了[Generic Options]（#generic Options）之外，您还可以提供以下任何选项:

| Option              | Description                                            |
|:--------------------|:-------------------------------------------------------|
| `--fg`              | Run the daemon as a single process in the foreground. Otherwise, `rippled` forks a second process for the daemon while the first process runs as a monitor. |
| `--import`          | Before fully starting, import ledger data from another `rippled` server's ledger store. Requires a valid `[import_db]` stanza in the config file. |
| `--net`             | **DEPRECATED** Intended for debugging: do not build a local ledger until one can be obtained from the network. |
| `--nodetoshard`     | Before fully starting, copy any complete [history shards](history-sharding.html) from the ledger store into the shard store, up to the shard store's configured maximum disk space. Uses large amounts of CPU and I/O. Caution: this command copies data (instead of moving it), so you must have enough disk space to store the data in both the shard store and the ledger store. <!--{# Task for writing a tutorial to use this: DOC-1639 #}--> |
| `--quorum {QUORUM}` | This option is intended for starting [test networks](parallel-networks.html). Override the minimum quorum for validation by requiring an agreement of `{QUORUM}` trusted validators. By default, the quorum for validation is automatically set to a safe number of trusted validators based on how many there are. If some validators are not online, this option can allow progress with a lower than normal quorum. **Warning:** If you set the quorum manually, it may be too low to prevent your server from diverging from the rest of the network. Only use this option if you have a deep understanding of consensus and have a need to use a non-standard configuration. |

The following option has been removed: `--validateShards`. [Removed in: rippled 1.7.0][]

## Stand-Alone Mode Options

```bash
rippled --standalone [OPTIONS]
rippled -a [OPTIONS]
```
Run in [stand-alone mode](rippled-server-modes.html). In this mode, `rippled` does not connect to the network or perform consensus. (Otherwise, `rippled` runs in daemon mode.)

### Initial Ledger Options

以下选项确定启动时首先加载哪个分类帐。这些选项用于重放历史记录或启动测试网络。

| Option                | Description                                          |
|:----------------------|:-----------------------------------------------------|
| `--ledger {LEDGER}`   | Load the ledger version identified by `{LEDGER}` (either a ledger hash or a ledger index) as the initial ledger. The specified ledger version must be in the server's ledger store. |
| `--ledgerfile {FILE}` | Load the ledger version from the specified `{FILE}`, which must contain a complete ledger in JSON format. For an example of such a file, see the provided [`ledger-file.json`]({{target.github_forkurl}}/blob/{{target.github_branch}}/content/_code-samples/rippled-cli/ledger-file.json). |
| `--load`              | **DEPRECATED** Intended for debugging. Only load the initial ledger from the ledger store on disk. |
| `--replay`            | Intended for debugging. Use with `--ledger` to replay a ledger close. Your server must have the ledger in question and its direct ancestor already in the ledger store. Using the previous ledger as a base, the server processes all the transactions in the specified ledger, resulting in a re-creation of the specified ledger. With a debugger, you can add breakpoints to analyze specific transaction processing logic. |
| `--start`             | Intended for debugging. Start with a new genesis ledger that has all known amendments (except those the server is configured to vote against) enabled. The functionality of those amendments is therefore available starting from the second ledger, rather than going through the full two-week [Amendment Process](amendments.html). |
| `--valid`            | **DEPRECATED** Intended for debugging. Consider the initial ledger a valid network ledger even before fully syncing with the network. |

## Client Mode Options

```bash
rippled [OPTIONS] -- {COMMAND} {COMMAND_PARAMETERS}
```

在客户端模式下，`rippled`可执行文件充当另一个`rippled`服务的客户端(该服务可能是在本地单独进程中运行的同一个可执行文件，也可能是另一台服务器上的`Ripple`服务器。）

To run in client mode, provide the [commandline syntax](request-formatting.html#commandline-format) for one of the [`rippled` API](rippled-api.html) methods.

In addition to the individual command syntax, client mode accepts the [Generic Options](#generic-options) and the following options:

| Option                  | Description                                        |
|:------------------------|:---------------------------------------------------|
| `--rpc`                 | Explicitly specify that the server should run in client mode. Not required. |
| `--rpc_ip {IP_ADDRESS}` | Connect to the `rippled` server at the specified IP Address, optionally including a port number. |
| `--rpc_port {PORT}`     | **DEPRECATED** Connect to the `rippled` server on the specified port. Specify the port alongside the IP address using `--rpc_ip` instead. |

**Tip:** 有些参数接受负数作为值。若要确保API命令的参数不会被解释为选项，请在命令名之前传递`--`参数。

示例用法（从最早可用的分类帐版本到最新可用的分类帐版本获取帐户交易历史）：

```bash
rippled -- account_tx r9cZA1mLK5R5Am25ArfXFmqgNwjZgnfk59 -1 -1
```


## Unit Tests

```bash
rippled --unittest [OPTIONS]
rippled -u [OPTIONS]
```

单元测试运行内置于`rippled`源代码中的测试，以确认可执行文件是否按预期执行。运行单元测试后，进程将显示结果和出口的摘要。单元测试包括诸如内置数据类型和事务处理例程之类的功能。

如果单元测试报告失败，通常表示以下情况之一：

- 编译`时出现问题，导致`泛起涟漪，无法正常工作
- `涟漪`的源代码包含错误
- 单元测试有一个bug或者没有更新以说明新的行为

运行单元测试时，除了下列任何选项外，还可以指定[通用选项]（#通用选项）:

| Option                             | Short Version | Description             |
|:-----------------------------------|:--------------|:------------------------|
| `--unittest-ipv6`                  |               | 使用[iv6](https://en.wikipedia.org/wiki/IPv6)运行单元测试时连接到本地服务器。如果没有提供，单元测试将改用IPv4。 |
| `--unittest-jobs {NUMBER_OF_JOBS}` |               | 使用指定数量的进程来运行单元测试。这样可以更快地在多核系统上完成测试。作业}`的`{NUMBER\u应为正整数，表示要使用的进程数。 |
| `--unittest-log`                   |               | 即使指定了`--quiet`，也允许单元测试写入日志(否则不起作用。） |
| `--quiet`                          | `-q`          | 运行单元测试时打印更少的诊断消息。 |


### Specific Unit Tests

```bash
rippled --unittest={TEST_OR_PACKAGE_NAME}
```

默认情况下，`rippled`运行所有单元测试，但分类为"manual"的单元测试除外。您可以通过指定单个测试的名称来运行该测试，也可以通过指定包名称来运行测试的子集。

测试被分组到包的层次结构中，包由`.`个字符分隔，以测试用例名称结尾。

#### Printing Unit Tests

```bash
rippled --unittest=print
```

`print`单元测试是一个特例，它打印可用测试及其包的列表。

#### Manual Unit Tests

某些单元测试被归类为"手动"，因为它们需要很长时间才能完成。这些测试在`print`单元测试的输出中标记为`|M|`。当运行所有单元测试或单元测试包时，默认情况下不会运行手动测试。通过指定测试的名称，可以单独运行手动测试。例如:

```bash
$ ./rippled --unittest=ripple.tx.OversizeMeta
ripple.tx.OversizeMeta
Longest suite times:
   60.9s ripple.tx.OversizeMeta
60.9s, 1 suite, 1 case, 9016 tests total, 0 failures
```

#### Providing Arguments to Unit Tests

某些手动单元测试接受一个参数。可以使用以下选项提供参数:

| Option                  | Description                                        |
|:------------------------|:---------------------------------------------------|
| `--unittest-arg {ARG}`  | 为当前正在运行的单元测试提供参数`{ARG}`。接受参数的每个单元测试都定义自己的参数格式。  |


<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
