# Cooperative Thread Library

This project implements a cooperative user-level threading library in C++, built on top of `ucontext.h`. It provides lightweight thread management, mutual exclusion via mutexes, and coordination between threads using condition variables.

The library is designed to simulate multithreaded execution without relying on kernel threads, giving fine-grained control over scheduling, synchronization, and context switching.

## Thread Management (`thread.h`)

This header defines the core thread abstraction, along with the interface available to user applications.

**Key features:**
- **Thread creation:** Threads are created with a user-defined function and argument. Each thread gets its own stack and execution context.
- **Join support:** A thread can call `join()` to wait for another thread to finish before continuing.
- **Voluntary context switching:** `thread::yield()` allows a thread to yield control, giving other threads a chance to run.
- **Resource management:** The thread constructor and destructor manage stack allocation and deallocation.
- **Thread-local IDs and state tracking:** Includes facilities to track thread IDs and join queues.

## CPU Simulation (`cpu.h`)

This header defines the interface to the simulated CPU abstraction. The infrastructure bootstraps multiple CPUs and delivers timer and inter-processor interrupts to drive scheduling.

**Key features:**
- **Boot process:** `cpu::boot()` starts the system with one or more CPUs and begins execution of the initial thread.
- **Interrupt handling:** Includes functions for enabling/disabling interrupts, suspending CPUs, and handling timer/IPI interrupts.
- **Preemption model:** Interrupts can be delivered synchronously or asynchronously based on the configuration.
- **Ready queue & scheduling:** Maintains a shared ready queue and supports selecting the next thread to run.
- **Thread wrapper logic:** Handles the lifecycle of a thread, including cleanup of terminated threads.

## Mutual Exclusion (`mutex.h`)

The `mutex` class provides basic locking mechanisms to ensure safe access to shared resources between threads.

**Key features:**
- **Lock and unlock interface:** A thread can acquire a lock using `lock()` and release it with `unlock()`.
- **Ownership tracking:** Only the owning thread may release a lock.
- **Blocking semantics:** Threads attempting to acquire a held lock will be placed in a blocking queue until the mutex becomes available.
- **Move-only semantics:** Copying is disabled to prevent unintended behavior, but move support is optional.

## Condition Variables (`cv.h`)

The `cv` class provides condition variable support to allow threads to wait and be notified of changes in shared state.

**Key features:**
- **Wait interface:** A thread can call `wait(mutex&)` to atomically release the lock and suspend execution until notified.
- **Signal and broadcast:** `signal()` wakes a single waiting thread, while `broadcast()` wakes all waiting threads.
- **Blocking queue:** Internally maintains a queue of threads waiting on the condition.

Please watch the video for a demonstration of these features.

_Please note that the source code is kept private, but can be shared if requested_
