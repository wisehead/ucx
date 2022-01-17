#1.struct ucp_am_info

```cpp
typedef struct ucp_am_info {
    size_t                   alignment;
    ucs_array_t(ucp_am_cbs)  cbs;
} ucp_am_info_t;
```


#2. ucp_am_cbs

```cpp
UCS_ARRAY_DECLARE_TYPE(ucp_am_cbs, unsigned, ucp_am_entry_t)

/**
 * Declare an array type.
 * The array keeps track of its capacity and length (current number of elements)
 * It can be either fixed-size or dynamically-growing.
 *
 * @param _name         Array definition name (needed for some array operations)
 * @param _index_type   Type of array's index (e.g size_t, int, ...)
 * @param _value_type   Type of array's values (could be anything)
 */
#define UCS_ARRAY_DECLARE_TYPE(_name, _index_type, _value_type) \
    typedef struct { \
        _value_type       *buffer; \
        _index_type       length; \
        _index_type       capacity; \
    } ucs_array_t(_name); \
    \
    typedef _value_type UCS_ARRAY_IDENTIFIER(_name, _value_type_t);
```

#3.ucs\_array\_t(_name)

```cpp
ucs_array_t(ucp_am_cbs) = ucs_array_ucp_am_cbs_t

/**
 * Expands to array type definition
 *
 * @param _name Array name (as passed to UCS_ARRAY_DECLARE)
 *
 * Example:
 *
 * @code{.c}
 * ucs_array_t(int_array) my_array;
 * @endcode
 */
#define ucs_array_t(_name) \
    UCS_ARRAY_IDENTIFIER(_name, _t)

/* Internal macro to construct array identifier from name */
#define UCS_ARRAY_IDENTIFIER(_name, _suffix) \
    UCS_PP_TOKENPASTE(ucs_array_, UCS_PP_TOKENPASTE(_name, _suffix))    

/* Paste two expanded tokens */
#define __UCS_TOKENPASTE_HELPER(x, y)     x ## y
#define UCS_PP_TOKENPASTE(x, y)           __UCS_TOKENPASTE_HELPER(x, y)
    
```

#4. ucp_am_entry_t

```cpp
/**
 * Data that is stored about each callback registered with a worker
 */
typedef struct ucp_am_entry {
    union {
        ucp_am_callback_t      cb_old;   /* user defined callback, used by legacy API */
        ucp_am_recv_callback_t cb;       /* user defined callback */
    };
    void                       *context;   /* user defined callback argument */
    unsigned                   flags;      /* flags affecting callback behavior
                                              (set by the user) */
} ucp_am_entry_t;
```

