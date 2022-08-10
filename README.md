# RTL-design-using-Verilog-with-SKY130-Technology
![Verilog-flyer](https://user-images.githubusercontent.com/104454253/166084640-128e6351-1739-4b38-a3ce-76459da921b5.png)
# Table of contents
 - [1. Introduction](#1-Introduction)
 - [2. Day-1- Introduction to Verilog RTL design and Synthesis](#2-Day-1--Introduction-to-Verilog-RTL-design-and-Synthesis)
    - [2.1 Various aspects of frontend design](#21-Various-aspects-of-frontend-design)
    - [2.2 Introduction to open source simulator iverilog and gtkwave](#22-Introduction-to-open-source-simulator-iverilog-and-gtkwave)
     	- [2.2.1 Lab examples using iverilog and gtkwave](#221-Lab-examples-using-iverilog-and-gtkwave)
    - [2.3 Introduction to Yosys synthesizer](#23-Introduction-to-Yosys-synthesizer)
        - [2.3.1 Labs on Yosys introduction](#231-Labs-on-Yosys-introduction)
 - [3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles](#3-DAY2-Timing-libs-hierarchical-flat-synthesis-efficient-flop-coding-styles)
    - [3.1 Introduction to timing .libs](#31-Introduction-to-timing-libs)
        - [3.1.1 LAB- Introduction to dot Lib](#311-LAB--Introduction-to-dot-Lib)
    - [3.2 LAB- Hierarchical synthesis and flat synthesis](#32-LAB-Hierarchical-synthesis-and-flat-synthesis)
    - [3.3 Various Flop coding styles and optimization](#33-Various-Flop-coding-styles-and-optimization)
         - [3.3.1 Lab- flop synthesis simulations](#331-Lab-flop-synthesis-simulations)
         - [3.3.2 Interesting optimisations](#332-Interesting-optimisations)
 - [4. Day3- Combinational and sequential optmizations](#4-Day3--Combinational-and-sequential-optmizations)
    - [4.1 Combinational logic optimization with examples](#41-Combinational-logic-optimization-with-examples)
    - [4.2 Sequential logic optimization with examples](#42-Sequential-logic-optimization-with-examples)
        - [4.2.1 Basic](#421-Basic)
        - [4.2.2 Advanced](#422-Advanced)
        - [4.2.3 Sequential optimisation of unused outputs](#423-Sequential-optimisation-of-unused-outputs)
 - [5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch](#5-DAY4--GLS-blocking-vs-non-blocking-and-Synthesis-Simulation-mismatch)
    - [5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements](#51-GLS-Synthesis-Simulation-mismatch-and-Blocking-Non-blocking-statements)
        - [5.1.1 GLS Concepts And Flow Using Iverilog](#511-GLS-Concepts-And-Flow-Using-Iverilog)
        - [5.1.2 Synthesis Simulation Mismatch](#512-Synthesis-Simulation-Mismatch)
    - [5.2 Lab- GLS Synth Sim Mismatch](#52-Lab--GLS-Synth-Sim-Mismatch)
    - [5.3 Lab- Synthesis simulation mismatch blocking statement](#53-Lab--Synthesis-simulation-mismatch-blocking-statement)
 - [6. DAY5- if, case, for loop and for generate](#6-DAY5--if-case-for-loop-and-for-generate)
    - [6.1 If and Case constructs](#61-If-and-Case-constructs)
       - [6.1.1 If construct](#611-If-construct)
       - [6.1.2 Case construct](#612-Case-construct)
    - [6.2 Lab- Incomplete IF](#62-Lab--Incomplete-IF)
    - [6.3 Lab- incomplete overlapping Case](#63-Lab--incomplete-overlapping-Case)
    - [6.4 For Loop and For Generate](#64-For-Loop-and-For-Generate)
    - [6.5 Lab- For and For Generate](#65-Lab--For-and-For-Generate)
 - [7. Word of Thanks](#7-Word-of-Thanks)
# 1. Introduction
This report is a final submission of 5-day workshop from [VLSI Sytem Design-IAT](https://www.vlsisystemdesign.com/) on RTL design and synthesis using open source tools, in particular iVerilog, GTKWave, Yosy and Skywater 130nm Standard Cell Libraries  
# 2. Day-1- Introduction to Verilog RTL design and Synthesis
## 2.1 Various aspects of frontend design
**RTL Design**: In simple terms RTL design or Register Transfer Level design is a method in which we can transfer data from one register to another. In RTL design we write code for Combinational and Sequential circuits in HDL(Hardware Description Language) like Verilog or VerilogHDL which can model logical and hardware operation. RTL design can be one code or set of verilog codes. **One key note is that we need to write RTL design with optimized and synthesizable (realizable as physical gates)**.

**Sample RTL design outline:**

	module module_name (port list);
		//declarations;
		//initializations;
		//continuos concurrent assigments;
		//procedural blocks;
	endmodule

**Test Bench**: Using Verilog we can write a test bench to apply stimulus to the RTL design and verify the results of the design by instantiating design with in test bench. Up-front verification becomes very important as design size increases in size and complexity while any project progresses. This ensures simulation results matches with post synthesis results. A test bench can have two parts, the one generates input signals for the model to be tested while the other part checks the output signals from the design under test. It can be represented as follows.

![Capture2](https://user-images.githubusercontent.com/104454253/166088950-634be5a4-7d5a-4b43-9990-711f8f660aaf.JPG)

**Simulation**: RTL design is checked for adherence to its design specification using simulation by giving sample inputs. This helps finding and fixing bugs in the RTL design in the early stages of design development. 

**Simulator**: Simulator is the tool used for this process. It looks for changes on input signals to evaluate outputs. No change in output if there is no change in input signals
Here is the flow of frondend design:

![Capture1](https://user-images.githubusercontent.com/104454253/166088866-80a4e792-7db7-4bf2-b3b5-b4b9b92452a8.JPG)

## 2.2 Introduction to open source simulator iverilog and gtkwave
**iverilog**: iverilog stands for Icarus Verilog. Icarus Verilog is an implementation of the Verilog hardware description language. It supports the 1995, 2001 and 2005 versions of the standard, portions of SystemVerilog, and some extensions.

**Gtkwave**: GTKWave is a fully featured GTK+ based wave viewer for Unix, Win32, and Mac OSX which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing. 

### 2.2.1 Lab examples using iverilog and gtkwave

We were introducted to Linux operating system and were made aware of the basic commands. Using **git clone** command we've cloned library files like standard cell library, primitives which are used for synthesis and few verilog codes for practice.

<img width="1512" alt="Screenshot 2022-08-06 at 5 44 53 PM" src="https://user-images.githubusercontent.com/110079631/183714284-f3842294-2ee0-4db7-9249-ef5071da20e8.png">

In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 

<img width="1511" alt="Screenshot 2022-08-06 at 5 49 24 PM" src="https://user-images.githubusercontent.com/110079631/183714482-3a07a3be-6a90-4b6b-b771-fc3173f28423.png">


Here is the code and gtkwave snippets:<br />

	module good_mux (input i0 , input i1 , input sel , output reg y); 
		always @ (*)
		begin
			if(sel)
			y <= i1;
			else 
			y <= i0;
		end
	endmodule**


	`timescale 1ns / 1ps
	module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;
      		// Instantiate the Unit Under Test (UUT), name based instantiation
		good_mux uut (.sel(sel),.i0(i0),.i1(i1),.y(y));
		//good_mux uut (sel,i0,i1,y);  //order based instantiation
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
	endmodule**
	

<img width="999" alt="Screenshot 2022-08-06 at 5 54 21 PM" src="https://user-images.githubusercontent.com/110079631/183714569-ac176c16-622a-4dac-910a-2529a4f07a87.png">


## 2.3 Introduction to Yosys synthesizer

**Synthesis**: Synthesis transforms the simple RTL design into a gate-level netlist with all the constraints as specified by the designer. In simple language, Synthesis is a process that converts the abstract form of design to a properly implemented chip in terms of logic gates.

Synthesis takes place in multiple steps:
- Converting RTL into simple logic gates.
- Mapping those gates to actual technology-dependent logic gates available in the technology libraries.
- Optimizing the mapped netlist keeping the constraints set by the designer intact.

**Synthesizer**: It is a tool we use to convert out RTL design code to netlist. Yosys is the tool I've used in this workshop.
Here is the flow of above processess.

![rtl-netlist](https://user-images.githubusercontent.com/104454253/166097298-41d913ee-640d-4e1e-9e70-5bf427f35ef4.JPG)

**Yosys**:Yosys is a framework for RTL synthesis and more. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains. Yosys is the core component of most our implementation and verification flows.

I was given an overview of the operation of the tool and the files we'll need to provide the tool to give the required netlist. We give RTL design code, .lib file which has all the building blocks of the netlist. Using these two files, Yosys synthesizer generates a netlist file. .lib basically is a collection of logical modules like, And, Or, Not etc.... These are equivalent gate level representation of the RTL code. 

Below are the commands to perform above synthesis.

- RTL Design  - read_verilog
- .lib        - read_liberty
- netlist file- write_verilog

**Operational flow of Yosys Synthesizer**

![Synthesizer](https://user-images.githubusercontent.com/104454253/166094901-27c70c0d-8ef2-4a34-a4b2-7307af492698.JPG)

**Verification of Synthesized design**: In order to make sure that there are no errors in the netlist, we'll have to verify the synthesized circuit. The netlist verification flow can be seen in the below image:

![Synthesisgtkwave](https://user-images.githubusercontent.com/104454253/166095185-f82dbbe0-afb4-43ac-8ec6-6b75491d6b58.JPG)

The gtkwave output for the netlist should match the output waveform for the RTL design file. As netlist and design code have same set of inputs and outputs, we can use the same testbench and compare the waveforms.

**Introduction to loigc synthesis**: Below is the snippet RTL code and equivalent digital circuit:

![sample rtl](https://user-images.githubusercontent.com/104454253/166097112-0fb5685c-fe88-4ca0-8ecf-bc014de46088.JPG)

In the above image, mapping of code and digital circuit is done using Synthesis.

**.lib**: It is a collection of logical modules like, And, Or, Not etc...It has different flvors of same gate like 2 input AND gate, 3 input AND gate etc... with different performace speed.

**Need for different flavours of gate**: In order to make a faster circuit, the clock frequency should be high. For that the time period of the clock should be as low as possible. However, in a sequential circuit, clock period depends on three factors so that data is not lost or to be glitch free.

For the below circuit the three factors are
- Clock to Q of flipflop A
- Propagation delay of combinational circuit
- Setuptime of flipflop B
![Timedelay circuit](https://user-images.githubusercontent.com/104454253/166098730-33bf0734-abec-466f-abe2-a2ac6813b5e0.JPG)

The equation is as follows

![Time](https://user-images.githubusercontent.com/104454253/166097710-2c1099e3-6323-496c-8eb7-12ee04c12096.JPG)

As per the above equation, for a smaller propagation delay, we need faster cells.
But again, why do we have faster cells? This is to ensure that there are no HOLD time violations at B flipflop.
**This complete collection forms .lib**

**Faster Cells vs Slower Cells**: 
Load in digital circuit is of **Capacitence**. Faster the charging or dicharging of capacitance, lesser is the celll delay. However, for a quick charge/ discharge of capacitor, we need transistors capable of sourcing more current i.e, we need WIDE TRANSISTORS. 

Wider transistors have lesser delay but consume more area and power. Narrow transistors are other way around. Faster cells come with a cost of area and power.

**Selection of the Cells**: We'll need to guide the Synthesizer to choose the flavour of cells that is optimum for implementation of logic circuit. Keeping in view of previous observations of faster vs slower cells,to avoid hold time violations, larger circuits, sluggish circuits, we offer guidance to synthesizer in the form of **Constraints**.

Below is an illustration of Synthesis.

![Screenshot (44)](https://user-images.githubusercontent.com/104454253/166099264-e3842e91-1a27-44ae-830c-0757dc5b1a5e.png)

### 2.3.1 Labs on Yosys introduction

Invoking Yosys:

<img width="913" alt="Screenshot 2022-08-06 at 5 56 46 PM" src="https://user-images.githubusercontent.com/110079631/183714733-22038bb7-602f-472e-a85e-85222c8d26ee.png">


Snippet below illustrates reading .lib, design and choosing the module to synthesize:

<img width="554" alt="Screenshot 2022-08-06 at 6 04 43 PM" src="https://user-images.githubusercontent.com/110079631/183715173-27ac42f2-31f1-4dbd-9fe9-55e5c666d98a.png">

**Generating Netlist**: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file.

<img width="724" alt="Screenshot 2022-08-06 at 6 06 26 PM" src="https://user-images.githubusercontent.com/110079631/183715256-e3261f29-e809-4bb0-a943-de8dd968c387.png">

Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.

<img width="609" alt="Screenshot 2022-08-06 at 6 41 36 PM" src="https://user-images.githubusercontent.com/110079631/183715312-fa950e03-25e4-4c0c-9309-34873636ec60.png">

**Netlist Code**

<img width="1413" alt="Screenshot 2022-08-06 at 6 48 31 PM" src="https://user-images.githubusercontent.com/110079631/183715401-5726f0c3-67f1-4218-964c-f4b4feea28ea.png">

**Simplified netlist code**: This code consisits of additional switch. To further simplify, we use below command

<img width="787" alt="Screenshot 2022-08-06 at 6 51 25 PM" src="https://user-images.githubusercontent.com/110079631/183716581-1785f0cf-07f1-4fdc-8ad4-2a49234a0d7c.png">


# 3. DAY2-Timing libs, hierarchical, flat synthesis, efficient flop coding styles

## 3.1 Introduction to timing .libs
### 3.1.1 LAB- Introduction to dot Lib

This lab guides us through the .lib files where we have all the gates coded in. According to the below parameters the libraries will be characterized to model the variations.

![lib1](https://user-images.githubusercontent.com/104454253/166105787-19a638a3-fe01-4fcf-828d-0b56a6acb8f7.JPG)

With in the lib file, the gates are delared as follows to meet the variations due to process, temperatures and voltages.

<img width="764" alt="Screenshot 2022-08-06 at 6 56 52 PM" src="https://user-images.githubusercontent.com/110079631/183716748-15b55941-1b49-4c34-8f74-ee887f7ed839.png">

For the above example, for all the 32 cominations i.e 2^5 (5 is no.of variables), the delay, power and all the related parameters for each gate are mentioned.

<img width="798" alt="Screenshot 2022-08-06 at 7 04 11 PM" src="https://user-images.githubusercontent.com/110079631/183717335-cd1333f4-1d68-4c34-9e0c-858ebe45eb7c.png">

This image displays the power consumtion comparision.

![lib5](https://user-images.githubusercontent.com/104454253/166107259-6fa398a4-2099-4da3-9b93-818c2c3f2404.JPG)

Below image is the delay order for the different flavor of gates.

![delay_libraries](https://user-images.githubusercontent.com/104454253/166187423-d21465e1-abc3-4ad0-a534-60f8e706ab6f.JPG)

## 3.2 LAB- Hierarchical synthesis and flat synthesis

**multiple_module**<br />

	module sub_module2 (input a, input b, output y);
		assign y = a | b;
	endmodule
	
	module sub_module1 (input a, input b, output y);
		assign y = a&b;
	endmodule


	module multiple_modules (input a, input b, input c , output y);
	wire net1;
	sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
	sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
	endmodule

This is the schematic as per the connections in the above module.

![94d6174a-bcb0-4cad-bef6-a0537ef32ef1](https://user-images.githubusercontent.com/104454253/166108402-f96b7fa9-3d8f-4f05-b4cb-89e443e43718.jpg)

However, the yosys synthesizer generates the following schematic instead of the above one and with in the submodules, the connections are made

<img width="1180" alt="Screenshot 2022-08-06 at 7 14 58 PM" src="https://user-images.githubusercontent.com/110079631/183717549-e638a86e-54cb-43f1-8b23-62d0d67643df.png">

The synthesizer considers the module hierarcy and does the mapping accordting to instantiation. Here is the hierarchical netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
		  input a;
 		 input b;
 		 input c;
		  wire net1;
 		 output y;
 	  sub_module1 u1 (.a(a),.b(b),.y(net1) );
	  sub_module2 u2 (.a(net1),.b(c),.y(y));
	endmodule
	
	module sub_module1(a, b, y);
 	 wire _0_;
 	 wire _1_;
 	 wire _2_;
 	 input a;
 	 input b;
 	 output y;
 	 sky130_fd_sc_hd__and2_0 _3_ (.A(_1_),.B(_0_),.X(_2_));
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

	module sub_module2(a, b, y);
  	wire _0_;
 	 wire _1_;
 	 wire _2_;
  	input a;
  	input b;
 	 output y;
 	 sky130_fd_sc_hd__lpflow_inputiso1p_1 _3_ (.A(_1_),.SLEEP(_0_),.X(_2_) );
 	 assign _1_ = b;
 	 assign _0_ = a;
 	 assign y = _2_;
	endmodule

Flattened netlist:

In flattened netlist, the hierarcies are flattend out and there is single module i.e, gates are instantiated directly instead of sub_modules. Here is the flattened netlist code for the  multiple_modules:

	module multiple_modules(a, b, c, y);
 		 wire _0_;
  		 wire _1_;
 		 wire _2_;
 		 wire _3_;
		 wire _4_;
		 wire _5_;
 		 input a;
 		 input b;
 		 input c;
 		 wire net1;
 		 wire \u1.a ;
		 wire \u1.b ;
		 wire \u1.y ;
		 wire \u2.a ;
		 wire \u2.b ;
 		 wire \u2.y ;
  		output y;
 		 sky130_fd_sc_hd__and2_0 _6_ (
  		  .A(_1_),
  		 .B(_0_),
   		 .X(_2_)
  		);
 		 sky130_fd_sc_hd__lpflow_inputiso1p_1 _7_ (
  		  .A(_4_),
 		  .SLEEP(_3_),
  		  .X(_5_)
 		 );
 		 assign _4_ = \u2.b ;
 		 assign _3_ = \u2.a ;
 		 assign \u2.y  = _5_;
 		 assign \u2.a  = net1;
		 assign \u2.b  = c;
 		 assign y = \u2.y ;
		 assign _1_ = \u1.b ;
		 assign _0_ = \u1.a ;
		 assign \u1.y  = _2_;
		 assign \u1.a  = a;
		 assign \u1.b  = b;
 		 assign net1 = \u1.y ;
		endmodule

The commands to get the hierarchical and flattened netlists is shown below:

**yosys> write_verilog -noattr multiple_modules_hier.v**

8. Executing Verilog backend.
Dumping module `\multiple_modules'.
Dumping module `\sub_module1'.
Dumping module `\sub_module2'.

**yosys> !gvim multiple_modules_hier.v**

11. Shell command: gvim multiple_modules_hier.v

**yosys> flatten**

12. Executing FLATTEN pass (flatten design).
Deleting now unused module sub_module1.
Deleting now unused module sub_module2.
<suppressed ~2 debug messages>

**yosys> write_verilog -noattr multiple_modules_flat.v**

13. Executing Verilog backend.
Dumping module `\multiple_modules'.

**yosys> !gvim multiple_modules_flat.v**

14. Shell command: gvim multiple_modules_flat.v

This is the synthyesized circuit for a flattened netlist. Here u1 and u2 are flattened and directly or gates are realized.

![lab51](https://user-images.githubusercontent.com/104454253/166112988-1b02eea6-a6e8-4a7e-8eee-0772182f914f.JPG)

Here is the synthesized circuit of sub_module1. We are also generating module level synthesis so that if there is a top module with multiple and same sub_modules, we can synthesize it once and can use and connect the same netlist multiple times in the top module netlist.

Another reason to generate module level synthesis and then stictch them together is to avoid errors in a top module if its massive and consists of several sub modules. Generating netlist for synthesis and then stiching it together in top level becomes easier and reduces risk of output mismatch.

We control this synthesis using **synth -top <module_name>** command

![lab52](https://user-images.githubusercontent.com/104454253/166113791-5c245c1c-727a-4f15-aaec-9fef1b817aec.JPG)

### 3.3 Various Flop coding styles and optimization

**Why Flops and Flop coding styles part1**

In this session, the discussion was about how to code various types of flops and various styles of coding a flop.

**Why a Flop?**

 In a combinational circuit, the output changes after the propagation delay of the circuit once inputs are changed. During the propagation of data, if there are different paths with different propagation delays, there might be a chance of getting a glitch at the output.<br />
 If there are multiple combinational circuits in the design, the occurances of glitches are more thereby making the output unstable.<br />
To curb this drawback, we are going for flops to store the data from the cominational circuits. When a flop is used, the output of combinational circuit is stored in     it and it is propagated only at the posedge or negedge of the clock so that the next combinational circuit gets a glitch free input thereby stabilising the output.
 
 We use initialize signals or control pins called **set** and **reset** on a flop to initialize the flop, other wise a garbage value to sent out to the next combinational circuit. These control pins can be synchronous or asynchronous.
 
### 3.3.1 Lab- flop synthesis simulations
 
 **d-flipflop with asynchronous reset**- Here the output **q** goes low whenever reset is high and will not wait for the clock's posedge, i.e irrespective of clock, the output is changed to low.
 
 ![f143804e-5d1a-49d1-9b00-9227785d3e29](https://user-images.githubusercontent.com/104454253/166116145-8fbbacb1-e453-465a-9e41-f21de2337190.jpg)<br />
 
	 module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
		always @ (posedge clk , posedge async_reset)
		begin
			if(async_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

<img width="995" alt="Screenshot 2022-08-06 at 7 18 36 PM" src="https://user-images.githubusercontent.com/110079631/183717952-f1eb47f1-d9d7-40e6-938b-e2ee922e9730.png">

**Synthesized circuit**:

<img width="1176" alt="Screenshot 2022-08-06 at 7 22 10 PM" src="https://user-images.githubusercontent.com/110079631/183718024-ba64e372-f435-4fbd-91d0-c052696fc50b.png">

**d-flipflop with synchronous reset**- Here the output **q** goes low whenever reset is high and at the positive edge of the clock. Here the reset of the output depends on the clock.

![433468fc-7853-49be-8e81-45fd23297c6c](https://user-images.githubusercontent.com/104454253/166116449-152fffef-c1f5-492b-9101-9a0d431b8a88.jpg)

	module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
		always @ (posedge clk )
		begin
			if (sync_reset)
				q <= 1'b0;
			else	
				q <= d;
		end
	endmodule

**Simulation**:

<img width="1000" alt="Screenshot 2022-08-06 at 7 25 15 PM" src="https://user-images.githubusercontent.com/110079631/183719770-547f9680-2044-43cb-bb8c-ae7ac36df8be.png">

**Synthesized circuit**:

<img width="1184" alt="Screenshot 2022-08-06 at 7 27 40 PM" src="https://user-images.githubusercontent.com/110079631/183719848-441b6ec5-758f-43ac-8bd4-1c78d0c86c87.png">


### 3.3.2 Interesting optimisations

This lab session deals with some automatic and interesting optimisations of the circuits based on logic. In the below example, multiplying a number with 2 doesn't need any additional hardeware and only needs connecting the bits from **a** to **y** and grounding the LSB bit of y is enough and the same is realized by Yosys.

	module mul2 (input [2:0] a, output [3:0] y);
		assign y = a * 2;
	endmodule

![8e686520-9e94-4c61-b8cf-6ac376a519c9](https://user-images.githubusercontent.com/104454253/166120664-44f5cd53-02bb-4457-bdb4-e8e02f0f64e4.jpg)

**Synthesized circuit**:

<img width="1180" alt="Screenshot 2022-08-06 at 9 08 04 PM" src="https://user-images.githubusercontent.com/110079631/183795537-6d05e166-b7c4-4f0b-9a8f-e47fbdd43a99.png">

When it comes to multiplying with powers of 2, it just needs shifting as shown in the below image:

![1781ff88-add9-4b73-b8ec-51257e3074f2](https://user-images.githubusercontent.com/104454253/166120876-1cbc110d-2760-4ab3-9199-7aae4ba2dffb.jpg)

**Netlist for the above schematic**

<img width="864" alt="Screenshot 2022-08-06 at 9 10 16 PM" src="https://user-images.githubusercontent.com/110079631/183795899-f28fcd38-2bfd-42bf-87e7-6d2abadc2222.png">

Special case of multiplying **a** with **9**. The result is shown in the below image:

<img width="1180" alt="Screenshot 2022-08-06 at 9 12 28 PM" src="https://user-images.githubusercontent.com/110079631/183802740-f813a972-474e-4b55-8869-8f95f5a1794d.png">


The schematic for the same is shown below:

<img width="800" alt="Screenshot 2022-08-06 at 9 14 24 PM" src="https://user-images.githubusercontent.com/110079631/183802853-fbef3ec1-6abd-4b3a-bdc4-fc2de864cee9.png">

**Netlist for the above schematic**


# 4. Day3- Combinational and sequential optmizations

## 4.1 Combinational logic optimization with examples

Optimising the combinational logic circuit is squeezing the logic to get the most optimized digital design so that the circuit finally is area and power efficient. This is achieved by the synthesis tool using various techniques and gives us the most optimized circuit.

**Techniques for optimization**:
- Constant propagation which is Direct optimizxation technique
- Boolean logic optimization using K-map or Quine McKluskey

Here is an example for **Constant Propagation**

![optimizations1](https://user-images.githubusercontent.com/104454253/166127772-9ff3dc8e-c5e2-4621-8070-d300df31667e.JPG)

In the above example, if we considor the trasnsistor level circuit of output Y, it has 6 MOS trasistors and when it comes to invertor, only 2 transistors will be sufficient. This is achieved by making A as contstant and propagating the same to output.

Example for **Boolean logic optimization**:

Let's consider an example concurrent statement **assign y=a?(b?c:(c?a:0)):(!c)**

The above expression is using a ternary operator which realizes a series of multiplexers, however, when we write the boolean expression at outputs of each mux and simplify them further using boolean reduction techniques, the outout **y** turns out be just **~(a^c)**

Command to optimize the circuit by yosys is **yosys> opt_clean -purge**

**Example-1**

![62695d29-f76d-426c-ab99-af6fbb2abda0](https://user-images.githubusercontent.com/104454253/166292324-f3243d68-55c1-4829-a836-8177edc79613.jpg)

	module opt_check (input a , input b , output y);
		assign y = a?b:0;
	endmodule

**Optimized circuit**

<img width="1183" alt="Screenshot 2022-08-06 at 9 17 07 PM" src="https://user-images.githubusercontent.com/110079631/183802971-7a62c496-1d76-44ce-83ae-da06d87b4766.png">

**Example-2**

![e748028c-ea3d-4d58-8f5a-71a8248e4dd5](https://user-images.githubusercontent.com/104454253/166292286-6b3fd349-23af-463e-988e-863038a542d8.jpg)

	module opt_check2 (input a , input b , output y);
		assign y = a?1:b;
	endmodule

<img width="1182" alt="Screenshot 2022-08-06 at 9 19 02 PM" src="https://user-images.githubusercontent.com/110079631/183803083-a4b99815-4b49-43d3-b4ce-3fa6236fe9ef.png">

**Example-3**

![bfcc0b60-1b3e-4f45-a5cf-88b33f4e7dcf](https://user-images.githubusercontent.com/104454253/166292243-7fa03ef2-bce9-418f-8830-e9587d459aef.jpg)

	module opt_check3 (input a , input b, input c , output y);
		assign y = a?(c?b:0):0;
	endmodule

<img width="1179" alt="Screenshot 2022-08-06 at 9 20 51 PM" src="https://user-images.githubusercontent.com/110079631/183803184-1c535af9-2d0e-41e6-a020-d5b46da34de5.png">

**Example-4**

![a42e5eb3-2966-4fb0-bad3-6c6a6fa12798](https://user-images.githubusercontent.com/104454253/166292378-3ebdc824-de41-4385-9f7f-1654e71c0ff0.jpg)

	module opt_check4 (input a , input b , input c , output y);
		assign y = a?(b?(a & c ):c):(!c);
	endmodule
 
<img width="1178" alt="Screenshot 2022-08-06 at 9 24 30 PM" src="https://user-images.githubusercontent.com/110079631/183803328-e6ef18fe-bec3-4bf1-b323-046a1a0990e5.png">

**Example- 5**

![7c0faa7e-cffc-44f3-988a-17e2e4857675](https://user-images.githubusercontent.com/104454253/166292469-dea41b7c-40ed-4b5b-b9f7-30ad3de0bad2.jpg)

	module sub_module(input a , input b , output y);
		assign y = a & b;
	endmodule

	module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
		sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
		sub_module U2 (.a(b), .b(c) , .y(n2));
		sub_module U3 (.a(n2), .b(d) , .y(n3));
		sub_module U4 (.a(n3), .b(n1) , .y(y));
	endmodule

<img width="1180" alt="Screenshot 2022-08-06 at 9 28 51 PM" src="https://user-images.githubusercontent.com/110079631/183803595-867c50d7-63da-4388-b09d-a3fb06d29689.png">

**Example-6**

![89cc6397-defe-4bba-a767-6e28c99ba0de](https://user-images.githubusercontent.com/104454253/166292486-d2041066-ab26-4010-b253-93c510c51674.jpg)

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

<img width="1181" alt="Screenshot 2022-08-06 at 9 31 59 PM" src="https://user-images.githubusercontent.com/110079631/183803833-e63bb071-69bb-4516-b1f3-e5e168f9a8ff.png">

### 4.2 Sequential Logic Optimization with examples

Below are the various techniques used for sequential logic optimisations:<br />
-Basic
  - Sequential contant propagation
- Advanced
  - State optimisation
  - Retiming
  - Sequential Logic Cloning (Floor Plan Aware Synthesis)
 
#### 4.2.1 Basic

**Sequential contant propagation**- Here only the first logic can be optimized as the output of flop is always zero. However for the second flop, the output changes continuously, therefor it cannot be used for contant propagation.

![1e34f504-64e1-411d-a668-5294c6ea78f5](https://user-images.githubusercontent.com/104454253/166128292-7faf6384-792a-4455-a847-350bb95b631f.jpg)

![6ef09bec-38c3-4a13-901b-0221461c9aca](https://user-images.githubusercontent.com/104454253/166128295-40ddcdda-9b4e-4a0f-a27e-5c5bd9a8bb3e.jpg)

#### 4.2.2. Advanced
**State Optimisation**: This is optimisation of unused state. Using this technique we can come up with most optimised state machine.

**Cloning**: This is done when performing PHYSICAL AWARE SYNTHESIS. Lets consider a flop A which is connected to flop B and flop C through a combination logic. If B and C are placed far from A in the flooerplan, there is a routing path delay. To avoid this, we connect A to two intermediate flops and then from these flops the output is sent to B and C thereby decreasing the delay. This process is called cloning since we are generating two new flops with same functionality as A.

**Retiming**: Retiming is a powerful sequential optimization technique used to move registers across the combinational logic or to optimize the number of registers to improve performance via power-delay trade-off, without changing the input-output behavior of the circuit. 

**Example-1**<br />
Here flop will be inferred as the output is not constant. <br />

	module dff_const1(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b0;
			else
				q <= 1'b1;
		end
	endmodule

![bdf6fb4e-ee93-4f76-bbef-e25a8fe8aeda](https://user-images.githubusercontent.com/104454253/166292590-1d76f35e-f83f-486e-a154-1e0a7a1441fb.jpg)

**Simulation**

<img width="1000" alt="Screenshot 2022-08-06 at 9 35 11 PM" src="https://user-images.githubusercontent.com/110079631/183804067-d2950c4e-5e63-4e20-905f-75378a99e4d4.png">

**Synthesis**<br />
In the synthesis report, we'll see that a Dflop was inferred in this example.

<img width="1180" alt="Screenshot 2022-08-06 at 9 37 26 PM" src="https://user-images.githubusercontent.com/110079631/183804216-eccd84c7-4056-41e1-8698-09b660079af1.png">

<img width="570" alt="Screenshot 2022-08-06 at 9 40 24 PM" src="https://user-images.githubusercontent.com/110079631/183804279-029377f6-f35e-4680-9e68-ad491deb140b.png">

**Example-2**<br />
Here flop will not be inferred as the output is always high. <br />

	module dff_const2(input clk, input reset, output reg q);
		always @(posedge clk, posedge reset)
		begin
			if(reset)
				q <= 1'b1;
			else
				q <= 1'b1;
		end
	endmodule

![7a01ae65-de9b-4e33-812e-206eb3c2733f](https://user-images.githubusercontent.com/104454253/166292615-3af99c64-305e-434a-b21b-907a30f83ab0.jpg)

**Simulation**

<img width="992" alt="Screenshot 2022-08-06 at 10 18 01 PM" src="https://user-images.githubusercontent.com/110079631/183804427-05b4f272-b44b-414f-88a2-70cc47a65886.png">

**Synthesis**

<img width="1181" alt="Screenshot 2022-08-06 at 9 42 11 PM" src="https://user-images.githubusercontent.com/110079631/183804568-27b6c41e-d151-4820-9b13-8ac604c87c8b.png">

**Example-3**

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


**Simulation***

<img width="1004" alt="Screenshot 2022-08-06 at 10 21 04 PM" src="https://user-images.githubusercontent.com/110079631/183804833-f04763ae-ab2f-46b3-b744-58aff049a9e2.png">

**Synthesis**

<img width="1178" alt="Screenshot 2022-08-06 at 10 22 07 PM" src="https://user-images.githubusercontent.com/110079631/183804910-93f05371-9b7f-4de3-936d-465e8ea75319.png">

**Example4**

		module dff_const4(input clk, input reset, output reg q);
		reg q1;

		always @(posedge clk, posedge reset)
		begin
			if(reset)
			begin
				q <= 1'b1;
				q1 <= 1'b1;
			end
		else
			begin
				q1 <= 1'b1;
				q <= q1;
			end
		end
		endmodule

**Simulation***

<img width="1003" alt="Screenshot 2022-08-06 at 10 23 38 PM" src="https://user-images.githubusercontent.com/110079631/183805060-c6845883-92fa-4724-a46f-79cc74f3ec20.png">

**Synthesis**

<img width="1182" alt="Screenshot 2022-08-06 at 10 24 22 PM" src="https://user-images.githubusercontent.com/110079631/183805110-b5292930-fc22-48be-bbf6-04e92066aa41.png">

**Example5**

		module dff_const5(input clk, input reset, output reg q);
		reg q1;
		always @(posedge clk, posedge reset)
			begin
				if(reset)
				begin
					q <= 1'b0;
					q1 <= 1'b0;
				end
			else
				begin
					q1 <= 1'b1;
					q <= q1;
				end
			end
		endmodule

**Simulation***

<img width="1000" alt="Screenshot 2022-08-06 at 10 25 34 PM" src="https://user-images.githubusercontent.com/110079631/183805190-1177a369-4860-4fd7-a5d1-052818c99bd4.png">

**Synthesis**

<img width="1183" alt="Screenshot 2022-08-06 at 10 26 20 PM" src="https://user-images.githubusercontent.com/110079631/183805240-90052542-139c-45a5-bbc6-9c2dea162622.png">

### 4.2.3 Sequential optimisation of unused outputs
**Example1**

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
		
![2f8f6e43-102f-4fd7-aad2-585b0a55098d](https://user-images.githubusercontent.com/104454253/166292769-44c0406c-4fbb-4a3e-9345-2b308783a7de.jpg)

**Synthesis**

<img width="1179" alt="Screenshot 2022-08-06 at 10 29 35 PM" src="https://user-images.githubusercontent.com/110079631/183805419-4e30ced7-39c6-4675-9b06-385dccc9e889.png">

**Updated counter logic-** 

	module counter_opt (input clk , input reset , output q);
		reg [2:0] count;
		assign q = {count[2:0]==3'b100};
		always @(posedge clk ,posedge reset)
		begin
		if(reset)
			count <= 3'b000;
		else
			count <= count + 1;
		end
	endmodule

![0e890967-cf43-4be0-8f53-968fe98c7f55](https://user-images.githubusercontent.com/104454253/166292799-97d7ee9c-1000-4dcc-935c-a5ae56a74f8e.jpg)

**Synthesis**

All the other blocks in synthesizer are for incrementing the counter but the output is only from the three input NOR gate.

<img width="1398" alt="Screenshot 2022-08-06 at 10 31 37 PM" src="https://user-images.githubusercontent.com/110079631/183805542-b467f221-c9cc-4632-8ac7-33a84d628cbe.png">

# 5. DAY4- GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

# 5.1 GLS, Synthesis-Simulation mismatch and Blocking, Non-blocking statements
### 5.1.1 GLS Concepts And Flow Using Iverilog

**What is GLS- Gate Level Simulation?**:<br />
GLS is generating the simulation output by running test bench with netlist file generated from synthesis as design under test. Netlist is logically same as RTL code, therefore, same test bench can be used for it.

**Why GLS?**:<br />
We perform this to verify logical correctness of the design after synthesizing it. Also ensuring the timing of the design is met.

Below picture gives an insight of the procedure. Here while using iverilog, we also include gate level verilog models to generate GLS simulation.

![Screenshot (49)](https://user-images.githubusercontent.com/104454253/166256679-1ac9167a-1358-4c60-bbdb-0f6423f0faa3.png)

### 5.1.2 Synthesis Simulation Mismatch

There are three main reasons for Synthesis Simulation Mismatch:<br />
- Missing sensitivity list in always block
- Blocking vs Non-Blocking Assignments
- Non standard Verilog coding

**Missing sensitivity list in always block:**<br />

If the consider - Example-2, we can see the only **sel** is mentioned in the sensitivity list. During the simulation, the waveforms will resemble a latched output but the simulation of netlist will not infer this as the synthesizer will only look at the statements with in the procedural block and not the sensitivity list.

As the synthesizer doen't look for sensitivity list and it looks only for the statements in procedural block, it infers correct  circuit  and if we simulate the netlist code, there will be a synthesis simulation mismatch.

To avoid the synthesis and simulation mismatch. It is very important to check the behaviour of the circuit first and then match it with the expected output seen in simulation and make sure there are no synthesis and simulation mismatches. This is why we use GLS.

**Blocking vs Non-Blocking Assignments**:

Blocking statements execute the statemetns in the order they are written inside the always block. Non-Blocking statements execute all the RHS and once always block is entered, the values are assigned to LHS. This will give mismatch as sometimes, improper use of blocking statements can create latches. Please click here to go to example - Example4

## 5.2 Lab- GLS Synth Sim Mismatch

**Example-1**

	module ternary_operator_mux (input i0 , input i1 , input sel , output y);
		assign y = sel?i1:i0;
	endmodule
	
**Simulation**

<img width="1002" alt="Screenshot 2022-08-06 at 10 33 37 PM" src="https://user-images.githubusercontent.com/110079631/183805723-51c6e2c7-df9e-4631-999b-74954e4e50ce.png">

**Synthesis**

<img width="1399" alt="Screenshot 2022-08-06 at 10 35 00 PM" src="https://user-images.githubusercontent.com/110079631/183805807-d08a9cc3-d080-4949-9521-86243fd32d08.png">

**Netlist Simulation**

<img width="1003" alt="Screenshot 2022-08-06 at 10 40 37 PM" src="https://user-images.githubusercontent.com/110079631/183805917-34f7d8c8-eced-4f8c-b5de-87934e4734d7.png">

# Example-2

	module bad_mux (input i0 , input i1 , input sel , output reg y);
		always @ (sel)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule

**Simulation**

<img width="999" alt="Screenshot 2022-08-07 at 9 12 16 AM" src="https://user-images.githubusercontent.com/110079631/183806030-4aa5698b-7648-4416-bf62-76ff52e77ad6.png">

**Synthesis**

<img width="1057" alt="Screenshot 2022-08-07 at 9 16 27 AM" src="https://user-images.githubusercontent.com/110079631/183806096-5e7d59d1-4e13-4dc8-8710-65c0dd5bc542.png">

**Netlist Simulation**

<img width="998" alt="Screenshot 2022-08-07 at 9 29 57 AM" src="https://user-images.githubusercontent.com/110079631/183806673-80bc1afe-2722-45ff-9408-5e54d06afd50.png">

**MISMATCH**<br />

![Capture](https://user-images.githubusercontent.com/104454253/166259062-c3cb7fb2-e28a-4e12-b2d6-83812c5f2582.JPG)

**Example-3**

	module good_mux (input i0 , input i1 , input sel , output reg y);
		always @ (*)
		begin
			if(sel)
				y <= i1;
			else 
				y <= i0;
		end
	endmodule
	
**Simulation**

<img width="1002" alt="Screenshot 2022-08-07 at 9 31 35 AM" src="https://user-images.githubusercontent.com/110079631/183806929-89fc6709-6e3e-4f88-b208-bb842bf0fff6.png">

**Synthesis**

<img width="1061" alt="Screenshot 2022-08-07 at 9 32 54 AM" src="https://user-images.githubusercontent.com/110079631/183806957-1d9c28bc-1b62-46f0-8065-5741b3b286d4.png">

**Netlist Simulation**

<img width="1000" alt="Screenshot 2022-08-07 at 9 34 31 AM" src="https://user-images.githubusercontent.com/110079631/183807009-29021abc-e6fe-48a8-9ba6-a01488b1e0d1.png">

## 5.3 Lab- Synthesis simulation mismatch blocking statement

Here the output is depending on the past value of x which is dependednt on a and b and it appears like a flop.

# Example4

	module blocking_caveat (input a , input b , input  c, output reg d); 
	reg x;
	always @ (*)
		begin
		d = x & c;
		x = a | b;
	end
	endmodule

**Simulation**

<img width="1000" alt="Screenshot 2022-08-07 at 9 39 20 AM" src="https://user-images.githubusercontent.com/110079631/183808184-85628461-af14-4e38-867c-69a3afeccb11.png">

**Synthesis**

<img width="1063" alt="Screenshot 2022-08-07 at 9 54 21 AM" src="https://user-images.githubusercontent.com/110079631/183808213-c08ee22d-610b-4047-b718-0002a0f66ea1.png">

**Netlist Simulation**

<img width="1001" alt="Screenshot 2022-08-07 at 9 56 48 AM" src="https://user-images.githubusercontent.com/110079631/183808227-baf932ae-553a-4c14-a212-2856d69e4739.png">

**MISMATCH**

![Capture2](https://user-images.githubusercontent.com/104454253/166260812-1832a4ba-8f95-4de3-8129-6b6c39ed17ce.JPG)

# 6. DAY5- if, case, for loop and for generate

## 6.1 If and Case constructs

### 6.1.1 If construct
The construct **if** is mainly used to create priority logic. In a nested if else construct, the conditions are given priority from top to bottom. Only if the condition is satisfied, if statement is executed and the compiler comes out of the block. If condition fails, it checks for next condition and so on as shown below.

**Syntax for nested if else**

	if (<condition 1>)
	begin
	-----------
	-----------
	end
	else if (<condition 2>)
	begin
	-----------
	-----------
	end
	else if (<condition 3>)
	.
	.
	.
	
**Dangers with IF**:

If use a bad coding style i.e, using incomplete if else constructs will infer a latch. We definetly don't require an unwanted latch in a combinational circuit.
When an incomplete construct is used, if all the conditions are failed, the input is latched to the output and hence we don't get desired output unless we need a latch.

This can be shown in below example:

![0ed60c6c-b272-4bc6-939b-df13f76fc063](https://user-images.githubusercontent.com/104454253/166262011-21800dd6-0427-4558-a75e-6bf928117bba.jpg)

### 6.1.2 Case construct

**Syntax**

	case(statement)
	  case1: begin
  	       --------
		 --------
		 end
 	 case2: begin
    	     --------
		 --------
		 end
 	 default:
	 endcase
 
 In case construct, the execution checks for all the case statements and whichever satisfies the statement, that particular statement is executed.If there is no match, the default statement is executed. But here unlike if construct, the execution doesn't stop once statement is satisfied, but it continues further.
 
**Caveats in Case**<br />
Caveats in case occur due to two reasons. One is **incomplete case statements** and the other is **partial assignments in case statements.**

## 6.2 Lab- Incomplete IF

This incomplete if construct forms a connection between i0 and output y i.e, D-latch with input as i1 and i0 will be the enable for it.<br />
**Example-1**

	module incomp_if (input i0 , input i1 , input i2 , output reg y);
	always @ (*)
	begin
		if(i0)
			y <= i1;
	end
	endmodule

**Simulation**

<img width="999" alt="Screenshot 2022-08-07 at 9 58 17 AM" src="https://user-images.githubusercontent.com/110079631/183808269-22f2e9fc-4f21-4ce6-a9d0-916b32ba38d1.png">


**Synthesis**

<img width="1058" alt="Screenshot 2022-08-07 at 9 59 19 AM" src="https://user-images.githubusercontent.com/110079631/183808307-8745b19f-5e06-4784-a259-d5e49e11cd07.png">

**Example-2**<br />
The below code is equivalent to two 2:1 mux with i0 and i2 as select lines with i1 and i3 as inputs respectively. Here as well, the output is connected back to input in the form of a latch with an enable input of **OR** of i0 and i2.

	module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
		always @ (*)
		begin
			if(i0)
				y <= i1;
			else if (i2)
				y <= i3;
		end
	endmodule

**Simulation**

<img width="998" alt="Screenshot 2022-08-07 at 10 00 55 AM" src="https://user-images.githubusercontent.com/110079631/183808354-ecbca8ab-73c2-4cf0-9ef3-c56b7ee355e9.png">

**Synthesis**

<img width="1063" alt="Screenshot 2022-08-07 at 10 01 46 AM" src="https://user-images.githubusercontent.com/110079631/183808387-66623335-6dd8-4fe2-9428-145a98c8c580.png">

## 6.3 Lab- incomplete overlapping Case

**Example-1**<br />
Thie is an example of incomplete case where other two combinations 10 and 11 were not included. This is infer a latch for the multiplexer and connect i2 and i3 with the output.

	module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
		always @ (*)
		begin
		case(sel)
			2'b00 : y = i0;
			2'b01 : y = i1;
		endcase
		end
	endmodule

**Simulator**

<img width="998" alt="Screenshot 2022-08-07 at 10 03 17 AM" src="https://user-images.githubusercontent.com/110079631/183808425-19af0318-af30-43c8-99a9-f31608703693.png">

**Synthesis**

<img width="1060" alt="Screenshot 2022-08-07 at 10 04 03 AM" src="https://user-images.githubusercontent.com/110079631/183808470-ace9104b-ab36-42c6-85cb-9afe89f615bc.png">

**Example-2- Complete case**

This is the case of complete case statements as the default case is given. If the actual case statements don't execute, the compiler directly executes the default statements and a latch is not inferred.

	module comp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
	always @ (*)
	begin
		case(sel)
			2'b00 : y = i0;
			2'b01 : y = i1;
			default : y = i2;
		endcase
	end
	endmodule

**Simulation**

<img width="998" alt="Screenshot 2022-08-07 at 10 05 14 AM" src="https://user-images.githubusercontent.com/110079631/183808499-98d61de8-a083-411e-8a5e-0477b243391d.png">

**Synthesis**

<img width="1060" alt="Screenshot 2022-08-07 at 10 06 00 AM" src="https://user-images.githubusercontent.com/110079631/183808529-501d1bb6-91f3-4df3-af5e-1b42c0d0b29a.png">

**Example-3**<br />
In the below example, y is present in all the case statements and it had particular outut for all cases. There no latch is inferred in case of y. 
When it comes to x, it is not assigned for the input 01, therefore a latch is inferred here.

	module partial_case_assign (input i0 , input i1 , input i2 , input [1:0] sel, output reg y , output reg x);
	always @ (*)
	begin
		case(sel)
			2'b00 : begin
				y = i0;
				x = i2;
				end
			2'b01 : y = i1;
			default : begin
		         	  x = i1;
				  y = i2;
			 	 end
		endcase
	end
	endmodule

**Simulation**

<img width="1002" alt="Screenshot 2022-08-07 at 10 14 35 AM" src="https://user-images.githubusercontent.com/110079631/183808573-00854950-5605-4eed-b112-8fc8049c5454.png">

**Synthesis**

<img width="1059" alt="Screenshot 2022-08-07 at 10 16 40 AM" src="https://user-images.githubusercontent.com/110079631/183808612-f69aa7c3-25a9-402d-8d88-61705995342b.png">

**Example-4-Bad case construct**

	module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
	always @(*)
	begin
		case(sel)
			2'b00: y = i0;
			2'b01: y = i1;
			2'b10: y = i2;
			2'b1?: y = i3;
			//2'b11: y = i3;
		endcase
	end
	endmodule
	
**Simulation**

<img width="1002" alt="Screenshot 2022-08-07 at 10 18 47 AM" src="https://user-images.githubusercontent.com/110079631/183808652-c564a2b1-277d-446f-9c52-b9a489e469cf.png">

**Synthesis**

<img width="1062" alt="Screenshot 2022-08-07 at 10 19 50 AM" src="https://user-images.githubusercontent.com/110079631/183808685-b1cabff1-bccf-4836-9e27-5ba8c81c726c.png">

**Netlist simulation**

<img width="1001" alt="Screenshot 2022-08-07 at 10 21 24 AM" src="https://user-images.githubusercontent.com/110079631/183808704-778fbd92-6334-48d0-a35d-239bfdbfb1c3.png">

## 6.4 For Loop and For Generate

**For Loop**<br />
- For look is used in always block
- It is used for excecuting expressions alone

**Generate For loop**<br />
- Generate for loop is used for instantaing hardware
- It should be used only outside always block

For loop can be used to generate larger circuits like 256:1 multiplexer or 1-256 demultiplexer where the coding style of smaller mux is not feesible and can have human errors since we would need to include huge number of combinations.

FOR Generate can be used to instantiate any number of sub modules with in a top module. For example, if we need a 32 bit ripple carry adder, instead of instantiating 32 full adders, we can write a generate for loop and connect the full adders appropriately.

## 6.5 Lab- For and For Generate

**Example-1- Mux using generate**<br />
Here for loop is used to design a 4:1 mux. This can also be written using case or if else block, however, for a large size mux, only for loop model is feasible.

	module mux_for (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
		wire [3:0] i_int;
		assign i_int = {i3,i2,i1,i0};
		integer k;
	always @ (*)
		begin
		for(k = 0; k < 4; k=k+1) begin
			if(k == sel)
			y = i_int[k];
			end
		end
	endmodule

**Simulation**

<img width="1003" alt="Screenshot 2022-08-07 at 10 24 11 AM" src="https://user-images.githubusercontent.com/110079631/183808747-cc78fa36-4a76-4071-bcc5-a7d4508ebf3b.png">

**Synthesis**

<img width="1056" alt="Screenshot 2022-08-07 at 10 25 01 AM" src="https://user-images.githubusercontent.com/110079631/183808761-cc6a617f-444f-4b1b-af48-c81644337de1.png">

**Netlist Simulation**

<img width="1002" alt="Screenshot 2022-08-07 at 10 26 31 AM" src="https://user-images.githubusercontent.com/110079631/183808778-8bad12b6-d4db-4ecf-bd9e-e1a8d29da80a.png">

**Example-2-Demux using Case**

	module demux_case (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
	reg [7:0]y_int;
	assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
	integer k;
	always @ (*)
	begin
	y_int = 8'b0;
	case(sel)
		3'b000 : y_int[0] = i;
		3'b001 : y_int[1] = i;
		3'b010 : y_int[2] = i;
		3'b011 : y_int[3] = i;
		3'b100 : y_int[4] = i;
		3'b101 : y_int[5] = i;
		3'b110 : y_int[6] = i;
		3'b111 : y_int[7] = i;
	endcase
	end
	endmodule

**Simulation**

<img width="1001" alt="Screenshot 2022-08-07 at 10 27 50 AM" src="https://user-images.githubusercontent.com/110079631/183808799-f62809dc-d45e-4827-99fb-cf305e71c15f.png">

**Synthesis**

<img width="1061" alt="Screenshot 2022-08-07 at 10 29 06 AM" src="https://user-images.githubusercontent.com/110079631/183808823-65ec9488-49d3-454a-ba13-3d8f9f9cbd9c.png">

**Example-3-Demux using Generate**

The code in above example is big and also there is a chance of human error wile writing the code. However, using for loop as shown below, this drawback can be elimiated to a great extent.

	module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
	reg [7:0]y_int;
	assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
	integer k;
	always @ (*)
	begin
		y_int = 8'b0;
		for(k = 0; k < 8; k++) begin
			if(k == sel)
			y_int[k] = i;
		end
	end
	endmodule

**Simulation**

<img width="1000" alt="Screenshot 2022-08-07 at 10 31 09 AM" src="https://user-images.githubusercontent.com/110079631/183808873-f5157321-c8dd-436c-9986-558ba8218e54.png">

**Synthesis**

<img width="1058" alt="Screenshot 2022-08-07 at 10 32 12 AM" src="https://user-images.githubusercontent.com/110079631/183808890-af672cde-d9ab-406d-89c0-1841727b72d0.png">

**Example-4- Ripple carry adder using fulladder**

In this Ripple carry adder example, unlike instantiating fulladder for 8 times, generate for loop is used to instantiate the fulladder for 7 times and only for first full adder, it is instantiated seperately. Using the same code, just by changing bus sizes and condition of for loop, we can design any required size of ripple carry adder.

	module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
	wire [7:0] int_sum;
	wire [7:0]int_co;

	genvar i;
	generate
		for (i = 1 ; i < 8; i=i+1) begin
			fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
		end

	endgenerate
	fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


	assign sum[7:0] = int_sum;
	assign sum[8] = int_co[7];
	endmodule

	module fa (input a , input b , input c, output co , output sum);
	endmodule

**Simulation**

<img width="1003" alt="Screenshot 2022-08-07 at 10 36 26 AM" src="https://user-images.githubusercontent.com/110079631/183808924-211c77ed-95c7-4f71-9a00-b8826d231a38.png">

**Synthesis**

<img width="1056" alt="Screenshot 2022-08-07 at 10 37 12 AM" src="https://user-images.githubusercontent.com/110079631/183808940-013cd439-84b3-42ce-8d74-551876f9f7bf.png">

**Netlist Simulation**

<img width="1001" alt="Screenshot 2022-08-07 at 10 46 11 AM" src="https://user-images.githubusercontent.com/110079631/183808967-eb627ab1-1d1d-48b4-8f55-d7fa5208a250.png">

# 7. Word of Thanks

My sincere thanks to the team of VLSI System Design for the 5-day workshop. The sessions are very well conducted. The content is very precise and helpful in my understanding of the flow. I very much look forward to any future workshops and events. My special thanks to Mr.kunal and Mr.Shon Taware for helping me out with the flow. I wish the VSD Team all the very best on their future endeavours.

