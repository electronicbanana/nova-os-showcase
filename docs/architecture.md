# Nova platform architecture

Nova is a small virtual computing platform built around a custom language and instruction set. Its toolchain is organized as a sequence of transformations:

```text
Nova source → compiler → intermediate representation → assembler → executable → virtual machine
```

The compiler and assembler are separate C++ programs. The VM is a host-side C++ implementation, while core operating-system services and system programs are written in Nova. This separation makes the project a useful place to explore how a language toolchain connects to a machine model and the software that runs on it.

## Desktop environment

NOVOS includes the Nova Desktop Environment (NDE), which provides the desktop shell, app launchers, and user-facing configuration. Launcher entries identify an app and its icon; NDE starts the selected app. Desktop settings, such as colors and the wallpaper, are read from configuration files and can be changed with desktop tools.

The compositor is a separate Nova system service. It coordinates application windows and desktop presentation, including window placement, drawing, and updates. Apps use the graphics library to request windows and submit drawing work, while the compositor keeps the desktop and its windows presented together.

## Device interface

The device path spans Nova and C++. Nova kernel code communicates with virtual devices through memory-mapped I/O (MMIO) registers. NVM implements those registers in its host-side C++ code and connects them to device models and host facilities for display, graphics, storage, audio, input, and networking.

This means device support involves both languages at different layers: Nova code makes requests through the OS and MMIO interface, and C++ code in the VM provides the corresponding virtual device behavior. The C++ device models are part of NVM, rather than drivers compiled into the Nova kernel.

The repository also contains Nova editor support and sample applications. This showcase describes the design at a high level; it does not include the private implementation or generated program files.
