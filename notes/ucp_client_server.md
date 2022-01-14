#1.main

```cpp
main
--parse_cmd
--init_context
----ucp_params.field_mask = UCP_PARAM_FIELD_FEATURES;
----ucp_init
------ucp_init_version
--------ucp_get_version
--------if (config == NULL)
----------ucp_config_read
------------ucs_config_parser_fill_opts
------------ucs_list_head_init(&config->cached_key_list);
--------context = ucs_calloc
--------ucs_list_head_init(&context->cached_key_list);
--------ucp_fill_config
--------ucp_fill_resources
--------ucp_context_create_vfs
----init_worker
```