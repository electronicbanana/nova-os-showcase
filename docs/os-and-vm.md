# Virtual machine and operating system

The Nova VM is implemented in C++ and models instruction execution along with virtual devices such as display, keyboard, audio, and storage. The operating-system kernel and system programs are written in Nova and use a syscall interface to request kernel services.

The OS source is organized into services for process and memory management, filesystem access, networking, and audio. System programs include startup, login, terminal, and desktop components. The VM and OS together provide the execution environment for Nova applications.

These descriptions summarize the repository structure and interfaces. Detailed behavior, test results, and performance figures will be added only when directly checked and measured.
