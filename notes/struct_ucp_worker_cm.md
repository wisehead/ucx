#1.struct ucp_worker_cm

```cpp

/**
 * UCP worker CM, which encapsulates UCT CM and its auxiliary info.
 */
struct ucp_worker_cm {
    uct_cm_h                      cm;            /* UCT CM handle */
    uct_cm_attr_t                 attr;          /* UCT CM attributes */
    ucp_rsc_index_t               cmpt_idx;      /* Index of corresponding
                                                    component */
};
```

#2. uct_cm_h

```cpp
struct uct_cm {
    uct_cm_ops_t         *ops;
    uct_component_h      component;
    uct_base_iface_t     iface;

    struct {
        ucs_log_level_t  failure_level;
        int              reuse_addr;
    } config;
};
```

#3.struct uct_cm_ops

```cpp

/**
 * Connection manager component operations
 */
typedef struct uct_cm_ops {
    void         (*close)(uct_cm_h cm);
    ucs_status_t (*cm_query)(uct_cm_h cm, uct_cm_attr_t *cm_attr);
    ucs_status_t (*listener_create)(uct_cm_h cm, const struct sockaddr *saddr,
                                    socklen_t socklen,
                                    const uct_listener_params_t *params,
                                    uct_listener_h *listener_p);
    ucs_status_t (*listener_reject)(uct_listener_h listener,
                                    uct_conn_request_h conn_request);
    ucs_status_t (*listener_query) (uct_listener_h listener,
                                    uct_listener_attr_t *listener_attr);
    void         (*listener_destroy)(uct_listener_h listener);
    ucs_status_t (*ep_create)(const uct_ep_params_t *params, uct_ep_h *ep_p);
} uct_cm_ops_t;
```


#4. uct_tcp_sockcm_ops

```cpp
static uct_cm_ops_t uct_tcp_sockcm_ops = {
    .close            = UCS_CLASS_DELETE_FUNC_NAME(uct_tcp_sockcm_t),
    .cm_query         = uct_tcp_sockcm_query,
    .listener_create  = UCS_CLASS_NEW_FUNC_NAME(uct_tcp_listener_t),
    .listener_reject  = uct_tcp_listener_reject,
    .listener_query   = uct_tcp_listener_query,
    .listener_destroy = UCS_CLASS_DELETE_FUNC_NAME(uct_tcp_listener_t),
    .ep_create        = uct_tcp_sockcm_ep_create
};

```

#5. uct_rdmacm_cm_ops

```cpp
static uct_cm_ops_t uct_rdmacm_cm_ops = {
    .close            = UCS_CLASS_DELETE_FUNC_NAME(uct_rdmacm_cm_t),
    .cm_query         = uct_rdmacm_cm_query,
    .listener_create  = UCS_CLASS_NEW_FUNC_NAME(uct_rdmacm_listener_t),
    .listener_reject  = uct_rdmacm_listener_reject,
    .listener_query   = uct_rdmacm_listener_query,
    .listener_destroy = UCS_CLASS_DELETE_FUNC_NAME(uct_rdmacm_listener_t),
    .ep_create        = UCS_CLASS_NEW_FUNC_NAME(uct_rdmacm_cm_ep_t)
};
```