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




In this session, I've performed simulation of multiplexer. I've added both the RTL design code and test bench code in iverilog to generate vcd file which I used in gtkwave generator to get the output waveformes after simulation. The output was generated by taking the inputs from the testbench code. 



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


Snippet below illustrates reading .lib, design and choosing the module to synthesize:

![yosys1](https://user-images.githubusercontent.com/104454253/166100005-8a2e45e9-2977-4743-b475-515996a046d9.JPG)

**Generating Netlist**: The logic of good_mux will be realizable using gates in the sky130_fd_sc_hd__tt_025C_1v80.lib file.

![yosys2](https://user-images.githubusercontent.com/104454253/166100160-7b8c5847-62b4-49e9-9a90-c4f4c8ff840f.JPG)

Below is the snippet showing the synthesis results and synthesized circuit for multiplexer.


**Netlist Code**


**Simplified netlist code**: This code consisits of additional switch. To further simplify, we use below command

