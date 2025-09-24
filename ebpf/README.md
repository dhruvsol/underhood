# eBPF (eXtended Berkeley Packet Filter)

eBPF is a powerful technology that allows for programs to be executed at the operating system level and operating system guarantees execution & efficency.

## Table of Contents

- [Overview](#overview)
- [Development Workflow](#development-workflow)
- [Core Concepts](#core-concepts)
  - [Maps](#maps)
  - [Helper Calls](#helper-calls)
  - [Function Calls & Tail Calls](#function-calls--tail-calls)

## Overview

ebpf works on events, when a certain hook point happens they get trigged

with ebpf we can also attach custom events if there is no predefined hook point, using kprobe (kernel probe) and uprobe (user probe), these helps us to attach ebpf hooks anywhere in the system, kernel or user space.

## Development Workflow

- we write the program in C or any other framework ( aya in rust )
- loading & verification happens to check if the program is safe to execute
- JIT compilation happens to convert the program into machine code, this helps to improve performance and reduce overhead.

## Core Concepts

### Maps

eBPF programs has ability to share collected data using maps.

### Helper Calls

eBPF cannot do calls to kernel functions. they use helper functions to interact with the stable api offered by the kernel.

### Function Calls & Tail Calls

function calls -> allows with defineing and calling function within a eBPF program
tail calls --> allows with executing eBPF programs
