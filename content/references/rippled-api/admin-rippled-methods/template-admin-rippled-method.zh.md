# TODO_method_name
[[Source]](TODO_URL "Source")

The `{{currentpage.name}}` method TODO_description.

_The `{{currentpage.name}}` method is an [admin method](admin-rippled-methods.html) that cannot be run by unprivileged users._


### Request Format

请求格式的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    TODO
}
```

*JSON-RPC*

```json
{
    "method": "{{currentpage.name}}",
    "params": [{
        TODO
    }]
}
```

*Commandline*

```sh
#Syntax: {{currentpage.name}} TODO
rippled {{currentpage.name}}
```

<!-- MULTICODE_BLOCK_END -->

请求包括以下参数:

| `Field`     | Type                      | Description                        |
|:------------|:--------------------------|:-----------------------------------|
TODO_request_params


### Response Format

成功响应的示例:

<!-- MULTICODE_BLOCK_START -->

*WebSocket*

```json
{
    TODO
}
```

*JSON-RPC*

```json
{
  TODO
}
```

*Commandline*

```json
Loading: "/etc/rippled.cfg"
Connecting to 127.0.0.1:5005

{
  TODO
}
```

<!-- MULTICODE_BLOCK_END -->

响应遵循[标准格式][]，成功的结果包含以下字段:

| `Field` | Type   | Description                                               |
|:--------|:-------|:----------------------------------------------------------|
TODO_response_params


### Possible Errors

- Any of the [universal error types][].
- TODO_errors
- `invalidParams` - 一个或多个字段指定不正确，或者缺少一个或多个必需字段。

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
