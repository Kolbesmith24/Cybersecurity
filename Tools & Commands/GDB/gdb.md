# GDB - GNU Debugger Reference Guide

**GDB** (GNU Debugger) is a powerful debugging tool for C, C++, and other languages. It helps developers and security professionals examine the execution of programs, inspect variables, analyze program crashes, and trace memory issues. GDB can be used to analyze both running programs and post-crash states.

---

## General Overview

GDB allows you to:
1. **Run** programs under the debugger.
2. **Pause** and **step through** execution line by line.
3. **Inspect variables**, memory, and registers.
4. **Modify values** during runtime.
5. **Analyze program crashes** through core dumps or live debugging.

GDB is essential for developers and penetration testers who want to analyze the behavior of programs and exploit vulnerabilities in compiled applications.

---

## Starting GDB

To start GDB with an executable, use the following command:
```
gdb <executable_file>
```

You can also provide the name of a **core dump file** to debug a crashed program:
```
gdb <executable_file> <core_file>
```

This command starts GDB with the executable and opens the core dump file for post-mortem analysis.

---

## GDB Commands Overview

### Basic Commands
- **run** or **r**: Start the program under GDB.
  - Example: `run arg1 arg2`
  
- **quit** or **q**: Exit GDB.
  
- **break** or **b**: Set a breakpoint at a specified location (line number or function).
  - Example: `break main`
  
- **continue** or **c**: Resume execution until the next breakpoint.
  
- **next** or **n**: Step to the next line of code, skipping over function calls.
  
- **step** or **s**: Step into the function call, allowing you to debug inside the function.
  
- **finish**: Continue running the program until the current function returns.
  
- **print** or **p**: Print the value of a variable or expression.
  - Example: `print my_variable`
  
- **list** or **l**: Show the source code around the current line.
  
- **info locals**: Show the values of local variables in the current function.
  
- **info args**: Show the arguments passed to the current function.
  
- **info registers**: Display the current values of CPU registers.
  
- **backtrace** or **bt**: Print a stack trace to show the function call history.
  
- **info stack**: Display the current stack trace.

### Debugging Crashes
- **core**: Examine the core dump file to debug the crashed program.
  
- **catch**: Catch exceptions, such as signals or system calls, for specific actions.
  - Example: `catch syscall`
  
- **watch**: Set a watchpoint to pause execution when a variable is modified.
  - Example: `watch my_variable`
  
- **signal**: Send a specific signal to a program running under GDB (e.g., `SIGKILL` or `SIGSEGV`).
  - Example: `signal SIGSEGV`

### Modifying Program State
- **set variable**: Modify a variable's value during runtime.
  - Example: `set variable my_variable = 10`
  
- **set args**: Set the arguments for the program being debugged.
  - Example: `set args arg1 arg2`

- **set register**: Modify a register value.
  - Example: `set $eax = 0x1234`
  
- **set environment**: Set environment variables for the program being debugged.
  - Example: `set environment VAR=value`

### Examining Memory
- **x**: Examine memory at a specific address.
  - Example: `x/10xw $esp` (Examines 10 words in hexadecimal at the ESP register)
  
- **dump**: Dump memory to a file.
  - Example: `dump memory memory_dump.bin 0x1000 0x2000`
  
---

## Running Programs with GDB

To run a program under GDB and provide arguments, use the following syntax:
```
gdb ./my_program (gdb) run arg1 arg2
```

Where `arg1` and `arg2` are arguments passed to the program.

### Handling Segmentation Faults
When a program crashes (e.g., due to a segmentation fault), GDB will provide an error message. You can then use commands such as `backtrace` (`bt`) or `info locals` to identify the root cause.

Example of a segmentation fault analysis:
```
(gdb) run Starting program: /path/to/my_program

Program received signal SIGSEGV, Segmentation fault. 0x080484f5 in main () at my_program.c:10 10 printf("%s\n", my_str); (gdb) backtrace #0 0x080484f5 in main () at my_program.c:10 (gdb) info locals my_str = (char *) 0x0
```

---

## Action Reference List

Below is a comprehensive list of common GDB actions and commands:

### Starting GDB
- **gdb <executable_file>**: Start GDB with an executable.
- **gdb <executable_file> <core_file>**: Start GDB with a core dump file.

### Breakpoints and Execution Control
- **break <location>**: Set a breakpoint at a specified location (line number or function name).
- **run**: Start the program under GDB.
- **continue**: Resume execution until the next breakpoint.
- **next**: Step over the current line of code.
- **step**: Step into the current function call.
- **finish**: Continue execution until the current function returns.
- **quit**: Exit GDB.

### Inspecting Program State
- **print <expression>**: Print the value of an expression or variable.
- **info locals**: Show local variables in the current function.
- **info args**: Show the arguments passed to the current function.
- **info registers**: Display the contents of CPU registers.
- **backtrace**: Show the stack trace of the program.
- **info stack**: Display stack information.

### Modifying Program State
- **set variable <var> = <value>**: Set a variable's value during runtime.
- **set args <arg1> <arg2>**: Set program arguments.
- **set register <reg> = <value>**: Modify the value of a CPU register.
- **set environment <VAR=value>**: Set environment variables for the program.

### Memory Inspection
- **x/<format> <address>**: Examine memory at a given address.
  - Example: `x/16xw $esp` (Examines 16 words at the ESP register in hexadecimal).
- **dump memory <file> <start_address> <end_address>**: Dump memory to a file.

### Handling Crashes and Signals
- **catch <event>**: Catch events like system calls or signals.
  - Example: `catch signal SIGSEGV` to catch segmentation faults.
- **watch <var>**: Set a watchpoint to stop execution when a variable is modified.
  
### Advanced Commands
- **core**: Load and analyze a core dump file.
- **signal <signal_name>**: Send a signal to the program (e.g., `SIGKILL`).
- **info threads**: List all threads in a multi-threaded program.
- **thread <thread_id>**: Switch to a different thread.

---




