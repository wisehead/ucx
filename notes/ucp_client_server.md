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
--------ucp_fill_config//todo
--------ucp_fill_resources//todo
--------ucp_context_create_vfs
----init_worker
--if (server_addr == NULL)
----run_server
--else
----run_client
```

#2. run_server

```cpp
run_server
--init_worker
--if (send_recv_type == CLIENT_SERVER_SEND_RECV_AM)
----ucp_worker_set_am_recv_handler
------ucp_worker_set_am_handler_common
--------ucs_array_reserve(ucp_am_cbs, &worker->am.cbs, id + 1)
------ucp_worker_am_init_handler
```

#3. ucs_array_reserve

```cpp
ucs_array_reserve(ucp_am_cbs, &worker->am.cbs, id + 1)
/*
 * Grow the array memory buffer to the at least required capacity. This does not
 * change the array length or existing contents, but the backing buffer can be
 * relocated to another memory area.
 *
 * @param _name    Array name
 * @param _array   Array to reserve buffer for
 *
 * @return UCS_OK if successful, UCS_ERR_NO_MEMORY if cannot grow the array
 */
#define ucs_array_reserve(_name, _array, _min_capacity) \
    UCS_ARRAY_IDENTIFIER(_name, _reserve)(_array, _min_capacity)
ucs_array_ucp_am_cbs_reserve(worker->am.cbs, id+1)
    
```