# tinyShell

## What it is

A freestanding shell for TinyOS, written in C and x86-64 assembly, with no libc. It reads
a program path, forks, and runs it. The syscall stubs are hand-written in Intel-syntax
GAS. A second, pure-assembly implementation lives in `pure-asm-implementation/`.

## Demo

<!-- TODO: screenshot or GIF -->
![tinyShell session](docs/demo.png)

## Stack

- C (`shell.c`) for the read-fork-exec loop.
- x86-64 assembly (`sys.S`) for the syscall stubs: `write`, `read`, `fork`, `execve`,
  `waitid`, `exit`.
- A custom linker script (`custom.ld`) that loads at `0x10000` and sets `main` as the
  entry point.
- GNU `gcc`, `as` and `ld`. No standard library is linked.

## Quickstart

```
./build.sh     # compiles and links into ./init
./init         # type a program path, for example /bin/ls
```

`build.sh` compiles with `-Os -fno-ident -fno-asynchronous-unwind-tables
-fno-stack-protector -fomit-frame-pointer`, then links with `custom.ld` and `--strip-all`
to keep the binary small.

## Results

The shell runs one loop:

1. Print the `# ` prompt.
2. Read a line into a 255-byte buffer.
3. Replace the trailing newline with a null byte.
4. `fork`, then `execve` the path in the child and `waitid` in the parent.

Known limits, visible in the source:

- No argument parsing. The whole line is the program path.
- No builtins and no `PATH` search. Give a full path, for example `/bin/ls`.
- A fixed 255-byte line buffer.

## License

Apache-2.0. See [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE).
