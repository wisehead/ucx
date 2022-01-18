#1. ucp_listener_create

```cpp
ucp_listener_create(ucp_worker, &params, listener_p);
--UCS_ASYNC_BLOCK(&worker->async);
--ucp_listen(listener, params);
----ucs_sockaddr_get_port
----uct_params.conn_request_cb  = ucp_cm_server_conn_request_cb;
----uct_params.user_data        = listener;
----uct_listeners               = ucs_calloc(num_cms, sizeof(*uct_listeners),uct_listeners_arr");
----listener->listeners = uct_listeners;
----while (cm_index < num_cms)
------ucp_cm = &worker->cms[cm_index++];
------uct_listener_create
--------cm->ops->listener_create(cm, saddr, socklen, params, listener_p);
----------UCS_CLASS_NEW_FUNC_NAME(uct_rdmacm_listener_t)
------uct_listener_query
--------listener->cm->ops->listener_query(listener, listener_attr)
----------uct_rdmacm_listener_query
--ucp_listener_vfs_init
----ucs_vfs_obj_add_dir(listener->worker, listener, "listener/%p", listener);
----ucs_vfs_obj_add_ro_file(listener, ucp_listener_vfs_show_ip, NULL, 0, "ip");
----ucs_vfs_obj_add_ro_file(listener, ucp_listener_vfs_show_port, NULL, 0, "port");
--UCS_ASYNC_UNBLOCK(&worker->async);

```