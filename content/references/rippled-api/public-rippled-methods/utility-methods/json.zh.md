---
html: json.html
parent: utility-methods.html
blurb: Pass JSON through the commandline.
---
# json

`json`方法是运行其他命令的代理，并接受命令的参数作为JSON值。它*专用于命令行客户机*，用于指定参数的命令行语法不充分或不需要的情况。

## Request Format
请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*Commandline*

```sh
# Syntax: json method json_stanza
rippled -q json ledger_closed '{}'
```

<!-- MULTICODE_BLOCK_END -->

## Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
   "result" : {
      "ledger_hash" : "8047C3ECF1FA66326C1E57694F6814A1C32867C04D3D68A851367EE2F89BBEF3",
      "ledger_index" : 390308,
      "status" : "success"
   }
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，使用与所执行命令类型相适应的字段。


{% include '_snippets/rippled_versions.md' %}
{% include '_snippets/rippled-api-links.md' %}
