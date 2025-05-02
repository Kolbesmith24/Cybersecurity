# GDB Cheatsheet – Common Commands

## 🔹 Starting GDB
- `gdb <binary>` – Start GDB with executable
- `gdb <binary> <core>` – Start GDB with core dump

## 🔹 Running & Controlling Execution
- `run [args]` / `r` – Run the program with optional args
- `continue` / `c` – Resume execution
- `next` / `n` – Next line (step _over_ function calls)
- `step` / `s` – Step _into_ function calls
- `finish` – Run until current function returns
- `quit` / `q` – Exit GDB
- `set args <args>` – Set command-line arguments
    

## 🔹 Breakpoints & Watchpoints
- `break <loc>` / `b` – Set breakpoint at function or line
    - e.g., `b main`, `b 42`
- `delete` – Remove all breakpoints
- `watch <expr>` – Break when expression changes
- `catch <event>` – Break on event (e.g., `catch syscall`)
    

## 🔹 Inspecting Program State
- `print <expr>` / `p` – Print variable or expression
- `info locals` – Show local variables
- `info args` – Show function arguments
- `info registers` – Show CPU register values
- `backtrace` / `bt` – Show call stack
- `info stack` – Detailed stack info
- `list` / `l` – View source code
    

## 🔹 Modifying State
- `set variable x = 123` – Set variable value
- `set $eax = 0x1234` – Set register value
- `set environment VAR=value` – Set env variable

## 🔹 Memory & Registers
- `x/<N><fmt> <addr>` – Examine memory
    - e.g., `x/10xw $esp` (10 words, hex, at ESP)
- `dump memory <file> <start> <end>` – Dump memory range to file
- Formats:
    - `x` (hex), `d` (decimal), `s` (string), `i` (instruction), `c` (char)
        

## 🔹 Crash & Signal Handling
- `signal <sig>` – Send signal (e.g., `signal SIGSEGV`)
- `core <file>` – Load core dump
- `catch signal SIGSEGV` – Catch segmentation fault
    

## 🔹 Threads
- `info threads` – List threads
- `thread <id>` – Switch to thread ID
    
