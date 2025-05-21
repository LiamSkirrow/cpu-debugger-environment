# CPU Debugger Environment

This is a placeholder repo for a Python-based CPU debug env, that shall give a convenient way to interact with RTL processor cores.

## Overview
- The python script is run and accepts the `--core` flag to specify which core we want to instantiate and interact with.
  - Example: `soft-cpu-debugger.py --core riscv32b`
- It might be convenient to have the ability to pass in ASM code as well so that in one single command we can:
  - assemble the ASM code
  - instantiate a specific CPU core
  - load the assembled code (as a .hex or .bin format) into the DMEM
  - enable the CPU and let it exeute the code
  - this probably would all be orchestrated by a Makefile that would call the relevant tools/scripts to make the above all happen in the right order
- The CPU's top-level module should be instantiated within a debug wrapper that is itself instantiated in a testbench (CPP tb if using verilator, or SV tb otherwise...). The debug wrapper hooks into the CPU's register file, any internal flip flops of interest and somehow can access an arbitrary region of memory. The TB opens a Unix socket and takes commands from the main Python script that connects to the other end of the socket. The Python script tells the debug environment which mode to operate in: single-step, run to completion etc. The debug environment passes back data from the CPU to the Python script, that shall then display this data to the screen for the user to inspect.
- It might also be useful that when in single-step mode, the debug environment can peek/poke imem and dmem, this would be useful for preloading memory or modifying instructions on the fly. 
