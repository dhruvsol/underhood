# XDP Programs

xdp programs all are same and work on limited set of actions that are predefined.

## Table of Contents

- [Program Characteristics](#program-characteristics)
- [Flow of Execution](#flow-of-execution)
- [XDP Return Codes / Actions](#xdp-return-codes--actions)
- [XDP Execution Modes](#xdp-execution-modes)

## Program Characteristics

- program gets attach to the NIC of machine and can process incoming packets from NIC only.
- program is visible only to incoming packets
- kernel defines a struct called `xdp_md` which has info on the packet metadata like -> size, memeory location, the struct is light weight

## Flow of Execution

- Receive packet from NIC
- Do processing on the packet
- Determine the fate( pass it, drop it, retransmit it)

## XDP Return Codes / Actions

- **XDP_PASS** --> tells the packet can be pass to networking layer
- **XDP_DROP** --> tells the packet should be dropped
- **XDP_TX** --> tells the packet should be transmitted back to the network, this also bypass the networking stack
- **XDP_REDIRECT** --> tells the packet should be redirected to another NIC
- **XDP_ABORTED** --> this code triggers when xdp program is aborted because of error.

## XDP Execution Modes

### Offloaded to NIC

some NIC has option to offload the program to NIC itself, this is preffered mode of operation when avaible it allows XDP programs to run super fast

### Generic

Generic execution mode is used when NIC does not support offloading XDP programs. In this mode, the XDP program runs in the kernel space, which can be slower than offloaded mode
