# sfal-vsd
# Day 0 - Tools Installation
## Yosys
```
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys 
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```
<img width="575" alt="yosys" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7dfb067d-5f8c-407b-86eb-6bcb44f60a97">

## Iverilog
```
$ sudo apt-get install iverilog
```
<img width="702" alt="iverilog" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e660c9fc-0d6d-4ab7-b75f-9992133771ef">

## GTKWave
```
$ sudo apt update
$ sudo apt install gtkwave
```
<img width="604" alt="gtkwave2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/843a73bc-20ec-4417-bdc8-883caa6a299b">

<img width="1008" alt="gtkwave1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1e0c0704-f9a8-4ce4-b364-55a1fb0fc9ca">


# Day 1 - Introduction to Verilog RTL Design and Synthesis
## Introduction to open-source simulator Iverilog

Folder structure of the git clone:
- `lib` - will contain sky130 standard cell library
- `my_lib/verilog_models` - will contain standard cell verilog model
- `verilog_files` -contains the lab experiments source files

<img width="762" alt="intro_iverilog" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ceceb871-47f9-4edc-8ec7-ac273dc5352c">


Example of a design good_mux.v 

```
module good_mux (input i0 , input i1 , input sel , output reg y);
always @ (*)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```
Example of a testbench tb_good_mux.v 

