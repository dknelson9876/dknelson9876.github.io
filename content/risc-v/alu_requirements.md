Title: Notes on a RISC-V ALU
Date: 2023-07-03
Tags: verilog, risc-v
Summary: I need to write an ALU for my senior projet. But first I need to figure out the requirements.

As part of my senior project to build a RISC-V processor, I've been assigned/chosen to design the ALU. That means I'm going to need to figure out what a RISC-V ALU needs. My main source here is the latest RISC-V Instruction Set Manual (Unprivileged ISA), [link](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf), as of writing that's version 20191213.

-------

The arithmetic related instructions that exist in RV32E (our chosen ISA subset) are:

- `ADD`[I]/`SUB`
- `SLL`[I]/`SRL`[I]/`SRA`[I]
- `AND`[I]/`OR`[I]/`XOR`[I]
- `LUI`/`AUIPC`, `JAL`/`JALR`
- `BEQ`/`BNE`/`BLT`[U]/`BGT`[U]

As I'm focusing on the ALU, the immediate variants are irrelevant right now. The first three bullets are instructions that it is the sole responsibility of the ALU to perform some math, so those are relatively. I can create an internal `aluOp` based on the func codes in the instruction, and simply perform the operation.

The exception there is `SLT` and `SLTU`, as verilog will happily take care of unsigned comparisons, but we have to get a little more manual to perform a signed comparison using the MSb and the difference between the two values.

> Note: `rs1` is the left operator for arithmetic, which I'm also calling `A`