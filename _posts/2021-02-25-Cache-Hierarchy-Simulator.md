---
layout: post
title: "Project1 (Computer Architecture) - Cache Hierarchy Simulator"
markdown: kramdown
date: 2021-02-25
---

This post is to record my project1 in Advanced/High Performance Computing Architecture.
##### Introduction
Caches are complex memories that are often difficult to understand. One way to understand them is to build them. However, building caches is time consuming, 
and as the case with computer architecture often is, writing a simulator is nearly as effective and far more time-efficient. Hence, in this project, we will write a cache simulator that can 
simulate memory traces containing data addresses, verify its functional correctness on traces from the SPEC 2017 benchmark suite, and finally run experiments on a given workload to find an optimal cache 
configuration for that workload.  
This simulator should support a configuration data cache with an optimal victim cache. The replacement policy is configurable between LRU, FIFO, and LFU-NotMRU, while the write policy is WBWA.
##### Cache Hierarchy Layout
The simulator should model an L1 Data Cache which is also connected to a Victim cache.
* Accesses, either Loads or Store, first access the L1 cache, and on a miss access the Victim Cache. The Victim Cache retrieved data to both the CPU and the L1-Data cache.
* The L1 cache is represented with three parameters (C, B, S) where: 
    * 2<sup>C</sup> is the cache size in bytes
    * 2<sup>B</sup> is the size of each block in bytes
    * 2<sup>S</sup> is the number of ways in each set
* The victim cache is configurable, i.e. it may or may not be present in the simulation, and if present, is sized by the V parameter in the config file. The Victim Cache can hold a maximum of 8 cache blovks,
 and uses the FIFO replacement policy.
* Memory addresses for each access are 64-bits long.
* The Miss Penalty for accessing main memory is 60.0 cycles.
* The caches are byte addressable.
* The L1 cache implements the WBWA - Write Back, Write Allocate policy.
* The L1 cache uses one of the following replacement policies:
    * LRU - Least Recently Used (the oldest block in age of use is kicked out)
    * FIFO - First in First Out (The oldest block in age of first use is kicked out)
    * LFU-NotMRU - Least Frequently Used - Not the Most Recently Used (The least Frequently block which is not then Most Recently Used block is kicked out)
* All valid bits and dirty bits(if applicable) are set to 0 when the simulation begins.

##### Cache Operations
* First, the L1 cache is checked for the block. A miss triggers a search in the victim cache. If the block is found in the victim cache, it is promoted 
to the L1 cache. If the block is not found in the victim cache, it is fetched from main memory. <b>In reality</b>, the Victim cache is checked in parallel with the L1 cache, 
and its access time is encompassed by the access time of L1 cache. The effect is achieved in the simulation by assuming that hit time of the Victim cache is 0.
* Blocks installed in the L1 cache from the victim cache follow the replacement policy for insertion. That is, an installation from victim cache to the L1 cache is similar to 
fetching the block from main memory, except the block's dirty bit is preserved.
* Replacement policy information is updated on hitting or installing a block in a cache. More specifically update the replacement information if:
    * An access hits in either the L1 cache
    * A block is installed in the L1-Data cache from the Victim Cache
    * A block is installed in the L1-Data cache from the main memory
* If the access to the data cache is a write (i.e. a store instruction), the dirty bit is set.
* The victim cache stores blocks evicted from the L1 cahce. It uses FIFO replacement, which means that if a new block has to be inserted in the victim cache when it is full, the oldest block in the victim cache is kicked out.
If the victim cache is dirty, it is written back to main memory.
* The LFU-NotMRU replacement policy can have conflicts wherein two blocks could have
the same use frequency. In that scenario, ties can be arbitrarily broken. In our simulator,
we break ties by evicting the block with the higher tag. For example, if one block has tag
0x100, and the other has tag 0x200, and they both have the same use frequency, the block
with tag 0x200 is evicted.
               
##### Conclusion
Based on all the conditions above, I drew the flow chart for this simulation as below.
<img  class="img-content" alt="Cache Layout" width="600"  src="/assets/img/L1-Victim_Cache.jpg">

###### Reference  
Project1 in Advanced/High performance computing architecture