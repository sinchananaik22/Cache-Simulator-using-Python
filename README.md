# Cache-Simulator-using-Python
Cache Simulator
This Cache Simulator project is a Python-based simulation tool that models the behavior of a cache memory system. It supports various cache configurations, including different cache associativities and replacement policies. The simulator can track cache hits, misses, and calculate the hit rate, providing insights into cache performance based on user-defined parameters.

Features
Cache Configurations:

Direct-Mapped Cache
Set-Associative Cache (User-defined associativity)
Fully-Associative Cache
Replacement Policies:

Least Recently Used (LRU)
First-In-First-Out (FIFO)
Random
Simulation Metrics:

Number of Cache Hits
Number of Cache Misses
Cache Hit Rate
How It Works
Initialization: The simulator is initialized with parameters like cache capacity, block size, main memory capacity, cache associativity, and replacement policy.

Memory Access: The simulator accepts memory addresses and determines whether each access results in a hit or a miss. If the block is not in the cache (miss), it loads the block into the cache, potentially evicting an existing block based on the chosen replacement policy.

Eviction: When the cache is full, blocks are evicted based on the selected replacement policy (LRU, FIFO, or Random).

Statistics: The simulator provides statistics on the number of cache hits, misses, and the hit rate.
