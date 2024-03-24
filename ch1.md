# Chapter 1. Introduction

Distributed system is a group of nodes communicating together over the network to achieve some task.

## Main Challenges
- Communication
  - How are the requests and responses represented over the wired?
  - How to ensure there are no intermediaries eavesdropping on the communication?
- Coordination
  - How nodes work in unison toward a shared objective?
- Scalability
  - How the system performs under heavy load?
  - How is performance measured?
    - Throughput: requests processed per second.
    - Response time: the time it takes to send a request and to get a response.
- Resiliency
  - How does the system continue to do its job even when failures happen.
    - Uptime/availability: amount of time the system can serve requests over the total time measured.
- Maintenance
  - The majority of time is spent on system maintenance work like adding new features, fixing bugs, and operating it.
  - System should be easy to maintain.

### Anatomy of a distributed system

- Runtime perspective
  - A group of software processes communicate via inter-process-communication (IPC).
- Implementation perspective
  - A group of loosely-coupled components (servers) that communicate via APIs.
- Ports and Adapters Architecture
  - Inbound adapter: service provides interface via application-programming-interface (API).
  - Outbound adapter: grant access to external services like a datastore

### Availability %
| % | Downtime per day |
|---|------------------|
| 90% | ~2.4 hrs |
| 99% | ~15 mins |
| 99.9% | 1.5 mins |
| 99.99% | 8.64 secs |
| 99.999% | 864 millisecs |