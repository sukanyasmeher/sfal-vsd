# sfal-vsd
<details>
	<summary>Day 0</summary>
	
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
</details>

<details>
<summary>Day 1</summary>

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

</details>

<details>

<summary>Day 2</summary>

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
</details>

<details>
	<summary>Day 3</summary>
	
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

## Sequential Optimzations for Unused Outputs

### Optimization of Case1: 3-bit Up Counter with q[0] used (counter_opt.v)
Example of a counter where bits at the position of [2] and [1] are unused.

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
The screenshot explains the logic of the counter. Only q[0] is used. ***So the optimization can be applied***.

<img width="1200" alt="17-counter" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/dbdbd8d5-a305-4f31-b8ba-a8c33d53d67a">

The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
We see only one flop after the synthesis and also seen in synthesis report after `synth -top counter_opt.v`

<img width="1672" alt="18-counter-opt" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/42d260a1-72bc-4d5f-b891-9fea58a57813">

### Optimization of Case2: 3-bit Up Counter (counter_opt2.v)

Example of a counter where all the bits are used.
```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = (count[2:0] == 3'b100);

always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end

endmodule
```
The commands to run the synthesis
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
We see only 3 flops after the synthesis and also seen in synthesis report after `synth -top counter_opt.v`
<img width="829" alt="19-counter-opt2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a7c68bda-5619-4dd6-89d4-d8773c1bf345">

<img width="1669" alt="20-counter-opt2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/95bbe05c-bdde-4bdd-8fe7-492be015dc9f">

<img width="1167" alt="21-counter-opt2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/11cd582b-4ccd-4b99-82e2-1c68c92db131">

</details>

<details>

 <summary>Day 4 </summary>
 
# Day 4 - GLS, Blocking vs Non-blocking and Synthesis-Simulation Mismatch

## GLS, Synthesis-Simulation Mismatch, and Blocking/Non-blocking Statements

### Why is Gate Level Simulation (GLS) necessary?
- Verify the correctness of the design after synthesis
- Ensure the timing of the design is met which is done with delay annotation (timing aware)

  <img width="628" alt="1-gls-iverilog" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e3bef5db-4722-4561-ad10-0370a924fdc9">
  
### Synthesis Simulation Mismatches

It happens because of the following reasons
- Missing sensitivity list
- Blocking vs non-blocking assignments
- Non-standard verilog coding

#### (1) Missing sensitivity list

As shown in the screenshot below, `always` block is evaluated only when `sel` is changing. So output `y` is not evaluated when `sel` is not changing although `i0` and `i1` are changing. Rather it acts like a latch. The code on the right side represents the correct design coding for `mux`. In this case `always` is evaluated for any signal changes. 

<img width="632" alt="2-missing-sensitivity-list" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/5c494646-600e-4b19-a3ce-7db8c1baa5a7">

#### (2) Blocking vs Non-blocking Assignments

 ##### Blocking Statements
 
 - Represented by `=`
 - Executes the statements in the order it is written inside always block
 - So the first statement is evaluated before the second statement

##### Non-Blocking Statements
- Represented by `<=`
- Executes all the RHS when always block is entered and assigns to LHS
- Parallel execution

   The left side of the screenshot below gives us the correct execution. While the right side can lead to serious issues as `d` is assigned to `q` directly. ***So choosing non-blocking statements is best practice*** (highlighted in the screenshot below).

<img width="612" alt="4-blocking-statement" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/bde53551-9858-4eee-afd2-f37cd0dff762">

 ##### Blocking Statements Leading to Synthesis Simulation Mismatch

In the code shown below, `y` gets the old `q0` value. This will mimic delay or flop. But when you synthesize, there will be no flop. If the order is changed (right side code), latest value of `q0` is assigned to `y`. 

When synthesized, both will lead to the same circuit. However, simulation will result in different behavior. For the left side of the code, `y` gets the old `q0` value and for the right side of the code, `y` gets the latest `q0` value leading to a synthesis simulation mismatch. 

This issue is resolved by using ***non-blocking statements***.

 <img width="615" alt="5-blocking-statement" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/aed8bcbb-ca5e-467d-bfd7-8c679ce91263">

## Labs on GLS and Synthesis-Simulation Mismatch

### Ternary operator MUX (ternary_operator_mux.v)

The Verilog code of ternary_operator_mux.v
```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
	assign y = sel?i1:i0;
	endmodule
```
The command to run HDL simulation
```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
HDL Simulation waveform of ternary_operator_mux.v is shown in the screenshot below

<img width="1013" alt="6-ternary-op-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/14d13e7f-e5f7-4d61-9942-5e5e57433db5">

The commands to run the synthesis for ternary_operator_mux.v
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog ternary_operator_mux_net.v
```

<img width="1002" alt="7-ternary-op-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/de35d1cd-8e62-470f-983c-6364f2a76447">

The commands to do GLS for ternary_operator_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
The GLS output is shown below.
<img width="982" alt="8-ternary-op-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7c1e66fd-c9dd-40be-9112-9e33da3e5ebb">

### Bad MUX (bad_mux.v)

The `always` block is executed only at `sel` signal. It works like a flop rather than mux.
The Verilog code of bad_mux.v
```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

The command to run HDL simulation
```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
HDL Simulation waveform of bad_mux.v is shown in the screenshot below

<img width="978" alt="9-bad-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a4c9addd-c225-4c1e-a2bd-32187373e56d">

The commands to run the synthesis for bad_mux.v.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog bad_mux_net.v
```

The synthesis report shows it is still inferring the mux but not the flop.

<img width="1035" alt="10-bad-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3851798a-31c7-4030-a81d-63325bd4d6f3">

The commands to do GLS for bad_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```
The GLS output is shown below. This shows correct functionality which is different from HDL simulation, leading to ***synthesis simulation mismatch***.

