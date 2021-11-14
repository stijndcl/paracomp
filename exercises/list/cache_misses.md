# Cache misses
Consider the following two implementations of an algorithm that adds all elements of an array.
Implementation 1:
```
for (i=0; i<m*n; i++) {
  sum += a[i];
}
```
Implementation 2:
```
for (i=0; i<m; i++) {
  for (j=0; j<n; j++) {
    sum += a[m*j+i];
  }
}
```

Assume that the array a\[] contains m.n 32-bit integers. The variable m is selected such that
exactly m elements of array a\[] fit in one cache block, i.e., the size of a cache block is 4.m
bytes.

a) Indicate the access pattern for both implementations for an array consisting of 16 elements 
and a cache block size of 16 bytes. Mark the element that is referenced first with a '1', the second
element with a '2', etc. Use circles to mark the memory references that will be cache misses.

_Note: I indicate cache misses in **bold** instead of using circles because Markdown_

Implementation 1:

[**1**, 2, 3, 4, **5**, 6, 7, 8, **9**, 10, 11, 12, **13**, 14, 15, 16]

Implementation 2:

_This one doesn't fill them from left to right but it jumps all over the place because of the loop order_

[**1**, 5, 9, 13, **2**, 6, 10, 14, **3**, 7, 11, 15, **4**, 8, 12, 16]


b) Can there be a difference in execution speed for both implementations on an out-of-order
microprocessor with non-blocking caches? Explain why (not).

Yes, the second one will be faster:
- The first implementation will load a cache block, but all elements are in the same cache line so the next cache block isn't loaded as it isn't needed yet
- The second will load all 4 cache blocks after each other because all of them are required from the start
