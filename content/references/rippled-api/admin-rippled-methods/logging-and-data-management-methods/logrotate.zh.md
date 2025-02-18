---
html: logrotate.html
parent: logging-and-data-management-methods.html
blurb: Reopen the log file.
---
# logrotate
[[Source]](https://github.com/ripple/rippled/blob/743bd6c9175c472814448ea889413be79dfd1c07/src/ripple/rpc/handlers/LogRotate.cpp "Source")

The `logrotate` command closes and reopens the log file. This is intended to help with log rotation on Linux file systems.

Most Linux systems come pre-installed with a [`logrotate`](https://linux.die.net/man/8/logrotate) program, which is separate from this command. Application specific log rotation scripts are placed in `/etc/logrotate.d`

The following script is a sample that can be created as `/etc/logrotate.d/rippled`

```logrotate
/var/log/rippled/*.log {
  daily
  minsize 200M
  rotate 7
  nocreate
  missingok
  notifempty
  compress
  compresscmd /usr/bin/nice
  compressoptions -n19 ionice -c3 gzip
  compressext .gz
  postrotate
    /opt/ripple/bin/rippled --conf /opt/ripple/etc/rippled.cfg logrotate
  endscript
}
```

You can configure parameters such as `minsize` and `rotate` depending on the amount of logs you keep. Use the `log_level` setting in your `rippled.cfg` file to configure how verbose your server's logs are. This sample script is based on standard `log_level` and will store approximately 2 weeks worth of logs in a compressed format.

Starting with `rippled` 1.3, the script `/etc/logrotate.d/rippled` will be automatically installed by the DEB and RPM packages. You can make modifications to this as required. Your modifications will not be overwritten on package upgrades.

**Note:** You should have only one system logrotate script per application. Please ensure that you do not have any other log rotation that handles the same directory.

_The `logrotate` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._

### Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    "id": "lr1",
    "command": "logrotate"
}
```

*Commandline*

```sh
#Syntax: logrotate
rippled logrotate
```

<!-- MULTICODE_BLOCK_END -->

The request includes no parameters.

### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*JSON-RPC*

```json
200 OK

{
   "result" : {
      "message" : "The log file was closed and reopened.",
      "status" : "success"
   }
}

```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
   "result" : {
      "message" : "The log file was closed and reopened.",
      "status" : "success"
   }
}

```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field`   | Type   | Description                                             |
|:----------|:-------|:--------------------------------------------------------|
| `message` | String | On success, contains the message `The log file was closed and reopened.` |

### Possible Errors

* 任何[通用错误类型][]。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
