#1.struct ucp_listener

```cpp
/**
 * UCP listener
 */
typedef struct ucp_listener {
    ucp_worker_h                   worker;

    union {
        ucp_worker_iface_t         **wifaces; /* Array of UCT interface
                                                 pointers to listen on */
        uct_listener_h             *listeners;/* Array of UCT listeners to
                                                 listen on */
    };

    struct sockaddr_storage        sockaddr;  /* Listening sockaddr */
    ucp_rsc_index_t                num_rscs;  /* Number of UCT listening
                                                 resources  (wifaces or
                                                 listeners) */
    ucp_listener_accept_callback_t accept_cb; /* Listen accept callback
                                                 which creates an endpoint
                                               */
    ucp_listener_conn_callback_t   conn_cb;   /* Listen callback which
                                                 creates a handle to
                                                 connection request to the
                                                 remote endpoint */
    void                           *arg;      /* User's arg for the accept
                                                 callback */
    uct_worker_cb_id_t             prog_id;   /* Slow-path callback */
} ucp_listener_t;

```

#2. uct_listener_h

```cpp
/**
 * Listener for incoming connections
 */
typedef struct uct_listener {
    uct_cm_h                 cm;
} uct_listener_t;

```

#3. ucp_worker_iface

```cpp
/**
 * UCP worker iface, which encapsulates UCT iface, its attributes and
 * some auxiliary info needed for tag matching offloads.
 */
struct ucp_worker_iface {
    uct_iface_h                   iface;         /* UCT interface */
    uct_iface_attr_t              attr;          /* UCT interface attributes */
    ucp_worker_h                  worker;        /* The parent worker */
    ucs_list_link_t               arm_list;      /* Element in arm_ifaces list */
    ucp_rsc_index_t               rsc_index;     /* Resource index */
    int                           event_fd;      /* Event FD, or -1 if undefined */
    unsigned                      activate_count;/* How many times this iface has
                                                    been activated */
    int                           check_events_id;/* Callback id for check_events */
    unsigned                      proxy_recv_count;/* Counts active messages on proxy handler */
    unsigned                      post_count;    /* Counts uncompleted requests which are
                                                    offloaded to the transport */
    uint8_t                       flags;         /* Interface flags */
};

```

#4. uct_iface

```cpp
/**
 * Communication interface context
 */
typedef struct uct_iface {
    uct_iface_ops_t          ops;
} uct_iface_t;
```