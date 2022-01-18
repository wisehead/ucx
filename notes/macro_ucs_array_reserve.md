#1. ucs_array_reserve

```cpp
ucs_array_reserve(ucp_am_cbs, &worker->am.cbs, id + 1)
/*
 * Grow the array memory buffer to the at least required capacity. This does not
 * change the array length or existing contents, but the backing buffer can be
 * relocated to another memory area.
 *
 * @param _name    Array name
 * @param _array   Array to reserve buffer for
 *
 * @return UCS_OK if successful, UCS_ERR_NO_MEMORY if cannot grow the array
 */
#define ucs_array_reserve(_name, _array, _min_capacity) \
    UCS_ARRAY_IDENTIFIER(_name, _reserve)(_array, _min_capacity)
ucs_array_ucp_am_cbs_reserve(worker->am.cbs, id+1)
    
```