# RISC‑V Reference SoC Tapeout Program

# Day 0 - Getting started with Digital VLSI SOC Design and Planning (Summary)

         # Application Testing and Chip Design Proces

Initial Compilation with GCC
GCC is a compiler used for compiling C programs on Linux.

First, we take an application written in C, compile it using GCC, and obtain the output, denoted as O0.

      # Chip Modeling

We write a model specification of the processor in C.

This model is verified to check if the specifications are correct, resulting in output O1.

We validate the correctness by ensuring O0 = O1.

    # RTL Architecture

A soft model of the hardware is written using RTL (Verilog).

We run the original application on this RTL-based hardware and get output O2.

Functional consistency is verified by ensuring O1 = O2.

    # SoC (System on Chip) Design

The design is divided into the processor and its peripherals.

ASIC Synthesis of the Processor: A gate-level netlist is generated.

ASIC Synthesis of Peripherals: These are divided into macros and analog IPs.

A SoC engineer integrates the processor, peripherals, and GPIOs.

Running the application at this stage produces output O3.

We verify functionality again: O3 = O2 = O1.

    # Microprocessor vs. Microcontroller

A microprocessor is like a CEO — very powerful, but needs support like memory, storage, and I/O to function. Used for complex tasks such as running operating systems or games.
Examples: Intel i5, AMD Ryzen

A microcontroller is like a self-employed worker — not as fast, but self-sufficient with built-in memory and I/O. Used for simple tasks like motor control or sensor reading.
Example: Arduino

    # RTL to GDS (RTL2GDS)

Floorplanning and placement are done at this stage.

Macros and analog IP libraries are "hardened."

The processor remains soft logic.

A GDSII file is generated, which then undergoes DRC (Design Rule Check) and LVS (Layout vs. Schematic) checks.

    # Tapeout and Tape-in

After successful verification, the design is ready for Tapeout and sent to the foundry.

Once fabricated, the chips are received — this is referred to as Tape-in.

    # Board Design and Final Testing

A board is designed for the chip along with necessary peripherals.

The original application is tested on the actual silicon, producing output O4.

Final verification ensures: O1 = O2 = O3 = O4.

    # Production Timeline

It typically takes around 4 months for the foundry to fabricate the chips.

After thorough testing, the chip is ready for deployment in the market.
# Day 0.1 - Tools Installation
Download : Oracle virtual machine
Tool check :
Yosys
Iverilog
gtkwave
OpenSTA 
ngspice
magic
OpenLANEsudo



