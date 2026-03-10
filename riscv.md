# RISC-V Development Setup & Usage (Ubuntu)

## 1. Install Everything (one-time)

```bash
sudo apt update
sudo apt install gcc-riscv64-linux-gnu \
                 binutils-riscv64-linux-gnu \
                 libc6-riscv64-cross \
                 qemu-user \
                 gdb-multiarch
```

---

## 2. Write a Program

```bash
nano hello.c
```

```c
#include <stdio.h>

int main() {
    int x = 5;
    printf("Hello, RISC-V! x=%d\n", x);
    return 0;
}
```

---

## 3. Compile (recommended: debug + static)

```bash
riscv64-linux-gnu-gcc hello.c -o hello -g -static
```

---

## 4. Run (emulated)

```bash
qemu-riscv64 ./hello
```

---

## 5. Debug (two terminals)

### Terminal A — start debug server

```bash
qemu-riscv64 -g 1234 ./hello
```

### Terminal B — debugger

```bash
gdb-multiarch hello
```

Inside gdb:

```
target remote :1234
break main
continue
next
print x
quit
```

---

## 6. Optional Tools

### Generate assembly

```bash
riscv64-linux-gnu-gcc -S hello.c
```

### Disassemble binary

```bash
riscv64-linux-gnu-objdump -d hello | less
```

---

## 7. Uninstall Everything (clean removal)

```bash
sudo apt purge gcc-riscv64-linux-gnu \
               binutils-riscv64-linux-gnu \
               libc6-riscv64-cross \
               qemu-user \
               gdb-multiarch
sudo apt autoremove --purge
sudo apt clean
```

---

## Workflow Summary

```
Write → Compile (-g -static) → Run (QEMU) → Debug (GDB)
```