<img width="985" alt="11-bad-mux" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/98dad98d-c46e-4b72-9857-e6b30c42367f">

## Labs on Synthesis-Simulation Mismatch for Blocking Statements

### Blocking Caveat (blocking_caveat.v)

The logic to simulate is shown below.
<img width="594" alt="12-blocking-caveat" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d7e6ea20-93fb-41d4-9f56-9910fe8f9931">

The Verilog code of blocking_caveat.v
```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
	d = x & c;
	x = a | b;
end
endmodule
```

The command to run HDL simulation
```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
HDL Simulation waveform of blocking_caveat.v is shown in the screenshot below. `d` takes the old value of `x` causing incorrect functionality.

<img width="976" alt="13-blocking caveat" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0a3a28b9-9647-47b5-83dd-eec1d81092cc">


The commands to run the synthesis for bad_mux.v.
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog blocking_caveat_net.v
```

The synthesis report and logic synthesis is shown below.

<img width="1007" alt="14-blocking-caveat" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2e61c1bd-37b6-4a38-bf5f-39c345f2305b">


The commands to do GLS for bad_mux.v
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```
The GLS output is shown below. In this case, `d` takes the current value of `x` causing incorrect functionality.The waveform shows correct functionality which is different from HDL simulation, leading to ***synthesis simulation mismatch***.

<img width="981" alt="15-blocking-caveat" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b8fcd356-8743-4098-aae8-bdbd8ab88a15">

</details>

<details>
	<summary>Day 5</summary>
	
# Day 5 - Introduction to DFT
## List of some possible issues that arise while manufacturing chips:
- Density Issue: Fabrication processes have become quite complicated with the advent of deep-submicron design technologies. Design elements are coming closer and closer; they are becoming smaller and thinner. Billions of transistors are involved in present-day VLSI chips. So, the chances of two wires touching each other or a very thin wire breaking in between are high. These are a few sources of errors or faults. The point is, that there can be many such errors that can creep in during the design and fabrication processes. So, with an increase in density, the probability of failure also becomes high.
- Software Issue: Moreover, apart from fabrication, there can even be errors in the translation process due to the bugs in CAD software tools used to design the chip.
- Application Issue: There are several critical applications, in which we can’t afford to have faults in the chip at any cost. For example, in medical or healthcare applications, a single fault in the equipment controllers may even risk the life of an individual. For rockets or space shuttles that run on cryogenic fuel, they may need their microcontroller or microprocessor to run on a broader temperature range. Hence the test conditions for these chips should be very application-specific and on an extreme level to prevent any future failures.
- Maintenance Issue: In case of any future failure, for repair or maintenance, we need to identify the proper coordinates of fault. Since PCB sizes are also decreasing, multimeter testing isn’t a viable option anymore. Moreover, moving towards SoC (System on Chip) design, the modular design is losing its relevance, thereby making the maintenance process more expensive.
- Business Issue: If designed chips are found to be faulty, then it transforms into a substantial loss and penalty for the company. Later, we will discuss how detecting a fault earlier decreases the cost of doing business significantly.
  
  Source - (https://technobyte.org/what-is-dft-design-for-testability-introduction/#What_is_Design_for_Testability_and_why_we_need_it)

## 3 W's for DFT - ***What***, ***Why***, ***When/Where***
### What is testability?
In VLSI terms, it means " If a design is well-controllable and well-observable, it is said to be easily testable".  
  
### What is Design for Testability (DFT)?
It a technique that facilitates a design to become testable after production. In another way, adding an extra design for an existing design to make sure it is tested after fabricated.
Some of the examples are 
- For `macros`, we use `MBist logic`
- For `flops`, we use `scan chains`
- for `combinational circuits`, we use `generate test patterns`


### Why do we do DFT?
It makes the testing easy in the post-production process.

<img width="1652" alt="1-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d08e387a-96cb-42f9-80a1-1275f688e3d6">

There are namely 3 levels of testing after a chip is fabricated.
- `Chip-level` or `die-level` where the chip is manufacture, usually done at foundry.
- `Board-level` when chips are integrated on the board/packages to make sure it works with other modules
- `System-level`, when several boards are assembled such as a chip within a laptop.

***DFT is also done due to economical and market needs.*** 

### When and where the DFT design is included?

When is it included? - At the beginning of the ASIC design flow

Where exactly is it included? - during the synthesis (front-end)

## Pros and Cons of DFT

<img width="1087" alt="2-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d7b9c887-2da9-4436-87dd-ed6c1cf01ec4">

## Basic Terminologies

### Controllability
By controllability from DFT point of view, we intend if both `0` and `1` are able to propagate to each and every node within the target patterns. A point is said to be controllable if both `0` and `1` propagated through the scan chains.

For example, there is no way to ***control*** `Node1` shown on the left side of the image below. But adding `MUX` allows that flexibility. However, it increases area, power and timing, ***PPA increases***.

<img width="1091" alt="3-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4da1589b-3cb8-4c91-babb-0f796235a6ca">

### Observability
By observability we are talking about the ability to measure the state of the logic signal. when we say that a node is observable, we mean that the value of the node can be shifted out through scan patterns and can be observed through scan put patterns.

For example, in the image shown below, if we want to observe `Node1`, we need to include a `flip-flop`. In this case, the area and power get affected by increasing, but the timing is not affected as the flop is not on the signal path.

<img width="1082" alt="4-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a0fd5dd4-6c48-45e6-b5eb-10ec1589ba60">

### Fault
It is a physical damage/defect compared to the good system, which may or may not cause a failure.

### Error
An error is caused by a fault because of which the system went to an erroneous state.

### Failure
Failure is when the system is not providing the expected service.

A ***fault*** causes an ***error*** which leads to a system ***failure***.

### Fault Coverage
Percentage of the total number of logical faults that can be tested using a given test set T.

### Defect Level
The fraction of shipped parts that are defective. Or the proportion of the faulty chip in which fault isn't detected and has been classified as good.

## DFT Techniques

<img width="1082" alt="5-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a1325dda-da31-4320-8ada-633d3f84648a">

The DFT compiler converts the flops to scan-enabled flops.

<img width="879" alt="6-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e9bd6e07-ab6e-4f9d-8b2e-3df551707347">

### Scan Chain Techniques
- Specify the scan constraints
- Specify scan ports and scan enables
- Compiling the DFT
- Identifying the no. of scan chains

### Scan-based technique, Scan chains

<img width="1083" alt="7-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/67e00b4e-244b-49c5-9090-f670dae65d9e">

#### What is the purpose of scan flops?

There are various reasons, but 2 main reasons are noted below:
- To test stuck-at faults in the manufactured devices
- To test the path in the manufactured devices for delay that is to test whether each path is working at a functional frequency or not

#### FAQs on Scan Chain

<img width="1081" alt="12-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/cbd27c87-6f7b-4137-b0f6-2f99015bb49b">

### When do we use ATPG?

<img width="1082" alt="13-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a7ccec92-3f60-41d7-a40c-611bdf7ac67f">

### Overview of DFT Compiler?

<img width="888" alt="14-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/916c17b8-3c47-46de-80c0-9ee37fd536e3">

<img width="1033" alt="15-dft" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/75a4e905-9c8e-472f-9e14-a43fe7e428a7">

# Introcution to the Synopsys Tools

## Design Compiler (dc_shell)

The commands to start DC shell and open GUI
```
dc_shell
gui_start
```
<img width="1444" alt="8-dc-shell" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e59c6e46-bcea-42e3-b00e-aa9fd1cd7298">

## Library compiler (lc_shell)

The commands to start LC shell and open GUI
```
lc_shell
gui_start
```

<img width="1475" alt="9-lc-shell" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0fa1f072-1ead-49c2-87b9-f717693c5ec7">

## IC Compiler (icc2_shell)

The commands to start ICC2 shell and open GUI
```
icc2_shell
gui_start
```
<img width="1593" alt="10-icc2-shell" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/68630b50-acc3-475e-9def-0bc7cee6e9f8">
</details>

<details> 
	<summary> Day 6 </summary>

 # Day 6 - Introduction to Logic Synthesis

 ## About the course

 ### Tools that will be used for the lab
 
 - `Iverilog` - for Verilog compilation and simulation
 - `GTKwave` - for viewing the simulation output
 - `Synopsys Design Compiler` - for logic synthesis
 - `Skywater 130nm Library` - for standard cell library

### Expectations from the course
- Understand the various steps involved in ***Digital Logic Synthesis***
- Understand and write ***Synopsys Design Constraints (SDC)*** for the given module
- Perform ***Synthesis*** and write out ***Netlist using Design Compiler***
- Generate and analysis the ***Synthesis Reports and STA Reports***

## Digital Logic and Synthesis
- Digital logic is a switching function
- Behavioral model of design is written in HDL or a programming language called Register Transfer Logic such as VHDL, Verilog, and System Verilog
- The conversion of RTL to gate-level translation is called ***Synthesis*** as shown in the screenshot below.
  
<img width="1056" alt="1-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/cf3260e5-7cd5-4477-83da-b3e9a88e66cd">

### What is .lib?

<img width="876" alt="2-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/8d8c1854-3667-42c5-afc0-fdfc3e45e454">

### Why do we need different flavors of cells?

<img width="1181" alt="3-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/59a9d03d-e6d7-4988-bb33-dd058e1bdf7d">

<img width="1586" alt="4-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d97b5e7f-004a-487d-90de-558882cf08b3">

<img width="1482" alt="5-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/5e77d25c-65ff-4535-ae40-eed814706123">

### How do we select the cells?

This is done through `constraints`.

<img width="1274" alt="6-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b8636231-01cd-4e9c-9026-bb569066b068">

Let's look at this through an example below.

An illustration of the synthesis is described in the screenshot below.

<img width="1076" alt="7-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d22c5400-6eee-41ee-8014-767b5a1a72e9">

The next question is which is the correct implementation from the 3 options shown below.

<img width="1680" alt="8-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/557b88d2-9447-44ae-860b-7e5d357474b3">

<img width="1680" alt="9-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9b67d51b-5f3b-4fc7-86bc-033b921c6f58">

<img width="1680" alt="10-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/283bf723-7ab8-430c-9fcf-bbe8ab3ea852">

From the above discussion, `Implementation 3` is the best. But the question is, is it always the case?

Not really, especially on the following conditions
- If the logic is present in the hold-sensitive path.
- Additional buffers will add to both power and area.

In conclusion, all the 3 implementations are correct and will be picked based on the need. These needs are defined by the `constraints`. Constraints guide the synthesizer to select the correct library cells that are the most appropriate for the design.

## Introduction to Design Compiler

### What is DC?

<img width="1680" alt="11-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/284a27c2-a065-489c-b04f-da70d776a251">

### Common terminologies associated with DC

<img width="1680" alt="12-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/cf07287d-9fb8-44ad-808c-a7a6a0445af0">

### Synopsys Design Constraints (SDC)

<img width="1680" alt="13-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/662cd37d-5ddc-4aea-90df-83a42961ac3c">


### How do we set up Design Compiler (DC)?

<img width="1680" alt="14-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0a1cfc9d-b846-4d85-89d8-6f161f9653ab">

### Implementation flow of ASIC

Steps in converting RTL to the physical database or GDS

<img width="1680" alt="15-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/72da8396-4912-4b63-82a5-02ec2ebb334f">

### DC Synthesis Flow

Link the design means to know that all the information in the design can be implemented in the form of standard cell libraries.

<img width="1680" alt="16-logicsyn" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b72c28f4-8337-4ad9-b188-75e0a3ba8060">

## Lab 1 - Invoking DC Basic Setup

The DC is invoked in the folder `/home/sukanya/VLSI/sky130RTLDesignAndSynthesisWorkshop`
Commands to invoke the DC compiler
```
csh
dc_shell
echo $target_library //It generates your_library.db
echo $link_library
read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v
write -f verilog -out lab1_net.v
sh gvim lab1_net.v
```
`your_library.db` is imaginary non-existent library, a dummy library invoked in the DC.

The Verilog syntax of `lab1_flop_with_en.v` (DFF with Asynchronous Reset)
```
module lab1_flop_with_en ( input res , input clk , input d , input en , output reg q);
always @ (posedge clk , posedge res)
begin
	if(res)
		q <= 1'b0;
	else if(en)
		q <= d;	
