#1.UCS_ARRAY_DECLARE_FUNCS

```cpp
/**
 * Declare the function prototypes of an array
 *
 * @param _name        Array name
 * @param _index_type  Type of array's index
 * @param _value_type  Type of array's values
 * @param _scope       Scope for array's functions (e.g 'static inline')
 */
#define UCS_ARRAY_DECLARE_FUNCS(_name, _index_type, _value_type, _scope) \
    \
    _scope ucs_status_t \
    UCS_ARRAY_IDENTIFIER(_name, _reserve)(ucs_array_t(_name) *array, \
                                          _index_type min_capacity); \
    \
    _scope ucs_status_t \
    UCS_ARRAY_IDENTIFIER(_name, _append)(ucs_array_t(_name) *array);
    
```

#2.caller of UCS_ARRAY_DECLARE_FUNCS

```cpp
UCS_ARRAY_DECLARE_FUNCS(ucp_proto_perf_envelope, unsigned,
                        ucp_proto_perf_envelope_elem_t,);
                        
```