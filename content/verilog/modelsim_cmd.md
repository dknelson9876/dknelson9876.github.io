---
Title: ModelSim Cheatsheet
Date: 2023-07-02
Tags: verilog, modelsim
slug: modelsim-cheatsheet
Summary: I'm trying to figure out how to use ModelSim as a standalone program for compiling and synthesizing verilog
---

In my search to learn how to use modelsim as a standalone program for my senior project (as opposed to being bundled into Quartus, as I was taught in a previous university course), I came across a random old pdf made by Mentor, that helped me put together the following commands:

## Commands run by the GUI

- Generating the project:
    - `vlib work`
    - `vmap work work`
- Compile verilog files:
    - `vlog -reportprogress 300 -work work [file]`
- Load the simulation:
    - `vsim work.test_counter`
- Add test signals to the wave window:
    - `add wave -position insertpoint sim:/test_counter/*`
- Start the sim:
    - `run` (for the default time)
    - `run 500` (for 500 ns)
    - `run -all` (until stopped)