end
endmodule
```

The screenshot after `read_verilog` command is shown below. Let's try to understand the meaning of the messages generated. `gtech.db` and `standard.sldb` are internal dbs (virtual library) in DC memory for understanding the design. It is invoking HDL compiler or `Presto HDL Compiler`.
The line `Warning: Can't read link_library file 'your_library.db'. (UID-3)` means that DC compiler can't read your_library.db which was created for a dummy purpose. We need to point to the correct library.
In the line `Compiling source file /home/sukanya/VLSI/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/verilog_files/lab1_flop_with_en.v`, it is compiling the source file and it infers the registers or memory device which is 1-bit flip-flop with asynchronous reset.

We need to link the design properly and point to the proper technology library.

<img width="1025" alt="17-lslab" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/137c6aa2-a57d-4de8-82a4-4ff555a4f363">

With the commands below we still get that the dummy library can't be linked. Since no proper standard cell library is provided, it has used `gtech` as the reference library to synthesize. 
```
dc_shell> write -f verilog -out lab1_net.v
Warning: Can't read link_library file 'your_library.db'. (UID-3)
Writing verilog file '/home/sukanya/VLSI/sky130RTLDesignAndSynthesisWorkshop/lab1_net.v'.
1
```
The Verilog netlist for lab1_net.v is opened using the command `sh gvim lab1_net.v` looks like as shown below. The netlist is not in form of sky130 library cells.

