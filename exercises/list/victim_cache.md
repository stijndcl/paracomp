# Victim cache

Consider a direct-mapped cache with 4 sets and a fully associative victim cache with 2 elements.
The block size in both the direct-mapped cache and the victim cache is 16 bytes. The
replacement policy in the victim cache is LRU. The memory is byte-addressable. Complete the
following table. Mark an access as a hit if it hits in the direct-mapped cache or in the victim cache;
a miss means a miss in the direct-mapped cache and the victim cache. The elements in the other
columns represent address tags. In the victim cache, the least recently used block is indicated by a
‘LRU’ label.

| ------------ | ---------- | Direct-mapped cache | ------------ Victim cache ------------ |
|-|-|---------------------|--------------|

| Address | Hit/mis | set 0 | set 1 | set 2 | set 3 | set 0   | set 1     |
| ------- | ------- | ----- | ----- | ----- | ----- | -----   | -----     |
|         |         | --    | 110   | --    | FF0   | 1F0     | 210 / LRU |
| 080     | miss    | 080   |       |       |       |         |           |
| 0A0     | miss    |       |       | 0A0   |       |         |           |
| 200     | miss    | 200   |       |       |       | IFO/LRU | 080       |
| 080     | hit     | 080   |       |       |       | IFO/LRU | 200       |
| 0B0     | miss    |       |       |       | 0B0   | FFO/LRU | 200       |
| 0E0     | miss    |       |       | 0E0   |       | 0A0/LRU | 200       |
| 200     | hit     | 200   |       |       |       | 0A0/LRU | 080       |
| 080     | hit     | 080   |       |       |       | 0A0/LRU | 200       |
| 200     | hit     | 200   |       |       |       | 0A0/LRU | 080       |
