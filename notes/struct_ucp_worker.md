#1.struct ucp_worker

```cpp
/**
 * UCP worker (thread context).
 */
typedef struct ucp_worker {
    uint64_t                         flags;               /* Worker flags */
    ucs_async_context_t              async;               /* Async context for this worker */
    ucp_context_h                    context;             /* Back-reference to UCP context */
    uint64_t                         uuid;                /* Unique ID for wireup */
    uint64_t                         client_id;           /* Worker client id for wireup */
    uct_worker_h                     uct;                 /* UCT worker handle */
    ucs_mpool_t                      req_mp;              /* Memory pool for requests */
    ucs_mpool_t                      rkey_mp;             /* Pool for small memory keys */
    ucp_tl_bitmap_t                  atomic_tls;          /* Which resources can be used for atomics */

    int                              inprogress;
    /* Worker name for tracing and analysis */
    char                             name[UCP_ENTITY_NAME_MAX];
    /* Worker address name composed of host name and process id */
    char                             address_name[UCP_WORKER_ADDRESS_NAME_MAX];

    unsigned                         flush_ops_count;     /* Number of pending operations */

    int                              event_fd;            /* Allocated (on-demand) event fd for wakeup */
    ucs_sys_event_set_t              *event_set;          /* Allocated UCS event set for wakeup */
    int                              eventfd;             /* Event fd to support signal() calls */
    unsigned                         uct_events;          /* UCT arm events */
    ucs_list_link_t                  arm_ifaces;          /* List of interfaces to arm */

    void                             *user_data;          /* User-defined data */
    ucs_strided_alloc_t              ep_alloc;            /* Endpoint allocator */
    ucs_list_link_t                  stream_ready_eps;    /* List of EPs with received stream data */
    unsigned                         num_all_eps;         /* Number of all endpoints (except internal
                                                           * endpoints) */
    ucs_list_link_t                  all_eps;             /* List of all endpoints (except internal
                                                           * endpoints) */
    ucs_list_link_t                  internal_eps;        /* List of internal endpoints */
    ucs_conn_match_ctx_t             conn_match_ctx;      /* Endpoint-to-endpoint matching context */
    ucp_worker_iface_t               **ifaces;            /* Array of pointers to interfaces,
                                                             one for each resource */
    unsigned                         num_ifaces;          /* Number of elements in ifaces array  */
    unsigned                         num_active_ifaces;   /* Number of activated ifaces  */
    ucp_tl_bitmap_t                  scalable_tl_bitmap;  /* Map of scalable tl resources */
    ucp_worker_cm_t                  *cms;                /* Array of CMs, one for each component */
    ucs_mpool_set_t                  am_mps;              /* Memory pool set for AM receives */
    ucs_mpool_t                      reg_mp;              /* Registered memory pool */
    ucp_worker_mpool_hash_t          mpool_hash;          /* Hash table of memory pools */
    ucs_queue_head_t                 rkey_ptr_reqs;       /* Queue of submitted RKEY PTR requests that
                                                           * are in-progress */
    uct_worker_cb_id_t               rkey_ptr_cb_id;      /* RKEY PTR worker callback queue ID */
    ucp_tag_match_t                  tm;                  /* Tag-matching queues and offload info */
    ucp_am_info_t                    am;                  /* Array of AM callbacks and their data */
    uint64_t                         am_message_id;       /* For matching long AMs */
    ucp_ep_h                         mem_type_ep[UCS_MEMORY_TYPE_LAST]; /* Memory type EPs */

    UCS_STATS_NODE_DECLARE(stats)
    UCS_STATS_NODE_DECLARE(tm_offload_stats)

    ucs_cpu_set_t                    cpu_mask;            /* Save CPU mask for subsequent calls to
                                                             ucp_worker_listen */

    ucp_worker_rkey_config_hash_t    rkey_config_hash;    /* RKEY config key -> index */
    ucp_worker_discard_uct_ep_hash_t discard_uct_ep_hash; /* Hash of discarded UCT EPs */
    UCS_PTR_MAP_T(ep)                ep_map;              /* UCP ep key to ptr
                                                             mapping */
    UCS_PTR_MAP_T(request)           request_map;         /* UCP requests key to
                                                             ptr mapping */

    unsigned                         ep_config_count;     /* Current number of ep configurations */
    ucp_ep_config_t                  ep_config[UCP_WORKER_MAX_EP_CONFIG];

    unsigned                         rkey_config_count;   /* Current number of rkey configurations */
    ucp_rkey_config_t                rkey_config[UCP_WORKER_MAX_RKEY_CONFIG];

    struct {
        int                          timerfd;             /* Timer needed to signal to user's fd when
                                                           * the next keepalive round must be done */
        uct_worker_cb_id_t           cb_id;               /* Keepalive callback id */
        ucs_time_t                   last_round;          /* Last round timestamp */
        ucs_list_link_t              *iter;               /* Last EP processed keepalive */
        ucp_lane_map_t               lane_map;            /* Lane map used to retry after no-resources */
        unsigned                     ep_count;            /* Number of EPs processed in current time slot */
        unsigned                     iter_count;          /* Number of progress iterations to skip,
                                                           * used to minimize call of ucs_get_time */
        size_t                       round_count;         /* Number of rounds done */
    } keepalive;

    struct {
        /* Number of requests to create endpoint */
        uint64_t                     ep_creations;
        /* Number of failed requests to create endpoint */
        uint64_t                     ep_creation_failures;
        /* Number of endpoint closures */
        uint64_t                     ep_closures;
        /* Number of failed endpoints */
        uint64_t                     ep_failures;
    } counters;
} ucp_worker_t;    
```