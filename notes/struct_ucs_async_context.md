#1.ucs_async_context

```cpp
/**
 * Async event context. Manages timer and fd notifications.
 */
struct ucs_async_context {
    union {
        ucs_async_thread_context_t thread;
        ucs_async_signal_context_t signal;
        int                        poll_block;
    };

    ucs_async_mode_t  mode;          /* Event delivery mode */
    volatile uint32_t num_handlers;  /* Number of event and timer handlers */
    ucs_mpmc_queue_t  missed;        /* Miss queue */
    ucs_time_t        last_wakeup;   /* time of the last wakeup */
};
```

#2. ucs_mpmc_queue

```cpp
/**
 * A Multi-producer-multi-consumer thread-safe queue.
 * Every push/pull is a single atomic operation in "good" scenario.
 * The queue can contain small integers up to UCS_MPMC_VALUE_MAX.
 *
 * TODO make the queue resizeable.
 */
typedef struct ucs_mpmc_queue {
    uint32_t           length;      /* Array size. Rounded to power of 2. */
    int                shift;
    volatile uint32_t  producer;    /* Producer index */
    volatile uint32_t  consumer;    /* Consumer index */
    uint64_t           *queue;      /* Array of data */
} ucs_mpmc_queue_t;
```