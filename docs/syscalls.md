# Nova OS system calls

This is a quick reference to the current user-visible syscall numbers, arguments, and results. It describes the interface only. The list follows the current NOVOS syscall registrations and may change over time.

Arguments are shown in call order. Pointer arguments refer to Nova user memory. Most calls return an integer value or status; the notes below describe the common result. A raw blocking call can return `SYSCALL_BLOCKED` (`4095`). Standard library wrappers wait and retry for calls that need it.

## Processes and identity

- **4 `FORK`** — No arguments. Child receives `0`; parent receives the child process ID.
- **18 `SET_FOREGROUND_PGID`** — Process group ID. Returns status.
- **19 `SET_SIGINT_MODE`** — Signal mode. Returns status.
- **20 `GETPID`** — No arguments. Returns the current process ID.
- **21 `GETPPID`** — No arguments. Returns the parent process ID.
- **24 `GETUID`** — No arguments. Returns the user ID.
- **25 `GETEUID`** — No arguments. Returns the effective user ID.
- **26 `SETUID`** — User ID. Returns status.
- **27 `AUTHENTICATE`** — Candidate text, byte count. Returns an authentication result code.
- **59 `EXECVE`** — Executable path, argument count, argument vector. Replaces the current program on success; returns an error if it fails.
- **60 `EXIT`** — Exit status. Does not return to the exiting program.
- **77 `SHUTDOWN`** — Reserved authorization values, delay in seconds. Returns status; accepted requests schedule shutdown.
- **94 `WAITPID`** — Process ID (or `WAIT_ANY_PID`), optional status pointer, options. Returns the child ID or exit status. A raw nonblocking call may return `SYSCALL_BLOCKED`.

## Files and directories

- **11 `OPEN_FILE`** — Path, flags, mode. Returns a file descriptor, or `0` on failure.
- **12 `READ_DIR`** — `ReadDirRequest*`. Returns the byte count; the request also receives byte count and status fields.
- **13 `READ_FILE`** — File descriptor, buffer, byte count. Returns bytes read or an error.
- **65 `CLOSE_FILE`** — File descriptor. Returns status.
- **66 `WRITE_FILE`** — File descriptor, buffer, byte count. Returns bytes written or an error.
- **67 `UNLINK_FILE`** — File path. Returns status.
- **68 `STAT_FILE`** — `StatRequest*`. Returns status; the request receives file metadata.
- **62 `CHDIR`** — Directory path. Returns status.
- **63 `MKDIR`** — Directory path. Returns status.
- **64 `RMDIR`** — Directory path. Returns status.

## Input and output

- **0 `READ`** — File descriptor, buffer, capacity. Returns bytes read, `0` at end of input, or an error.
- **1 `WRITE`** — File descriptor, buffer, byte count. Returns bytes written or an error.
- **3 `CONSOLE_WRITE`** — Text, row, column, text attributes. Returns console output status.
- **7 `CLEAR_WINDOW`** — No arguments. Returns status.
- **17 `CARRIAGE_RETURN`** — No arguments. Returns status.
- **22 `SET_CONSOLE_BUFFER`** — Buffer index. Returns status.
- **23 `COMPOSITOR_WAIT_EVENT`** — No arguments. Returns an event wait result.
- **82 `SET_DISPLAY_MODE`** — Display mode, buffer index. Returns status.
- **87 `KEY_DOWN`** — Key code. Returns key state.

## Sockets and pipes

- **40 `SEND`** — Socket descriptor, buffer, byte count. Returns send status.
- **41 `RECV`** — Socket descriptor, buffer, capacity. Returns bytes received or an error.
- **42 `SOCKET_OPEN`** — Protocol or local-socket operation, address or name. Returns a socket descriptor, or `0` on failure.
- **43 `SOCKET_CLOSE`** — Socket descriptor. Returns status.
- **44 `SOCKET_ACCEPT`** — Listening socket descriptor. Returns a connected socket descriptor, or an error.
- **91 `PIPE`** — Pointer to two descriptor slots. Returns status; success fills the read and write descriptors.
- **92 `MOVE_FD`** — Source descriptor, destination descriptor. Returns status.
- **93 `WAIT_ANY`** — Descriptor array, count, blocking flag. Returns a ready descriptor, `WAIT_ANY_NONE` (`4094`), or a status code.

## Memory, time, and devices

- **10 `BRK`** — Requested heap end address. Returns the new/current heap end, or failure.
- **16 `SLEEP`** — Duration in milliseconds. Returns status.
- **81 `RAND_INT`** — Minimum, maximum. Returns an integer in the requested range.
- **90 `IOCTL`** — File descriptor, request code, request-specific argument. Result depends on the request.
- **95 `GET_TIME`** — `Time*` output. Returns status; output receives seconds and milliseconds.

`ReadDirRequest` and `StatRequest` are declared in the NOVOS user interface headers. `Time` contains `seconds` and `milliseconds`. File flags, socket protocol values, wait options, and `ioctl` request codes are separate interface constants.

## Examples

The Nova programs in [`../examples/`](../examples/) demonstrate file operations, network requests, and graphics APIs that communicate with the compositor.
