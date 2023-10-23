# postgres_LRU_buffer_replacement
Replaced clock sweep algorithm in postgres buffer manager with least recently used algorithm.

### Files modified:
bufmgr.c
- Added a call to StrategyFreeBuffer() at the end of UnpinBuffer() to update the LRU.

freelist.c
- Removed clock sweep logic
- Added LRU logic
    - StrategyGetBuffer() was modified to utilize the LRU algorithm for retrieving a buffer for the freelist. In essense, it fetches the first buffer in the freelist and updates the freelist.
    - StrategyFreeBuffer() was modified to maintain the order of buffers based on recent usage (i.e., the first buffer in the list is the least recently used).
    - This included maintaining atomicity and only modifying code where necessary.