```
module lab1_flop_with_en ( res, clk, d, en, q );
  input res, clk, d, en;
  output q;


  \**SEQGEN**  q_reg ( .clear(res), .preset(1'b0), .next_state(d), 
        .clocked_on(clk), .data_in(1'b0), .enable(1'b0), .Q(q), .synch_clear(
        1'b0), .synch_preset(1'b0), .synch_toggle(1'b0), .synch_enable(en) );
endmodule
```

The correct commands for reading the correct library using DC Compiler
```
csh
dc_shell
set target_library /home/sukanya/VLSI/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
set link_library {* $tarhet_library}
read_db DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
read_verilog DC_WORKSHOP/verilog_files/lab1_flop_with_en.v
link
compile
write -f verilog -out lab1_net_sky130.v
sh gvim lab1_net_sky130.v
```
There may be multiple libraries in DC memory. * represents all the libraries already loaded in DC memory. So we are appending an additional library without overwriting the already loaded library.
With the correct library specification, the Verilog netlist is appropriately generated with skywater130 libraries as shown below

```
module lab1_flop_with_en ( res, clk, d, en, q );
  input res, clk, d, en;
  output q;
  wire   n2, n3;

  sky130_fd_sc_hd__dfrtp_1 q_reg ( .D(n3), .CLK(clk), .RESET_B(n2), .Q(q) );
  sky130_fd_sc_hd__mux2_1 U5 ( .A0(q), .A1(d), .S(en), .X(n3) );
  sky130_fd_sc_hd__clkinv_1 U6 ( .A(res), .Y(n2) );
endmodule
```
## Lab 2 - Introduction to DDC GUI using Design Vision

Command to write the DDC file after compilation of the design
```
write -f verilog -out lab1_net_sky130.v
```

Commands to launch Design Vision (GUI format of Design Compiler)
```
csh
design_vision
start_gui //If GUI doesn't start automatically
```

Once GUI is opened, the command to open DDC is 
```
read_ddc lab1.ddc
```
If we type `read_verilog` in GUI, it reads only the Verilog file. But `read_ddc` reads the library as well. So, DDC will save all the information in the tool memory in that particular session. Only disadvantage is DDC is Synopsys's proprietary format.

<img width="1301" alt="18-lslab" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b38fc689-4a24-4798-8106-d68b2b61537b">

<img width="1193" alt="19-lslab" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ce191f29-98c2-4c86-98f6-23d80f10430f">

## Lab 3 - DC Compiler
`.synopsys_dc.setup` from `User home directory` overwrites the `default .synopsys_dc.setup` that is installed under DC.

<img width="1218" alt="20-lslab" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/dbabdc56-57e4-4aee-bc4c-6dbc96c98488">

Invoke the setup file by `gvim .synopsys_dc.setup` and copy paste the commands

```
set target_library /home/sukanya/VLSI/sky130RTLDesignAndSynthesisWorkshop/DC_WORKSHOP/lib/sky130_fd_sc_hd__tt_025C_1v80.db
set link_library {* $target_library}
```
## Tool Command Language (TCL) Quick Refresher
The location of `{` `}` is very important.

