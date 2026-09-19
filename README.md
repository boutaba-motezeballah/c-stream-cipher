# c-stream-cipher

## Overview
This repository contains a simple stream cipher implementation written in 100% pure x86_64 Assembly (NASM) for Linux. The program encrypts or decrypts data by generating a pseudo-random key-stream based on a seed, and applying a bitwise XOR operation directly on the byte stream in memory. It also includes a secure memory purging loop to overwrite buffers after execution.

---

## How it Works
The program processes data block by block or byte by byte directly inside the CPU registers for maximum speed and control.

### Step-by-Step Flow:
1. **State Initialization:** Takes a symmetric key seed and sets up an internal state counter to generate a pseudo-random stream of bytes.
2. **Bitwise Shuffling:** Uses shifts, additions, and XOR operations on registers to morph the internal key state for every processed byte.
3. **XOR Transformation:** Applies a bitwise XOR between the raw input byte stream and the generated key-stream byte. This single operation serves both for encryption and decryption.
4. **Memory Sanitization:** Runs a secure-purge loop over the data buffers immediately after processing, overwriting the internal state with zeroes to prevent data recovery from RAM.

---

## Compilation and Build (Makefile)
The build process uses a minimal Makefile to compile the pure assembly file without linking any C libraries.

### Build and Run Instructions
```bash
# Compile and link the assembler code automatically
make

# Run the stream cipher binary
./stream_cipher

# Clean build artifacts
make clean
```

---

## Project Structure (Makefile Code)
This is the Makefile used to track and compile the source file natively:

```makefile
ASM=nasm
ASMFLAGS=-f elf64
LD=ld

all: stream_cipher

stream_cipher: main.o
	\$(LD) main.o -o stream_cipher

main.o: main.asm
	(ASM) (ASMFLAGS) main.asm -o main.o

clean:
	rm -f *.o stream_cipher
```
