# Day 1
## Introduciton to verilog RTL Design and synthesis

## Design
It's the .v or .sv code which contains actual implementaion of Design

## TestBench
Ensures Design is Behaving as intended by applying stimulus.
Simulator looks for changes in input and output is evaluated accordingly
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/1.JPG?raw=true)

## Lab using i verilog and gtkwave
clone the repository 
[sky130](https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git)

It should contain these files
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/2.JPG?raw=true)
Screen shot of MUX Waveform using Iverilog
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/3.JPG?raw=true)

## Yosys
Yosys is an open source tool for synthesizing (generating netlist)
Netlist is representation of design in standard cells
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/4.JPG?raw=true)
To check if the geenrated netlist behaves like the design file we should take the netlist and testbench and simulate the waveform and it shoudl match with the original stimulus.

RTL Design is behavioral representation of required specification
## Synthesis
RTL to Gate level translation
.lib is collection of standard cells
We need different operating speeds of gates to accomodate setup times of circuits.
We need slow cells to ensure there's no hold time violations.
Faster cells require more power and area as its width is more and vice versa.
## yosys Lab
Install Yosys and read the library using the command below
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/5.JPG?raw=true)
run commands
yosys
read_liberty (libfile path with name)

read_verilog (verilog design file)

synth -top (design_verilog_file)

abc -liberty (library_path_with_name)

show
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/6.JPG?raw=true)
write_verilog good_mux_netlist.v
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/7.JPG?raw=true)
to see simple and small netlist use below
write_netlist -noattr good_mux_netlist.v
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/8.JPG?raw=true)

# Day 2

### Library file
It has information about area power and logical implementaion of standard cells which when used can be used to implement any verilog code
Multiple_modules.v code
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/9.JPG?raw=true)
This has one AND Gate and one OR Gate
Input of A and B is connected to AND Gate this output and C is connected to OR Gate output of this is final output

read_liberty -lib (lib file)
read_verilog Multiple_modules.v
synth -top Multiple_modules
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/10.JPG?raw=true)
schematic of Multiple_modules
abc -liberty (lib file)
show Multiple_modules
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/11.JPG?raw=true)
netlist of Multiple_modules
write_verilog -noattr multiple_modules_hier.v
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/12.JPG?raw=true)
We expected to see a NOR Gate but output was 2 inverters connected to NAND Gate (Demorgans theorem)(As in NOR Gate pmos are stacked and its bad as pmos has lower mobility than nmos)
flatten
generate Netlist
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/13.JPG?raw=true)
show
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/14.JPG?raw=true)
### Submodule Synthesis
synth -top sub_module1
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/15.JPG?raw=true)
sub_module1
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/16.JPG?raw=true)
We synthesize submodules when we have multiple instantiation of same module we will synthesize the submodule once and copy paste it multiple times
Divide and Conquer 
Breaking down big design into small and digestable pieces
## FLIP FLOPS
Asynchronous reset flip FLOPS
iverilog design.v tb.v
./a.out
gtkwave tb.vcd
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/17.JPG?raw=true)
Asynchronous set flip FLOPS
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/18.JPG?raw=true)
Synchronous Reset Flip FLOp
![](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/19.JPG?raw=true)
# Combinational and Sequential optimizations
### Constant Propagation
### Boolean Logic optimization
Synthesis tool does these types of optimization
## Sequential Logic Optimization
### Sequential Logic Propagation
##Sequential Constant Propagation
Identifying and propagating constant values through sequential elements like flip-flops or registers. It helps reduce the logic complexity of the circuit and improve performance. Here are two lines explaining sequential constant propagation
##Lab
after synth -top module name
opt_clean -purge
All optimization is design_verilog_file
abc -liberty (libfile)
![20](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/20.JPG?raw=true)
Output of opt_check.v
![21](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/21.JPG?raw=true)
Output of opt_check2.v
![22](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/22.JPG?raw=true)
NAND Realization of OR Gate 
![23](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/23.JPG?raw=true)
Multiple_modules opt

## Sequential Logic optimization
dff_const1
![24](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/24.JPG?raw=true)
Simulation
![25](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/25.JPG?raw=true)
Synthesis