<img width="937" alt="21-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1c319176-2690-4042-a55b-4fb1001e2cc5">
<img width="1008" alt="22-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/31146aa1-4156-4b1d-8474-bb06b1435ce9">
<img width="1181" alt="23-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e772cbba-0958-4460-90a1-977cab6c0187">
<img width="1185" alt="26-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/40832bcf-d0da-4524-8b24-7bab17dafc98">
<img width="1172" alt="25-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4f1a6890-55fe-4780-ac7c-06787aef3990">
<img width="878" alt="24-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f582ee10-1671-48c8-a205-244dfd1ca714">

## Lab 4 - TCL Scripting

<img width="1178" alt="27-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3f3d85b2-ba62-4a24-bfa1-5e3a02f07ea9">

Command to launch `gvim` within DC compiler
```
sh gvim &
```
<img width="841" alt="28-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0b1d4aca-fdff-4f33-8d46-b22c6b3cb95b">

Command to see `and` gates in `.lib` file
<img width="1675" alt="29-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ccf3ed5d-c112-4cc2-9c49-a1ceb62385dd">

Syntax to print each `and` gate in `.lib` file

```
foreach_in_collection my_var [get_lib_cells */*and*] {
	set my_var_name [get_object_name $my_var];
	echo $my_var_name;
}
```

The output looks like the one shown in the screenshot

<img width="1047" alt="30-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4896877e-a751-486a-8d25-443ede1e27a5">

Syntax to print `multiplication` table

```
echo "Printing Multiplication Table"

set i 10;
set j 1;
while {$j < 21} {
	echo [expr $i*$j];
	incr j;
}
```

The output is shown below

<img width="453" alt="31-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c92d4b29-9d7d-4f09-9288-a57dd9b487b1">

Syntax to read from a list

```
set mylist [list a b c d e f g];

foreach myvar $mylist {
	echo $myvar;
}
echo $mylist;
```
The output is shown below

<img width="268" alt="32-lab-tcl" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/8f455335-ccde-400b-9d27-39d60b488a9a">


</details>

<details> 
	<summary> Day 7 </summary>

# Day 7 - Basics of Static Timing Analysis (STA)

## Introduction to STA

### Max and Min Delay Constraints

<img width="1203" alt="1-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/772ea1c9-ca38-4967-ac99-869232150a6c">

### Water Bucket Analogy for Delay

The inflow of water translates to the inflow of current. Fast current sourcing (fast rise time of input) => less delay.

So delay is a function of `input transition` (inflow) and `load capacitance` (the size of the bucket).

<img width="1215" alt="2-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a0bb9c46-f2ea-409c-8426-ac74dad8bfba">

<img width="1223" alt="3-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/24aca3cf-2d0b-482e-b088-72af7a8baab7">

<img width="1149" alt="4-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/45daf929-b61f-4964-bde6-4c0faa584ea0">

### Timing Arcs

<img width="1180" alt="5-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/fdd5bb66-8b88-4ceb-86d9-b741e7d89379">

<img width="1163" alt="6-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ea55b5b2-28b1-415c-95d9-d557887597cf">

<img width="1216" alt="7-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a62047e5-0ead-4cbe-bf81-b5a67eeb7845">

## What are Constraints and Why are They Needed?

The path that decides or limits the max clock frequency is the `critical path`. If the critical path delay is reduced, a better frequency can be achieved. We need to tell the tools to pick library cells with specific delays. This is defined through `constraints`.

<img width="1214" alt="8-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3a34189f-8d5f-489e-9ab2-db87273d6c9a">

<img width="1220" alt="9-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/815ddb1d-a08d-4d67-a302-3f8cc910d0ad">

## Different Timing Paths

<img width="1161" alt="10-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/674af831-0b39-4d47-8e60-36f032b3781e">

Let's assume the clock frequency is 500MHz or the clock period is 2ns. So if the delay of flops is 0.5ns each, by combining they account for a 2ns delay in the path. So we only have the rest 1ns delay leverage for combinational logic. As shown in the screenshot below,it is actually the clock period that limits the combinational logic.

<img width="1177" alt="11-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/089ace72-3b1f-4409-9eb8-fe8511725847">

***So a couple of constraints that can be specified are:***

(1) So the constraint given to the synthesis tool is the `acceptable clock period as constraint`. So the tool will pick appropriate cells for the combinational logic.

(2) Input constraint

<img width="1197" alt="12-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/655e47e8-ef65-49db-a848-8e63d7e76f47">


(3) `REG3` is sending the output to the external outside flop `REG_EXT_3` through `Output Logic` and both the flops are clocked by the same clock `CLK`. This becomes a synchronous path. There should be a limit in the output logic, how much delay this logic can consume on the path. So we need to constraint the output path.

<img width="1223" alt="13-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2bc2390f-2957-4541-9147-cd0c9b3bcb85">

<img width="1148" alt="14-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4772a70e-2972-4d77-9aa1-6adcd789aff6">

We can constraint I/O logic by specifying the input and output external delay with the associated clock `CLK`. 
I/O delay modelling can come from:
- Standard interface specifications like I2C, SPI
- IO budgeting based on interactions with other modules

<img width="1229" alt="15-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/5ba038af-005e-43df-a3de-a74e9315f494">

## Input Transition and Output Load

### Is IO Delay Modeling Sufficient?

<img width="1184" alt="16-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/fa7b7093-3640-4290-a843-718997a16b6a">

<img width="1215" alt="17-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2d5dd211-6f47-481b-af0e-61afa63208e3">

70% of clock period for external delay and 30% of clock period for internal delay as rule of thumb.

