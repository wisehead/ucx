#1.uct_worker

```cpp
/**
 *  A progress engine and a domain for allocating communication resources.
 *  Different workers are progressed independently.
 */
typedef struct uct_worker {
    ucs_callbackq_t        progress_q;
} uct_worker_t;
```

#2. ucs_callbackq

```cpp
/**
 * A queue of callback to execute
 */
struct ucs_callbackq {
    /**
     * Array of fast-path element, the last is reserved as a sentinel to mark
     * array end.
     */
    ucs_callbackq_elem_t           fast_elems[UCS_CALLBACKQ_FAST_COUNT + 1];

    /**
     * Private data, which we don't want to expose in API to avoid pulling
     * more header files
     */
    char                           priv[72];
};
```

#3. ucs_callbackq_elem

```cpp
/**
 * Callback queue element.
 */
struct ucs_callbackq_elem {
    ucs_callback_t                 cb;       /**< Callback function */
    void                           *arg;     /**< Function argument */
    unsigned                       flags;    /**< Callback flags */
    int                            id;       /**< Callback id */
};
```

#4. ucs_callback_t

```cpp
/**
 * Callback which can be placed in a queue.
 *
 * @param [in] arg  User-defined argument for the callback.
 *
 * @return Count of how much "work" was done by the callback. For example, zero
 *         means that no work was done, and any nonzero value means that something
 *         was done.
 */
typedef unsigned (*ucs_callback_t)(void *arg);
```