# Linux Kernel Tracing Project

[Coroot Node Agent GitHub Repository](https://github.com/coroot/coroot-node-agent)

This project implements a Linux kernel tracing mechanism that monitors file-related system calls, specifically focusing on the `open` and `openat` calls. The tracing is designed to filter out events associated with specific file paths like `/proc/`, `/dev/`, and `/sys/`.

## Table of Contents

- [Introduction](#introduction)
- [Code Structure](#code-structure)
- [Macros](#macros)
- [Data Structures](#data-structures)
- [Code Explanation](#code-explanation)
- [How it Works](#how-it-works)
- [License](#license)

## Introduction

The project provides a detailed example of an eBPF (extended Berkeley Packet Filter) program embedded in the Linux kernel. This program traces file-related system calls and logs events into a `file_events` array for further analysis.

## Code Structure

The code is organized into several key components:

- `file_event` structure: Represents an event related to file operations.
- `file_events` array: A BPF map of type `PERF_EVENT_ARRAY` used to store file events.
- `open_file_info` hash map: A BPF map used to store information about open files.
- `trace_event_raw_sys_enter__stub` and `trace_event_raw_sys_exit__stub`: Structures representing the raw data of syscalls before and after execution.
- `trace_enter` and `trace_exit` functions: Handle the tracing logic for `open` and `openat` syscalls.

## Macros

### `TASK_COMM_LEN`

- **Purpose:** Specifies the maximum length of the process/task name in the Linux kernel.
- **Value:** `16`
- **Explanation:** In the Linux kernel, each process or task has a name associated with it, and `TASK_COMM_LEN` determines the maximum length of this name. The value `16` indicates that the maximum length is 16 characters.

### `CLONE_THREAD`

- **Purpose:** Specifies a flag used in the `clone` system call to indicate that the new process should be a thread.
- **Value:** `0x00010000`
- **Explanation:** In the Linux kernel, the `CLONE_THREAD` flag is used to indicate that the new process should be a thread rather than a separate process. This flag is often used in multithreading scenarios to create new threads within the same process.

## Data Structures

### `struct file_event`

- type: Represents the type of the file-related event.
- pid: Represents the process ID associated with the event.
- fd: Represents the file descriptor associated with the event.

```c
struct file_event {
    __u32 type;
    __u32 pid;
    __u64 fd;
};



```