<img width="1134" alt="18-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6da5358d-a587-4663-870c-0041a317908f">

## Lab 1 - Timing .lib

- PVT operating conditions are mentioned
- Technology - CMOS

### Max_transition 

```
default_max_transition : 1.5000000000;
```
Output has a pin capacitance, net has capacitance and there is input capacitance at the next gate or sum of all input capacitance for fan out > 1.
If the library has a max capacitance limit of 1.5pF, the DC will use this value and divide the net and add buffer in the path as shown in the green diagram.

<img width="1101" alt="19-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6da90ea8-5785-4479-96a5-71dadd8f9e1d">

### Delay Model Look Up Table

<img width="1172" alt="20-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d296138a-4d4e-4648-89ca-61651df2176b">

There are attributes associated with the pin such as whether it is clock or not, pin capacitance, direction, and more. 

### Unateness

<img width="1218" alt="21-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c28286f2-c3d0-42d4-bbe9-62326d0319d1">

## Lab 2 - Exploring .lib (Part 1)

- `CLK_N` is the active low clock. Attribute `clock` is `true`. While for the `D` pin, attribute `clock` is `false`.
- `timing_sense : "non_unate"` - `non_unate` means concerning clock `Q` may be rising or falling (called no unateness).
- `timing_type : "falling_edge"` - sequential timing arc

For example, `CLK` has `setup_rising` attribute while `CLK_N` has `setup_falling` attribute as shown below.

<img width="1675" alt="22-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/49d1624c-24b9-43c2-9981-16f3a40b8920">

For latch the point when data is sampled is different than flop.

<img width="1216" alt="23-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3053204b-d5b3-44db-89ab-2a06c74bd3df">

<img width="1667" alt="24-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2b8878b5-7bc2-4599-bc0b-935a50d6df0c">

## Lab 3 - Exploring .lib (Part 2)

### Query the properties of library from dc_shell

Command to check the library loaded in the design

```
dc_shell> list_lib
```
Command to get a particular `cell` in library

```
get_lib_cells */*and*
```
<img width="1061" alt="25-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/12efea20-2b60-4762-b5b3-0e920d9758ef">

Command to get the list of that particular cell as a list but the command below shows only the pointer names

```
foreach_in_collection my_lib_cell [get_lib_cells */*and*] {
echo $my_lib_cell;
}
```

Command to get the list of that particular cell as a list 
```
foreach_in_collection my_lib_cell [get_lib_cells */*and*] {
set my_lib_cell_name [get_object_name $my_lib_cell]; echo $my_lib_cell_name;
}
```

<img width="1058" alt="26-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d14456c1-33fd-4afb-a940-7a990a8c8b09">

Command to get the pins in a cell (sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0)
```
get_lib_pins sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/*
```
<img width="1042" alt="27-lab-sta" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a398bb71-a288-4f66-b6a7-bc0355dbf407">

Functionality is shown on the `output` pin.
`get_lib_attribute` is the command to query the attribute of pin or cell.

Command to query the function of the `output` pin

```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X function
```

Output is shown as 
```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X function
Performing get_lib_attribute on library object 'sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X'. 
A&B
```
Command to query the direction of the `output` pin

```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/X direction
```
Command to query the area of the cell 
```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0 area
```

Command to query the capacitance of the pin

```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A capacitance
```
Command to query the if a pin is clock or not

```
get_lib_attribute sky130_fd_sc_hd__tt_025C_1v80/sky130_fd_sc_hd__and2_0/A clock
```

Command to query all the sequential cells in the library 

```
get_lib_cells */* -filter "is_sequential == true"
```

***Command to list all attributes***
```
list_attributes -app
```

</details>

<details>
<summary> Day 8 - Advanced Constraints </summary>

# Day 8 - Advanced Constraints

## Clock Tree Modelling and Uncertainty

### Till now we have seen the constraints defined are 
- Reg2Reg - Clock period
- Input to Reg - Clock period, Input delay and Input transition
- Reg to Output - Clock period, Output delay and Output transition

### What needs to be constrained for the clock ?
 One is clock period. But the question is will the clock arrive at the same time to the flop? In practical scenerio, it is not possible.
 Clock is built only during CTS, before which clock is an ideal network. But clock doesn't reach all te flops at the same time.
 During synthesis, logic is optimized considering ideal clock netwrok. Should synthesis consider the practicality of clock network?

 ### Clock Generation and Jitter
 
<img width="1195" alt="1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/505d9576-960a-401d-87da-ca29b06d1c5f">

Jitter happens due to stocastic variations of clock generator. Jitter can have serious effect on clock timing.

### Clock Distribution and Skew

<img width="1242" alt="2-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/58ad127c-e48a-4ee5-ac88-af52d1f059f3">
 
<img width="1222" alt="3-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ced6b409-673a-4a51-a0fb-52bcc0d9363b">

### Factors considered for Clock Modelling
<img width="1219" alt="4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0243511c-8d7f-401f-9a05-7e6c05f84896">

## IO Delays

<img width="1173" alt="5-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6b85dabe-7b37-4c7a-a53c-e5e3668284d0">

### Getting the Ports in DC 

<img width="1136" alt="6-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/21a78514-a4b5-4170-bd68-95c8f9044e53">

### Getting the Clock in DC 
<img width="1173" alt="7-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/bcb23fb3-c904-417f-8f15-f6156f07a918">

### Querying the Cells in the Design

<img width="1173" alt="7-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/a45e0199-2573-4de1-a6b0-93d71a69f99c">

<img width="1216" alt="8-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b3e57bab-ff0d-4912-8e09-067d4abaaa0d">

