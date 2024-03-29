## Chapter 6. System models

**Common models for communication links**
- Fair-loss link: assumes messages may be lost and duplicated. Retry transmission and they will eventually be delivered.
- Reliable link: assumes the message will be delivered exactly once.
- Autenticated reliable link: builds on top of reliable link but also perform authentication.

**Common models for process behaviours**
- Arbitrary-fault model: assumes the algorithm can fail in arbitrary ways, causing crashes or unexpeted behaviours by bugs or malicious activity (a.k.a the byzantine model).
- Crash-recovery model: assumes the process does not deviate from its algorithm but when it doesn't come back up when it crashes.
- Crash-stop model: assumes the process does not deviate from its algorithm and it will not come back online if it crashes.

**Timing models**
- Synchronous: assumes the duration for deliverying a message is bounded.
- Asynchronous: assumes the duration for delivering a message is unbounded.
- Partially synchronous: assumes the system behaves synchronously most of the time, which models most real world applications.

