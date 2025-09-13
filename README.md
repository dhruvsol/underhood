# Underhood

A repository containing notes and code that explore what happens beneath user space.


### eBPF, XDP, Kernel, and User Space Diagram

     +------------------------------------------------------------+
     |                        USER SPACE                          |
     |                                                            |
     |  +-------------------+    +-----------------------------+  |
     |  | Apps (nginx,curl) |    | libbpf / bpftool / loaders  |  |
     |  +---------+---------+    +--------------+--------------+  |
     |            |                               |               |
     |        syscalls (send,recv, bpf())         |               |
     +------------|-------------------------------|---------------+
                  v                               v
     +------------------------------------------------------------+
     |                        KERNEL SPACE                        |
     |                                                            |
     |  Network RX path:                                          |
     |                                                            |
     |   +-------------------+                                    |
     |   | XDP (eBPF)        |  <-- runs at NIC driver (early)    |
     |   | drop/redirect/pass|                                    |
     |   +---------+---------+                                    |
     |             | (pass)                                       |
     |        [SKB allocated]                                     |
     |             v                                              |
     |   TC ingress (clsact eBPF)                                 |
     |             v                                              |
     |   Netfilter (iptables/nftables)                            |
     |             v                                              |
     |   L3/L4 stack (IP, TCP/UDP)                                |
     |             v                                              |
     |   Socket filters / sockmap eBPF                            |
     |             v                                              |
     |   User sockets  <-----------------------------------------+
     |                                                            |
     |  Tracing / Observability:                                  |
     |   - kprobes/kretprobes (kernel fns)                        |
     |   - tracepoints                                            |
     |   - uprobes (user binaries)                                |
     |                                                            |
     |  eBPF subsystem:                                           |
     |   - verifier, JIT compiler                                 |
     |   - maps (hash, array, LRU, per-CPU)                       |
     |   - ringbuf / perfbuf for events                           |
     |                                                            |
     |  Network TX path:                                          |
     |   User socket --> stack --> TC egress (eBPF) --> driver    |
     +------------------------------------------------------------+
