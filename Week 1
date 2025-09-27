# Day 1 - Introduction to Verilog RTL Design and Synthesis
   # Day 1.1 - Introduction to open source simulator iverilog 
Simulator : Tool for checking design(RTL)
Iverilog tool we will use in this course
Design is actual Verilog code which has intended functionality to meet with required specifications
testbench : apply stimulus (testvector) to design to check functionality 
     # how does a simulator works:
It looks for changes in input signal upon change in input the output is evaluated if no change in input no change in output.
Note : It only looks for change in input
 # Day 1.2 - Labs using iverilog and gtkwave
Design and testbench file goes into iverilog and gives us vcd file (Value change dump format) which converts to gtkwave and gives us behavorial simulation.

 # Day 1.3 Introduction to Yosys and synthesis 
Synthesizer : Tool for converting RTL to netlist 
Yosys: synthesizer in this course
netlist is representation of design in form of cells present in the .lib file

Steps to verify the synthesis are that the output  of the synthesis should be same as RTL simulation. 
The set of primary input and output will remain same between RTL design and synthesised netlist so same test bench can be used 

RTL design : Behaviorial representation of required specification 
Synthesis :
RTL to gate level translation 
design is converted into gates and conection is between gates 
given out file is netlist 


.lib is collection of logical modules 
includes basic logic gates 
different flavours of same gates :
     2 input AND gate(Slow, Medium ,Fast)
     3 input AND gate(Slow, Medium ,Fast)
     4 input AND gate(Slow, Medium ,Fast)
Why we need different flavours of same gate :
Combinational delay in logic path determines the maximum speed of operation of digital logic circuit 
Tclk > Tcq_A + Tcombi + Tsetup_B
where Tcq_A:Proportional delay of Flip Flop A
Tclk :Proportional delay for 1 clock cycle 
Tsetup_B:Proportional delay for Setting up Flip Flop B
Tcombi :Proportional delay inbetween Flip flop A and B
So cells work fast to make Tcombi small
Fclk max = 1/ Tclk min

To ensure that we donot have hold issues at Flip flop B we need cells to work slow
hence we need cells that work fast to meet the required performance and cells that work slow to meet Hold.
This collection forms .lib.

Load on digital logic circuit is called capacitance.
Faster charging and discharging means lesser the cell delay. to cgarge and discharge we need transistors capable of sourcing more current 
so wide transistors means low delay means more area and power.
Norrow transistors means more delay means less area and power.

We need to guide synthesizer to select flavour of cells that is optimum for implementation of logic circuit.
More use of faster cells means bad circuit as less power and area.
use of slow cells gives a sluggish circuit that may not meet up with the required performance.







Introduction to timing .lib 
Day 2.1 : Introduction to .lib
Name of the library is "sky130_fd_sc_hd_tt_025C_1v80", the information we get from decoding this name about this library 
It is 130 nm library.
tt= Typical (can be slow , fast or typical)
025C: temperature 
1v: Voltage

P V T in library name stands for :
P: Process (Variations due to fabrication)
V : Voltage (Variations due to voltage)
T: Temperature (Variations due to temperature , silicon is sensitive to temperature)
These are essential for a design to work
They determine how silicon will work fast or slow.
The same circuit with different variations should work in all conditions.
This library is using cmos technology.
Delay model is table_lookup
it specifies unit of :
      time: nanosecond 
      voltage:volt
      power: Nanowolt
      current: miliampere
      resistance : kiloohm
      capacitance : picofarad 
.lib contains alot of cells that are defined as "sky130_fd_sc_hd_a2111o_1": which stands for 2 input AND into first input of 4 input OR. 
It has 32 combinations and values of each pin are also defined like capacitance, clock,direction,internal power.
we can open the verilog model for the same .
The area increases with the increaseing name numbering , as no. of transistors incresases. 

large cell means that that cell is employing wider transisator that will be faster but area and power will be more, narrow cells power and area will be less.

Day 2.2 : Hierarchical vs Flat synthesis
Flat Synthesis: treating the entire design as one block, without dividing it into smaller parts.
this have better logic optimization 
it result in small area and faster performance 
This is not suitable for big designs as debugging is difficult 
Hierarchical Synthesis: design divided into multiple modules written in RTL.
It preserves module boundraries . every module is synthesised individually.
Better for large and complex designs 
Uses larger area and slower performance as compared to flat synthesis.

Multiple module structure description :

Sub module 2 is OR gate 
Sub module 1 is AND gate 
3 inputs a,b,c and output is Y 
AND gate : input a,b 
OR gate: input c, output of AND gate
Y : output of OR gate 
Using NAND gate we get stacked NMOS  which is good.
Where as using NOR INVERTOR we get stacked PMOS which is bad as the mobility is poor and to improve this we need to create a wide cell.
In this cell defining code the hericarchy is preserved.

Why sub module level is synthesised ?
It means synthesising smaller part of a design instead of whole top level design at once.
It is done when we have multiple instances of same module. 
It uses divide and conquer statergy.


