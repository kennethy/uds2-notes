## Chapter 9. Leader election

- A leader election algorithm needs to ensure there is at most one leader at all times (safety) and that an election will eventally completes even in the presence of failures (liveness).
  
### 9.1 Raft leader election
- Implemented as a state machine, and a process can only be in one of three states:
  - The follower state
  - The candidate state: the process starts a new election and proposes itself as a leader.
  - The leader state
- Time is divided into election terms with arbitrary length and with a consecutive integer assigned (logical timestamp).
- The leader would sent out heartbeart to its followers. If a follower does not receive heartbeat within a certin period of time, it starts a new election by incrementing the term counter and transitions itself to the candidate state. It sends requests (along with the current election term) to all other followers to make a vote.
- Possible election outcomes
  - Wins the election: each process can vote for at most one candidate on a first-come-first-serve basis. If a process wins, it transitions to the leader state and starts sending heartbeats to others.
  - Another process wins: if the candidate receives a heartbeart from a process that claims to be the new leader greater than or equal to the candidate's term, then it accepts the new leader and returns to the follower state. If not, it returns to the candidate'state since other processes may have won if the current process was stopped (e.g. garbage collection pause).
  - No winner for a period of time: split vote is when multiple followers become candidates at the same time, and none manages to win. Election timeout is chosen randomly to reduce the likelihood of another split vote.

### 9.2 Practical considerations
- Unless a solution with zero external dependencies is needed, we don't typically implement leader-election algorithms from scratch.
- Any fault-tolerant key-value store that offers linearizable compare-and-swap operation with an expiration time (TTL) could be used to implement a leader-election algorithm.
- compare-and-swap operation atomically updates the value of a key if and only if the process that tries to update the value correctly identifies the current value.
- When a process gets to write to a file, it might no longer hold the lease due to delays from system pauses. The process could try compare lease expiration time to its local clock assuming clocks are synchronized (though not perfect due to inaccuracy).
  - To provide extra safety, a version number can be assigned to each file that is incremented every time it is updated. The process with the lease can read the file and its version number from the file store, and can only perform updates if the version number have not been changed.
- A leader is a single point of failure with a large blast radius. The entire system can be brought down if the election process stops working or if the leader is faulty.
- Rule of thumb: minimize the work a leader needs to perform and be prepared that there may be more than one leaders, if we must have a leader.