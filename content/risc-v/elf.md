Title: Notes on ELF Files
Date: 2023-08-07
Tags: risc-v, gcc
Summary: It's my job to figure out how to compile programs for RISC-V, or else I'm gonna have to write a bunch of asm again

As a continuation of working with the RISC-V version of GCC, I want to make sure I understand more details about the nuances of ELF files, to make sure nothing goes wrong and we won't have any issues using memory mapped I/O in our processor. Especially since if I can get away with it, being eventually able to `printf` to an LED array would be pretty cool, but is dependent on learning how `printf` actually works in the background

## Sections of an ELF file

- ELF header
- `.text`: the machine code of the compiled program
- `.rodata`: Read only data such as the format strings in `printf` statements, and jump tables for switch statements
- `.data`: *Initialized* global and static C variables. Local C variables are maintained at run time on the stack and do *not* appear in either the `.data` or `.bss` sections
- `.bss`: *Uninitialized* global and static C variables, along wiht any global or static variables that are initialized to zero. This section occupies no actual space in the object file; it is merely a placeholder. Object file formats distinguish between initialized and unintialized variables for space efficiency: uninitialized variables do not have to occupy any actual disk space in the object file. At run time, these variables are allocated in memory with an initial value of zero
- `.symtab`: A *symbol table* with information about functions and global variables that are dfined and referenced in the program. Some programmers mistakenly believe that a program must be compiled with the `-g` option to get symbol table information. In fact, every relocatable object file has a symbol table in `.symtab` (unless the programmer has specifically removed it with the `strip` command). However, unlike the symbol table inside a compiler, the `.symtab` symbol table does not contain entries for local variables
- `.rel.text`
- `.rel.data`
- `.debug`
- `.line`
- `.strtab`
- Section header table