```
`timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule
```
Command to run the design and testbench
```
iverilog good_mux.v tb_good_mux.v
```
The output of the iverilog is a .vcd file and a.out file is created. By executing a.out iverilog dump the vcd file.

## Introduction to GTKWave
gtkwave will be used to generate the waveforms and display in visual format.

Command to view the vcd file in gtkwave 
```
gtkwave tb_good_mux.vcd
```
The waveform in gtwave is shown below

<img width="993" alt="lab1-gtkwave" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2f27e5f2-6c6c-491a-b6be-8c8974ed1303">

## Introduction to Yosys
It is the synthesizer used to convert RTL to netlist.
Netlist should be the same as the Design but represented in the form of standard cells.
The same testbench can be used to verify RTL and Synthesized Netlist.

<img width="800" alt="intro_yosys" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0920be6f-770d-447d-a2cf-eaf73280539e">

## Introduction to Logic Synthesis

<img width="611" alt="intro_logic_synthesis1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d01c7771-7bb7-42cd-b7a1-24472ca61226">

## Lab using Yosys and Sky130 PDKs
<img width="736" alt="yosyslab1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/81628bfa-c5b4-4715-bd30-ccc5dc97f789">

<img width="730" alt="yosyslab2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a62c4b4c-d1b0-4412-bfa5-11fcec22627a">

<img width="727" alt="yosyslab3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a96c3730-0071-49c2-b2f9-d61f9640ba20">

<img width="636" alt="show_logic" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/19d70533-9d4e-4cec-81c5-7ad2fafc381f">

# Day 2 - Timing libs, Hierarchical vs Flat Synthesis and Efficient Flop Coding Styles

## Introduction to timing .libs
Libraries are characterized based on PVT (process, voltage, temperature) \
Process -> Variations due to fabrication \
Voltage -> Variations due to voltage \
Temperature -> Variations due to temperature 

As seen in the screenshot below \
tt stands for typical in the .lib name \
025C stands for temperature of 25 C in the .lib name \
1v80 stands for voltage of 1.8V in the .lib name

<img width="903" alt="libertyfile1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4d70123b-e2a7-406b-9d19-9f7bfc958840">

-cell defines the beginning of the cell. Other information of cells mentioned are:
- Leakage power based on the combination of inputs
- Area
- Power ports
- Input capacitance
- Power associated with the pin
- Transition
- Delay

## Hierarchical vs Flat Synthesis

### Hierarchical Synthesis
Report after synthesizing multiple_modules.v. As shown below the sub_modules statistics are printed. For example, sub-module1 has 1 AND gate and sub-module2 has 1 OR gate. This is an example of Hierarchical Synthesis.

<img width="1284" alt="multiplemodules1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a3fe130b-84ba-4605-9bb5-b6ea0600162f">

Hierarchy is preserved. sub_module1 and sub_module2 are instantiated separately in the synthesized Verilog netlist. Rather than seeing AND or OR gate, we see sub_modules when we run the command 'show' as shown in the screenshot.

<img width="951" alt="mutiplemoduleshier2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/bfedb3bd-484b-4bec-b8a3-95636b0dadb2">

If we look into the sub_module2 in synthesized netlist 'multiple_modules_hier.v', we see that rather than OR gate, the inputs a & b, pass through the inverter and then NAND gate. It is because in CMOS, stacking PMOS, which happens in 'OR' gate is bad as PMOS has lower mobility and always have to be wider to get some meaningful output. The next step is to check .lib file for the answer.

### Flat Synthesis
The design can be flattened by using the command `flatten`.

Screenshot shows the command, synthesized netlist and the logical diagram.

<img width="1534" alt="multiplemodulesflat1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b9045858-4928-4503-9fdf-597848406a43">

### Sub-module Level Synthesis
RTL (Register Transfer Level) designs are often modular, with various functional blocks or sub-modules. Sub-module level synthesis allows each of these sub-modules to be synthesized independently.

Why is the sub-module level synthesis necessary?
- Optimization and Area Reduction: By synthesizing sub-modules separately, the synthesis tool can optimize each one individually. It performs logic optimization, technology mapping, and area minimization for each sub-module. This leads to more efficient use of resources and reduced overall chip area.
- Resuability: Each submodule can be designed, verified, and optimized independently. They can be reused in a large design multiple times saving time and enhancing efficiency. 
- Parallel Processing: Different sub-modules can be synthesized concurrently, improving efficiency. For large designs, parallel synthesis significantly reduces turnaround time.

The commands to run sub-module synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The screenshot shows that when sub_module1 is synthesized, only AND gate is generated. 
<img width="1003" alt="submodulesynth" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1e3c7a7e-7ae2-41af-89ce-27e3b7a198ec">

## Various Flop Coding Styles and Optimization

### Why do we need flops and how do they prevent glitches in the circuit?

Glitches can occur in digital circuits due to various reasons such as signal delays, noise, or timing issues. Flops prevent glitches during the operation in the following ways:
- Synchronization: Flops are edge-triggered devices, meaning they respond only to transitions of the input signal (e.g., rising edge, falling edge). This synchronization ensures that the output changes only at specific points, reducing the likelihood of glitches caused by transient signal variations.
- Timing Control: Flops are typically controlled by a clock signal, ensuring that all circuit operations occur synchronously. This eliminates timing issues that could lead to glitches due to data arriving at different times.

<img width="846" altthe ="flops1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/32aab966-261a-4e42-9f49-59572586cd0f">

### Different types of flops
To initialize flops, we need to `set` and `reset` which can be synchronous or asynchronous.

<img width="775" alt="syn_async_reset_flop1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/338b941f-4a51-4cf3-9289-f344afac2922">

<img width="967" alt="sync_async_reset_flop2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/22ef5caa-be1d-4cfe-be44-b1dc7083c377">

The screenshot below shows DFF with asynchronous reset HDL simulation in Iverilog and  waveform display in GTKwave. Irrespective of the clock and d, as soon as async_reset=1, q=0.

<img width="1011" alt="async_reset_flop_hdl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4f4c5e04-85c9-4492-90cd-00a6dddcbbb3">

### Synthesizing flops
The command to synthesize ***DFF with asynchronous reset*** as an example
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="1142" alt="dff_asyncres_syn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1beb6298-6b1c-4c6b-a2dd-ac28de40f108">


On synthesizing ***DFF with synchronous reset*** we get NOR gate with inverted `d` as shown in the screenshot below. However,on evaluating the boolean expression, we reached the same logic realization. 


<img width="1211" alt="dff_sync_reset1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/45f7fc3b-d87f-43bc-94fd-0d9f3870382f">

<img alt="dff_sync_reset2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/81c0b90f-06af-4ee9-9f10-b8f4f8f743a7" width="700" height="500">

Using the `stat` command, all the cells used for logic synthesis are visible even though it is not evident from the statistics of doing synthesis.

<img width="572" alt="dff_syncres2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/15ff7c0a-cec7-4aac-8385-a0b59f99ec04">



### Synthesizing mult2 (multiply by 2)

 
To implement `y[3:0] = 2*a[2:0]`, we append a `1'b0 `to the `a[2:0]` i.e, `y[3:0] = {a[2:0],0}`. This is also equal to left shift the input bits by 1.
This can be realized by just wiring.
So we expect no hardware which is also seen in the screenshot below, analysis after synthesis and show. The command 'abc' is not required for mapping when there are no cells.

<img width="972" alt="mult2_syn2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d2cd1b79-d13b-4478-83f3-e73f16a7fce9">

### Synthesizing mult9 (multiply by 9 or 8+1)

`y=9*a` can be considered `8*a+1*a`
To implement `y[5:0] = 9*a[2:0]`, we append `000` to `a[2:0]` and then add `a` i.e, `y[5:0] = {a[2:0],000} + a[2:0]`.
This can be realized just by wiring.
So we expect no hardware which is also seen in the screenshot below, analysis after synthesis and show. The command 'abc' is not required for mapping when there are no cells.

<img width="918" alt="mult8_syn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/dd1b4634-50ed-4229-9d82-80689bafb7e7">

# Day 3 - Combinational and Sequential Optimizations

## Introduction to Optimizations

### Combinational Logic Optimization
It means squeezing the logic to get the most optimized design in terms of area and power. the most commonly used techniques are:
1) Constant propagation using direct optimization
2) Boolean logic optimization using K-map and Quine McKlusky

An example of constant propagation optimization is highlighted below.
<img width="618" alt="1_const_prop" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c8bd1118-52f7-441b-8cff-254d851cb892">

