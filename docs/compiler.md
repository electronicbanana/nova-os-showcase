# Compiler and toolchain

Nova source passes through a compiler, an intermediate representation (IR), and a dedicated assembler before it becomes a VM executable. The compiler is divided into front-end, optimization, and code-generation components. The assembler turns Nova assembly instructions into the executable representation consumed by the VM.

This staged design makes each boundary visible: language constructs are lowered into IR, the optimizer processes that representation, code generation emits assembly, and assembly is encoded for execution. The public showcase will add a small end-to-end example after its source and output can be validated safely.

No private source, IR dump, assembly listing, or executable is included here.