<img width="1217" alt="9-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/274a70ee-d351-482e-8073-00d4175b8f24">

### Creating Clock or Clock Distribution

<img width="1226" alt="10-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/68521f32-cf0a-4cde-8ed0-1bc31ffa601d">

<img width="1207" alt="11-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9192a11d-b1db-4265-8c56-62dd6097230a">

### Constraining IO Paths

<img width="1221" alt="12-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0b31f392-9078-43cb-bc52-32bfba402d71">

<img width="1210" alt="13-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e4c370d5-951e-4d32-b8f8-eb87c70ec2d1">

# Lab 1 - Loading design get_cells, get_ports, get_nets

Verilog code of lab8_circuit.v
```
module lab8_circuit (input rst, input clk , input IN_A , input IN_B , output OUT_Y , output out_clk);
reg REGA , REGB , REGC ; 

always @ (posedge clk , posedge rst)
begin
	if(rst)
	begin
		REGA <= 1'b0;
		REGB <= 1'b0;
		REGC <= 1'b0;
	end
	else
	begin
		REGA <= IN_A | IN_B;
		REGB <= IN_A ^ IN_B;
		REGC <= !(REGA & REGB); 
	end
end

assign OUT_Y = ~REGC;

assign out_clk = clk;

endmodule
```
The circuit is shown below 

<img width="902" alt="14-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7c93393e-a166-4543-9687-087af1d62226">

Syntax to run the command in DC under `/verilog_files` folder
```
csh
dc_shell
read_verilog lab8_circuit.v
link
compile_ultra
```
After reading the verilog file, make sure that there is no error. It has inferred 3 registers, 1-bit wide, asynchronous reset type as shown below.

<img width="1053" alt="15-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/655e5e31-2de3-4f98-8107-eb91e6fc95f4">

After compilation make sure that there is no error.

<img width="1049" alt="16-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f0868f9a-5049-43d8-9b3f-00b13aa7d0dd">

### Commands with get_ports
<img width="1053" alt="17-lab-ac1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/5790ddc4-5d63-449e-bbdb-9719402c18c5">

### Commands with get_cells

Syntax to check if a cell is hierarchical, where U9 is cell
```
get_attribute [get_cells U9] is_hierarchical
```
Syntax to check all the cells which are hierarchical or not
```
get_cells * -hier -filter "is_hierarchical == false"
or
get_cells * -hier -filter "is_hierarchical == true"
```
To know the reference name of the cell in the library

<img width="1208" alt="18-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/38a4f8ca-dfab-43bf-b403-268aa1803e91">

Syntax to know the reference name of cell REGA_reg. The output obtained is `sky130_fd_sc_hd__dfrtp_1` where `dfrtp` means DFF with `r` means asynchronous reset, `p` means positive triggered and `t` means with true output.
```
get_attribute [get_cells REGA_reg] ref_name
```
Syntax to know the reference name of all the cells in the design
```
foreach_in_collection my_cell [get_cells * -hier] {
set my_cell_name [get_object_name $my_cell];
set rname [get_attribute [get_cells $my_cell_name] ref_name];
echo $my_cell_name $rname;
}
```
The output is shown below.

<img width="1066" alt="19-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/dd00b1a8-aa8a-4463-835a-363b31982cb8">

Command to write DDC of the design
```
write -f ddc -out lab8_circuit.ddc
```
Open the Design Vision with command `design_vision` in the terminal.
Command to read the DDC of the design in Design Vision GUI
```
read_ddc lab8_circuit.ddc
```
<img width="1678" alt="20-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e015dd69-53cc-47fc-b3fc-9a4ed05ac053">

Syntax to get all nets of the design and typed in Design Vision
```
get_nets *
```
Syntax to check what a particular net such as N1 is connected to 
```
all_connected N1
```

***In design digital net can only be driven by one driver***

<img width="1217" alt="21-lab1-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/effadc79-ac28-45a1-84f5-cacba8c64e06">

## Lab 2 - get_pins, get_clocks, querying_clocks

Syntax to get all pins
```
get_pins *
```

Synatx to read all the pins individually
```
foreach_in_collection my_pin [get_pins *] {
set pin_name [get_object_name $my_pin];
echo $pin_name;
}
```
Output is shown below.

<img width="1071" alt="22-lab2-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0b71cf0b-4248-4429-bd0e-99175a43fff9">

Synatx to check direction of a pin
```
get_attribute [get_pins REGC_reg/RESET_B] direction
```
Synatx to whether a pin is clock pin or not. ***This attibute should only be queried on input pins***
```
get_attribute [get_pins REGC_reg/RESET_B] clock
```
Syntax to know the direction of all pins
```
foreach_in_collection my_pin [get_pins *] {
set my_pin_name [get_object_name $my_pin];
set dir [get_attribute [get_pins $my_pin_name] direction];
echo $my_pin_name $dir;
}
```

Syntax to get the pins with all the clock attibute
```
foreach_in_collection my_pin [get_pins *] {                                                                                                                                                                                         set my_pin_name [get_object_name $my_pin];                                                                                                                                                                                            set dir [get_attribute [get_pins $my_pin_name] direction];                                                                                                                                                                    if { [regexp $dir in] } {
if { [get_attribute [get_pins $my_pin_name] clock ] } {
echo $my_pin_name;
}
}
}
```

Synatx of query_clock_pin_sm.tcl
```
foreach_in_collection my_pin [get_pins *] {
	set my_pin_name [get_object_name $my_pin];
        set dir [get_attribute [get_pins $my_pin_name] direction];                                                                                              
	if { [regexp $dir in] } {
		if { [get_attribute [get_pins $my_pin_name] clock ] } { 
 			echo $my_pin_name;

		}
	}
}
```
Output is shown below.