An example of boolean optimization is highlighted below.

<img width="619" alt="2-boolean-opt" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/aa864102-ef33-4d45-9ec9-929738172cd4">

### Sequential Logic Optimization
The technqiues used are:
1) Basic
   - Sequential constant propagation
2) Advanced (not covered as part of lab)
   - Static optimization
   - Retiming
   - Sequential logic cloning (floorplan aware synthesis)

An example of sequential constant propagation is highlighted below of DFF with asynchronous reset where D input is grounded. To note, the same technique cannot be applied to DFF with the asynchronous set because while `Q=1` when `Set=1`, but `Q=0` at `Set=0` at the next CLK pulse. Q is dependent not only on Set but also on the clock edge.

<img width="629" alt="3-seq-const-prop" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3e31a212-a0b0-42c3-be92-d0075a9f7d1c">

Retiming is a technique to improve the performance of the circuit.

<img width="600" alt="4-seq-adv" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/23bcc15c-813b-496a-aebf-ebbf5ceba557">

## Combinational Logic Optimizations
Commands for optimization

```
opt_clean -purge
```
### Optimization of opt_check.v
Syntax for opt_check.v
```
module opt_check (input a , input b , output y);
        assign y = a?b:0;
endmodule
```
For opt_check.v the assignment `y = a?b:0` reduces to `y = ab`. The screenshot shown below explains this

<img width="533" alt="5-seq-opt" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f4b6a999-f665-412f-a705-9496bfdd04c2">

The logic implementation after synthesis for opt_check.v is shown below, showing only AND gate.

<img width="1120" alt="6-opt_check" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1375f89b-a78f-4702-a1b3-ae9fbcc85ffc">


### Optimization of opt_check2.v
Syntax for opt_check2.v
```
module opt_check2 (input a , input b , output y);
        assign y = a?1:b;
endmodule
```
For opt_check2.v the assignment `y = a?1:b` reduces to `y = a + b`. 

The logic implementation after synthesis for opt_check2.v is shown below, showing only OR gate.

<img width="1115" alt="7-opt-check2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6c8b8eea-c605-4110-9a74-f3b2737ff29f">

### Optimization of opt_check3.v
Syntax for opt_check3.v
```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
For opt_check.v the assignment `y = a?(c?b:0):0` reduces to `y = abc`. The screenshot shown below explains this.

<img width="541" alt="8-opt-check3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c9fb59d8-d080-4776-bec6-46e3b48b3d68">

The logic implementation after synthesis for opt_check3.v is shown below, showing 3 input AND gate.

<img width="1046" alt="9-opt-check3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e0306198-1dd2-4504-9f97-7d7541316160">

### Optimization of multiple_module_opt.v

Syntax of multiple_module_opt.v
```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 
endmodule
```
The logic implementation after synthesis for multiple_module_opt.v is shown below.
<img width="1080" alt="10-multiple-module-opt" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f2486353-e1f9-44e3-97b1-4b235912b69d">


## Sequential Logic Optimizations

Both the dff_const1.v and dff_const2 are explained below.

<img width="851" alt="12-dff-const1-const2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b9bede59-edaa-4f4f-ad90-9036c63aa4da">

### Optimizing dff_const1.v

Syntax for dff_const1.v
```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end

endmodule
```
For dff_const1.v, `q=0` as long as `reset=1`. However, when `reset=0` `q` doesn't immediately becomes `1` rather at the next rising edge of the `clk` as shown below. ***So the optimization cannot be applied***.

<img width="998" alt="11-dff-const1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b046fd71-f0bd-4b79-9345-81ace8795e11">

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The logic implementation after synthesis for dff_const1.v is shown below.

<img width="1680" alt="13-dff-const1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/dcde9e56-06b9-4b20-9b7c-4e44477df7ba">

***complete dff_const2,4,5***

### Optimizing dff_const3.v

Syntax for dff_const3.v
```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end

endmodule
```
For dff_const3.v, there are two flops.  `q1=0` as long as `reset=1`. However, when `reset=0` `q1` doesn't immediately becomes `1` rather at the next rising edge of the `clk` with some propagation delay as shown below. `q=1` as long as `reset=1`, acting as `set` rather than `reset`. However, when `reset=0` `q` samples `q1` as `0` as there are some propagation delay for `q1`as shown below. At the next `clk` edge `q` samples `q1` as `1`.
***So the optimization cannot be applied***.

<img width="973" alt="14-dff-const3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6bf8a7c4-07f1-4f70-9878-e2773b3eeab5">

The command to run HDL simulation
```
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```
The HDL simulation is shown below.

<img width="997" alt="15-dff-const3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3e9440d3-a562-4365-90fb-d17e8ea5c7a2">

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

The logic implementation after synthesis for dff_const3.v is shown below.

<img width="1066" alt="16-dff-const3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6d30d2b7-0c21-464f-9dad-02db228e8c5c">







   


















