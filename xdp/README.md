# XDP (eXpress Data Path)

xdp is a networking feature that helps with creating high performance packet processing applications. this allows to create programs that can process packets superfast at kernel level.

## Table of Contents

- [Linux Network Stack](#linux-network-stack)
  - [Traditional Packet Processing Flow](#traditional-packet-processing-flow)
  - [Performance Issues](#performance-issues)
- [Packet Flow with XDP](#packet-flow-with-xdp)
  - [How XDP Works](#how-xdp-works)
  - [Key Advantage](#key-advantage)
- [Next Steps](#next-steps)

## Linux Network Stack

kernel stack is designed to be flexible and support wide range of networking protocols. but this comes with alot of overhead, a packet has to go through multiple layers of processing packets.

### Traditional Packet Processing Flow

packet from the NIC directly comes and gets stored in the kernel ring buffer.
this is called `rx_ring`. the packet gets copied to an instance of data structure called `sk_buff` (socket buffer) in the kernel network stack. this just has raw packet data.
sk_buff has features allowing to parse headers & checksumming & fragmentation
after sk_buff the packet is then forwarded to packet handler and it moves up in the network stack.
sk_buff has metadata on the packet such as where its going and what protocol it is.

**Flow:** NIC -> rx_ring -> sk_buff --> protocol handler

### Performance Issues

The problem with sk_buff is that its slow and copying each packet to the structure and everything makes it costly as well. This process can affect the performace of traditional processing solutions like netfilter hooks.

to improve performace idea is to process the packet before it reaches sk_buff.

![xdp-1](https://imgix.datadoghq.com/img/blog/xdp-intro/xdp-1.png?auto=compress%2Cformat&cs=origin&lossless=true&fit=max&q=75&w=1026&h=&dpr=1)

## Packet Flow with XDP

### How XDP Works

xdp program is an ebpf based program that can run as a hook point in NIC driver. it can easily handle the packet and decide the fate of the packet, even before it reaches the network stack.

### Key Advantage

so xdp program works on the `rx_ring` before it gets copied to `sk_buff`. this allows to process the packet at the NIC level itself.

## Next Steps

continue reading

1. [XDP programs](./xdp-program.md)
