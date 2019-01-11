# pthread-barrier-macos
Pthread-barrier implementation for macOS using a pthread mutex and a condition variable.

## Use
The implementation consists of 1 header and 1 source file and can be included directly in other projects. Additionally, the project contains a CMake file to build as a library.

The sources files use `#if` guards to hide their contents if not used on macOS. Therefore, it is possible to `#include "pthread_barrier.h"` on all systems which just results in `#include <pthread.h>` when not on macOS.

## Barriers among Processes
In this implementation, barriers among processes (in shared memory) work only if they are at the same address in all processes. This works out-of-the box for scenarios where the processes are forked from a common parent but requires specifying a dedicated address if `shm_open` is used.

For barriers among processes call `pthread_barrierattr_setpshared` with `PTHREAD_PROCESS_SHARED` (as described in the pthread specification).

The implementation uses the corresponding attributes for the mutex and condition variable. However, there seems to be a macOS limitation that requires the mutex and/or the condition variable to be at the same address in all processes as described above.

## Undefined Behavior
The barrier implementation imposes the usage restrictions as described by the Posix standard. Most importantly calling `pthread_barrier_destroy` while any thread is waiting on the barrier results in undefined behavior.

## Fairness
While it is possible to use the barrier with more threads/processes than specified by the `count` parameter, the implementation does not attempt to provide any guarantees regarding to fairness.

## License
[MIT](LICENSE)
