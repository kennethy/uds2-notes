## Communication

**Network Protocols**
- Application Layer
  - High-level communication protocols like HTTP or DNS.
- Transport Layer
  - Transmits data between processes using the Transmission-Control-Protocol (TCP) that is built on IP. Port numbers are used on both ends.
- Internet Layer
  - Routes packets between machines across the network. The Internet Protocol (IP) delivers packets on a best-effort basis. Routers operate at this layer and forward IP packets to the next router along the path to the final destination.
- Link Layer
  - Switches forward ethernet packets based on destination MAC address.

## Chapter 2. Reliable Links
- Addressing is handled by the IP protocol. IPV6 uses 128 bits for addresses. Routers use local routing table to determine where the packet should be sent to. The routing table maps the destination address to the address to the next router.
- Border-Control-Gateway (BCG) protocol is responsible for communicating the routing tables across the routers.
- TCP provides some guarantees on the data delivery. It implements safety mechanisms to ensure the network and the receivers are not overwhelmed.

### 2.1 Reliability

- A sequentially set of ordered segments (partitions of a byte stream) are transmitted over the network.
- The delivery of each segment must be acknowledged by the sender and the receiver. The integrity of the segment is also verified through checksums.

### 2.2 Connection Lifecycle
- A connection needs to be established before any data can be transmitted.
- A connection will be one of the following states (simplified version):
  - Opening State: when the connection is being established.
  - Established State: connection is established and data is being transmitted.
  - Closed state: the connection is closed.
- TCP uses three-way handshake for establishing a connection:
  - Sender randomly selects a sequence number X and sends the SYNC segment.
  - Receiver randomly selects a sequence number Y, increments X, and sends back a SYN/ACK segment.
  - The sender increments both X and Y, and replies an ACK segment and the first bytes of the data.
- Sequence numer is used to ensure order and there is no missing segments.
- Several round-trips are made, and thus the lower the round-trip-time (RTT) is, the faster a connection can be established.
- Connections will be closed to release resources. If we anticipate more data to be transmitted, the connection should remain open to avoid the cold-start-tax.
- The operating system manages the connection on both ends using a socket.
  - Closing a connection updates the socket to be in the waiting state (TIME_WAIT). This is to ensure delayed segments coming from the closed connection are not considered as part of a new connection.
  - Processes typically maintain a connection pool to prevent an overwhelming number of sockets in the waiting state.

### 2.3 Flow control
- TCP implements the flow control, a backoff mechanism that prevent the sender from overwhelming the receiver.
- The receiver uses a buffer to store segments that are not yet processed. The receiver also communicates the buffer size when acknowledging a segment. The sender would not send more data than the buffer can hold.

### 2.4 Congestion control
- The sender maintains a congestion window that specifies the number of outstanding segments that can be sent without acknowledgement from the receiver.
- A small congestion window may indicate the bandwidth of the network is not fully utilized.
- The size of the congestion window is set to system default when a new connection is established. It is increased exponentially (up to a limit) as the receiver acknowledges a delivery of a segment.
- Congestion window size may be decreased congestion avoidance: it occurs when the sender detects a missed acknowledgement through a timeout.
- Bandwidth = WinSize / RTT: bandwidth is a function of latency.
- The shorter the RTT, the better the network's bandwidth is utilized. Servers should be geographically closed to clients for this reason.

### 2.5 Custom Protocol
- The guarantees TCP provides come with a cost of lower bandwidth and higher latency.
- User Datagram Protocol (UDP) allows clients to send discrete packets with a limited size (datagram).
  - It does not offer sequencing, flow control, nor congestion control.
  - Often used in systems in which intermittent data loss is accpetable (like multi-player games in which delayed data should be considered obsolete).