dff_const2
![26](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/26.JPG?raw=true)
Simulation
![27](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/27.JPG?raw=true)
Synthesis

dff_const3
![28](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/28.JPG?raw=true)
Simulation
![29](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/29.JPG?raw=true)
Synthesis

dff_const4
![30](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/30.JPG?raw=true)
Simulation
![31](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/31.JPG?raw=true)
Synthesis

dff_const5
![32](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/32.JPG?raw=true)
Simulation
![33](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/33.JPG?raw=true)
Synthesis
## Sequential Optimisations for Unused Outputs
Counter_opt
![34](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/34.JPG?raw=true)
![35](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/35.JPG?raw=true)
counter_opt2
![36](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/36.JPG?raw=true)
![37](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/37.JPG?raw=true)

## GLS Synthesis-Simulation 
Certainly, here are the key points about gate-level simulation in three concise statements:

1. **Digital Circuit Representation**: Gate-level simulation models digital circuits using logic gates like AND, OR, and XOR, with a focus on interconnections and signal propagation.

2. **Event-Driven Timing Analysis**: It's event-driven, accounting for gate propagation delays and changes in input signals, essential for verifying circuit correctness and performance.

3. **Verification and Debugging**: Gate-level simulation aids in circuit testing, debugging, and performance analysis, using specialized EDA tools to ensure a design meets functional and timing requirements.
##Mismatch and Blocking Non-blocking Statements
Certainly, let's break down the concepts of synthesis, simulation, mismatch, and blocking/non-blocking statements in digital design:

1. **Synthesis**: Synthesis in digital design refers to the process of converting a high-level hardware description language (HDL) code, such as VHDL or Verilog, into a gate-level representation or netlist. This gate-level representation can be implemented in hardware using FPGA (Field-Programmable Gate Array) or ASIC (Application-Specific Integrated Circuit) technology.

2. **Simulation**: Simulation is the process of testing and verifying the functionality of a digital design before actual hardware implementation. It involves creating a model of the design and simulating its behavior using a software simulator. This helps in identifying and debugging issues in the design.

3. **Mismatch**: Mismatch in simulation refers to a situation where the behavior of the simulated model does not match the expected or desired behavior. It can occur due to errors in the design, incorrect simulation settings, or other factors. Mismatch often indicates a problem that needs to be resolved before moving to synthesis or hardware implementation.

4. **Blocking and Non-blocking Statements**: In HDLs like Verilog, there are two types of assignment statements used to describe how signals are updated:

   - **Blocking Assignment**: In a blocking assignment, the right-hand side (RHS) expression is computed immediately, and the result is assigned to the left-hand side (LHS) signal. It means that the RHS calculations are done sequentially, and the order of assignments matters.

   - **Non-blocking Assignment**: Non-blocking assignments are used to model concurrent behavior. The RHS expressions are evaluated simultaneously, and the results are stored in temporary storage (flip-flops) and then updated to the LHS signals in parallel. The order of non-blocking assignments does not affect the simulation results.

In summary, synthesis converts high-level HDL code into hardware, simulation tests and verifies the design's behavior, mismatch indicates inconsistencies in simulation results, and blocking/non-blocking statements in HDL code control how signal assignments are executed during simulation, affecting the simulation's concurrency and behavior representation.




### Gate Level Simulation Lab

ternary operator MUX
![38](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/38.JPG?raw=true)
Simulation 
![39](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/39.JPG?raw=true)
Synthesis
![40](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/40.JPG?raw=true)
Gate level Simulation
![41](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/41.JPG?raw=true)

BAD MUX
![42](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/42.JPG?raw=true)
Simulation
![43](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/43.JPG?raw=true)
Synthesis
![44](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/44.JPG?raw=true)
Gate level Simulation
![45](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/45.JPG?raw=true)
Synth Simulation mismatch for blocking Statements
![46](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/46.JPG?raw=true)
Simulation
![47](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/47.JPG?raw=true)
Synthesis
![48](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/48.JPG?raw=true)
Gate level Simulation
![49](https://github.com/Akshay1000101/riscvcourse/blob/main/screenshots2/49.JPG?raw=true)