<img width="1069" alt="23-lab2-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/cd968e75-15a1-4fa2-9e85-f47984f51586">

<img width="1007" alt="24-lab2-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1de3f211-aefe-431a-bb9b-7543ef46c7fb">


## Lab 3 - create_clock waveform

The command `current_design` tells the name of top module

Syntax to create a clock with period of 10ns
```
create_clock -name MYCLK -period 10 [get_ports clk]
```

Syntax to know the clocks
```
get_clocks *
```

Syntax to get the period of the clock
```
get_attribute [get_clocks MYCLK] period
```
Syntax to check whether the clock is generated or not. If `false` then it is a master clock.
```
get_attribute [get_clocks MYCLK] period
```
Syntax to know the information about all clock
```
report_clocks *
```

<img width="1065" alt="25-lab3-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1d139938-e81b-4125-bd4d-11ff33925626">

Syntax to query the attributes of all clock pin 
```
foreach_in_collection my_pin [get_pins *] {
	set my_pin_name [get_object_name $my_pin];
        set dir [get_attribute [get_pins $my_pin_name] direction];                                                                                              
	if { [regexp $dir in] } {
		if { [get_attribute [get_pins $my_pin_name] clock ] } { 
			set clk [get_attribute [get_pins $my_pin_name] clocks]; # 	set clk_name [get_object_name [get_attribute [get_pins $my_pin_name] clocks]];
			set clk_name [get_object_name $clk];
 			echo $my_pin_name $clk_name;

		}
	}
}
```
Output is shown below

<img width="374" alt="26-lab3-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b3824590-7ff3-449b-904d-f54f0f3522d6">

***Do not create a clock on the pin but rather create one on the port.*** The clock created on the pin will not reach to any pin of any flop.

Syntax to remove the clock, BAD_CLK is the clock name
```
remove_clock BAD_CLK
```

Syntax to create a clock with different rising and falling time
```
create_clock -name MYCLK -period 10 [get_ports clk] -wave {5 10}
```

Syntax to create a clock with a different duty cycle
```
create_clock -name MYCLK -period 10 -wave {0 2.5} [get_ports clk]
```
The order of command doesn't matter when creating a clock.

# Lab 4 - Clock Network Modelling - Uncertainty, report_timing

<img width="1000" alt="27-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b74e590c-dd64-4712-8436-07698c341956">

Syntax to model the source latency 
```
set_clock_latency -source 1 [get_clocks MYCLK]
```
Syntax to model the network latency 
```
set_clock_latency 1 [get_clocks MYCLK]
```
Syntax to model uncertainity for max delay or setup which is default
```
set_clock_uncertainty 0.5 [get_clocks MYCLK]
```
Syntax to model uncertainity for max delay or hold 
```
set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]
```
***If no clock is present, `report_timing` shows `path is unconstrained`***

syntax to report clock to register
```
report_timing -to REGC_reg/D
```
The result is shown below.

<img width="755" alt="28-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/fedfc44a-11da-471f-ba5a-1ba606875fef">

After modeling the clock for following commands
```
dc_shell> set_clock_latency -source 2 [get_clocks MYCLK]
1
dc_shell> set_clock_latency 1 [get_clocks MYCLK]
1
dc_shell> set_clock_uncertainty 0.5 [get_clocks MYCLK]
1
dc_shell> set_clock_uncertainty -hold 0.1 [get_clocks MYCLK]
```
The result is shown below.

<img width="711" alt="29-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3dcff3bc-37fd-425f-bbe2-2dcf307e46db">

Calculation for uncertainty and skew and why are they subtracted from the clock period

<img width="1006" alt="30-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7a83ae1f-adc9-404a-884f-a7c0ff009bbe">

Calculations for hold, uncertainty, and skew
<img width="990" alt="31-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ba4f8b1f-57dc-4acf-9d3c-0a8c8e23a610">

<img width="721" alt="32-lab4-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/515c2466-0c6d-4d1d-a214-a404f3f2320a">

# Lab 5 - IO Delays

Status until now
<img width="902" alt="33-lab5-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0c1046ed-b010-4bd3-855a-413a6fcc3ed2">

Syntax to know the modeling of ports and pins 
```
report_port verbose
```
Syntax to model the input port delay
```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports IN_A]
```
Syntax to know the timing around port IN_A
```
report_timing -from IN_A
```
<img width="915" alt="34-lab5-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/33bebb45-0df9-4126-acc7-450635bfb3ce">

Syntax to model the transition in port IN_A and write to a file `a`
```
report_timing -from IN_A -trans -net -cap -nosplit > a
```
<img width="951" alt="35-lab5-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1c596057-579f-4876-958e-8e18251c9077">

Syntax to set the hold time for port IN_A(or IN_B)
```
set_input_delay -min 1 -clock [get_clocks MYCLK] [get_ports IN_A]
```

Syntax to check the hold timing for port IN_A
```
report_timing -from IN_A -trans -net -cap -nosplit -delay_type min > a
```
<img width="946" alt="36-lab5-ac" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2cb84c2c-df05-4b75-b496-8871541aa13f">

Syntax to set a max transition for port IN_A
```
set_input_transition -max 0.3 [get_ports IN_A]
```

Syntax to set min transition for port IN_A
```
set_input_transition -min 0.1 [get_ports IN_A]
```

Syntax to model the output port delay
```
set_input_delay -max 5 -clock [get_clocks MYCLK] [get_ports OUT_Y]
```

Syntax to model a max load for output port
```
set_load -max 0.4 [get_ports OUT_Y]
```

Syntax to model a min load for output port
```
set_load -min 0.1 [get_ports OUT_Y]
```

</details>













   


















