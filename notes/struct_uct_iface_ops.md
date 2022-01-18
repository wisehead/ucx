#1.struct uct_iface_ops

```cpp
/**
 * Transport interface operations.
 * Every operation exposed in the API must appear in the table below, to allow
 * creating interface/endpoint with custom operations.
 */
typedef struct uct_iface_ops {

    /* endpoint - put */
    uct_ep_put_short_func_t             ep_put_short;
    uct_ep_put_bcopy_func_t             ep_put_bcopy;
    uct_ep_put_zcopy_func_t             ep_put_zcopy;

    /* endpoint - get */
    uct_ep_get_short_func_t             ep_get_short;
    uct_ep_get_bcopy_func_t             ep_get_bcopy;
    uct_ep_get_zcopy_func_t             ep_get_zcopy;

    /* endpoint - active message */
    uct_ep_am_short_func_t              ep_am_short;
    uct_ep_am_short_iov_func_t          ep_am_short_iov;
    uct_ep_am_bcopy_func_t              ep_am_bcopy;
    uct_ep_am_zcopy_func_t              ep_am_zcopy;

    /* endpoint - atomics */
    uct_ep_atomic_cswap64_func_t        ep_atomic_cswap64;
    uct_ep_atomic_cswap32_func_t        ep_atomic_cswap32;
    uct_ep_atomic32_post_func_t         ep_atomic32_post;
    uct_ep_atomic64_post_func_t         ep_atomic64_post;
    uct_ep_atomic32_fetch_func_t        ep_atomic32_fetch;
    uct_ep_atomic64_fetch_func_t        ep_atomic64_fetch;

    /* endpoint - tagged operations */
    uct_ep_tag_eager_short_func_t       ep_tag_eager_short;
    uct_ep_tag_eager_bcopy_func_t       ep_tag_eager_bcopy;
    uct_ep_tag_eager_zcopy_func_t       ep_tag_eager_zcopy;
    uct_ep_tag_rndv_zcopy_func_t        ep_tag_rndv_zcopy;
    uct_ep_tag_rndv_cancel_func_t       ep_tag_rndv_cancel;
    uct_ep_tag_rndv_request_func_t      ep_tag_rndv_request;

    /* interface - tagged operations */
    uct_iface_tag_recv_zcopy_func_t     iface_tag_recv_zcopy;
    uct_iface_tag_recv_cancel_func_t    iface_tag_recv_cancel;

    /* endpoint - pending queue */
    uct_ep_pending_add_func_t           ep_pending_add;
    uct_ep_pending_purge_func_t         ep_pending_purge;

    /* endpoint - synchronization */
    uct_ep_flush_func_t                 ep_flush;
    uct_ep_fence_func_t                 ep_fence;
    uct_ep_check_func_t                 ep_check;

    /* endpoint - connection establishment */
    uct_ep_create_func_t                ep_create;
    uct_ep_connect_func_t               ep_connect;
    uct_ep_disconnect_func_t            ep_disconnect;
    uct_cm_ep_conn_notify_func_t        cm_ep_conn_notify;
    uct_ep_destroy_func_t               ep_destroy;
    uct_ep_get_address_func_t           ep_get_address;
    uct_ep_connect_to_ep_func_t         ep_connect_to_ep;
    uct_iface_accept_func_t             iface_accept;
    uct_iface_reject_func_t             iface_reject;

    /* interface - synchronization */
    uct_iface_flush_func_t              iface_flush;
    uct_iface_fence_func_t              iface_fence;
    

    /* interface - progress control */
    uct_iface_progress_enable_func_t    iface_progress_enable;
    uct_iface_progress_disable_func_t   iface_progress_disable;
    uct_iface_progress_func_t           iface_progress;

    /* interface - events */
    uct_iface_event_fd_get_func_t       iface_event_fd_get;
    uct_iface_event_arm_func_t          iface_event_arm;

    /* interface - management */
    uct_iface_close_func_t              iface_close;
    uct_iface_query_func_t              iface_query;

    /* interface - connection establishment */
    uct_iface_get_device_address_func_t iface_get_device_address;
    uct_iface_get_address_func_t        iface_get_address;
    uct_iface_is_reachable_func_t       iface_is_reachable;

} uct_iface_ops_t;
```