Day 2.3 : Various Flop coding Styles and Optimization 
Why flip flop coding styles
Combinational circuits do not remember anything.
We want a circuit that can store data, react over time, or synchronize with a clock, we need flip-flops (flops).
Flip Flop: basic memory element which can store only 1 bit 0 or 1.
It remembers value and updates the stored value on clock edge hence called clock controlled memory.
common type of clock is D flip flop.
We have 4 types :
1) Flip-Flop with Asynchronous Reset:
Output is reset to 0 immediately when the reset signal is high.
Does NOT wait for the clock.
Commonly used for resetting all registers at power-on.
code:
module dff_async_reset (
    input wire clk,
    input wire reset,  // asynchronous reset
    input wire d,
    output reg q
);
    always @(posedge clk or posedge reset) begin
        if (reset)
            q <= 1'b0;
        else
            q <= d;
    end
endmodule

2) Flip-Flop with Asynchronous Set: 
Output is set to 1 immediately when the set signal is high.
Does NOT wait for the clock.
Used when a signal must quickly go high regardless of the clock.
Code:
module dff_async_set (
    input wire clk,
    input wire set,    // asynchronous set
    input wire d,
    output reg q
);
    always @(posedge clk or posedge set) begin
        if (set)
            q <= 1'b1;
        else
            q <= d;
    end
endmodule

3) Flip-Flop with Synchronous Set:
Output is set to 1, but only on clock edge when set = 1.
Ignores set signal outside of the clock edge.
Used in state machines or controlled data latching.
Code:
module dff_sync_set (
    input wire clk,
    input wire set,    // synchronous set
    input wire d,
    output reg q
);
    always @(posedge clk) begin
        if (set)
            q <= 1'b1;
        else
            q <= d;
    end
endmodule

4) Flip-Flop with Synchronous Reset:
Output is reset to 0, but only on clock edge when reset = 1.
More timing-controlled reset behavior.
Common in pipelining and data path control.
Code:
module dff_sync_reset (
    input wire clk,
    input wire reset,   // synchronous reset
    input wire d,
    output reg q
);
    always @(posedge clk) begin
        if (reset)
            q <= 1'b0;
        else
            q <= d;
    end
endmodule

Asynchronus : Acts instantly, doesn't wait for clock.
Synchronous : Acts only with the clock edge.

Flop synthesis 
We observe different patterns in waves in Flop asynchronus set,Flop asynchronus reset, Flop synchronus reset,Flop synchronus set.
We synthesis circuit of all 4 flip flop(Flop asynchronus set,Flop asynchronus reset, Flop synchronus reset,Flop synchronus set) in yosys and get a logic diagram of the same.

Interesting optimization 
A with 3 bit input into mux Y = 2* A gives output Y 4 bit input
we tried to code and get the same logic diagram which we were sucessful at.

we take 2 bit number and y is a 6 bit number. 
a*9 =y 
a*[8+1] =y 
8a + a = y 
we get output as "aa" and "9".






Combinational and sequential optimizations
Day 3.1 : Introduction to optimizations
Digital logic as combinational and sequential logic 
we study combinational logic optimization 
to squeez the logic to get most optimized design for area and power saving 
constant propagation for direct optimization 
eg: Y =((AB)+C)' and A=0
we get ((0)+c)' =(C)'  
AND and OR gate in this operation give us output of NOT gate and we get an invertor 
boolean logic optimization 
Eg: assign y = a?(b?c:(c?a:0)):(!c)
we reduced and get y = a'c'+ac = a xor c 
Now we study sequential logic optimization 
sequential constant propagation :Replaces flip-flops with constant outputs by their constant values to simplify logic.
state optimization :Minimizes FSM by removing unreachable or merging equivalent states to reduce complexity.
retiming :Moves flip-flops across logic gates to balance timing and improve performance.
sequential logic cloning :Duplicates flip-flops and logic to reduce fanout and enhance timing on critical paths.

Day 3.2 : Combinational Logic optimizations 
opt_check is the modeule we will use with input a, b and output y.
y = a?b:0
y= ab AND gate
opt_check_2 has y = a+b OR gate 
we verify this via labs.
We did NAND realization of OR gate.
circuit :Inverter and inverter followed by NAND
We create a 3 input AND gate 
we learned to get optimized design.
Day 3.3 : Sequential Logic optimizations 
dff_const_1: we have a flip flop which gets Q on reset else 0.
We draw the wave form to check later we will also stimulate this design.
we replace flip-flops with constant outputs by their constant values to reduce logic.
Simplify FSMs by merging equivalent states.
Shifting flip-flops across logic to improve timing and reduce critical path delays.
duplicate flops and logic to reduce fanout and balance timing.
eliminate unused or duplicated registers to save area.
disables clock to idle flops to reduce dynamic power consumption.
adjusts the placement of registers to distribute logic delay evenly.
Adds registers between stages to increase throughput and improve timing.
chooses optimal state encoding for area/speed trade-offs.

Day 3.4 : Sequential optimizations for unused outputs 
In this step unused sequential outputs are detected and removed to optimize area and power in the design.

During synthesis, some outputs of sequential blocks might never be used in the design and are called unused outputs or dead outputs.
They increase power consumption and occupy space. It complicates design without benifit.

we detect flops whose output are not connected then remove those flops this simplifies the design anf reduces size and power. 












