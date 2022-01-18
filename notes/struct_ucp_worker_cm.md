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