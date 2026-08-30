# tinyShell

A minimal shell for TinyOS, in C and x86 assembly, linked with a custom linker
script to keep the binary small. Includes a second, pure-assembly implementation.

## Build

```
./build.sh
```

## Run

```
./shell
```

## Notes

- `custom.ld` is the linker script; `sys.S` holds the syscall stubs.
- `pure-asm-implementation/shell.asm` is a standalone assembly-only version.
- Freestanding: no libc.
