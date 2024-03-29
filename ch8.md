## Chapter 8. Time

### 8.1 Physical clocks
- **Physical wall-time block**
  - Based on vibrating quartz crystal
  - Cheap but inaccurate. Needs to be resynced often to remain accurate.
  - Clock drift: the rate in which the clock runs faster or slower.
  - Clock skew: the difference between two blocks at a specific point in time.
- **Atomic clock**
  - Expensive as it is based on quantum-mechanical properties of atoms
  - Accurate to 1 second in 3 million years
- **Network Time Protocol (NTP)**
  - Timetsamp is retrived from a NTP server and further corrected to account for the latency of the request.
  - Time may go backfoward or forward as a result, making timestamps comparisons inaccurate.
- **Monotic clock**
  - Measures the elapsed time (often in seconds) since an arbitrary point
  - Suitable only for a single node
- In a single-threaded process in which operations must happen one bfore the other. The happened-before relationship creates a causal bond between the two operations.

### 8.2 Logical clocks
- **Logical clock**
  - Measures time passage based on logical operations instead of wall-clock time, like a simple counter.
- **Lamport clock**
  - A local counter that have the following rules
    - Initialized as 0
    - The process increments its counter by 1 before executing an operation
    - When the process sends a message, it increments the counter by 1 and make a copy of the counter in the message.
    - When the process receives a message, it sets the counter to max(counter_received, local_counter) + 1.
    - Provides the guarantee that if operation A happened-before operation B, the logical timestamp of operation A is less than the one of operation B.
  - Unrelated operations can have the same logical timestamp.
  - The order of logical timestamp doesn't imply a causal relationship.
  
### 8.3 Vector clocks
- **Vector block**
  - Provides the guarantee that if a logical timestamp is less than another, then the former must have happened-before the latter.
  - Uses an array of counters, one counter for each process , and all processes have a copy of the counters.
  - For example, if system has process 1, 2, 3, then each process has a vector block with [C_1, C_2, C_3].
  - A process updates it local clock based on the following rules:
    - All counters are zeros initially
    - When an operation occurs, the process adds 1 to its counter.
    - When the process sends a message, it adds 1 to its counter and sends a copy of the array with the message.
    - When the process receivesa message, it merges the array with the local one by taking the max of the two arrays element-wise. Finally, it adds its own counter by 1.
  - Given two operations O1 and O2, and timestamps T1 and T2
    - if
      - every counter in T1 <= T2
      - there's at least one counter in T1 that is < T2
    - then O1 happened-before O2.
    - if O1 didn't happen before O2 and O2 didn't happen before O1
      - then O1 and O2 are considered to be concurrent
  - Storage requirement grows linearly with the number of processes. May consider dotted version vectors that is optimized for storage.
- In general, we should not use physical clocks to accurately derive the order of events.