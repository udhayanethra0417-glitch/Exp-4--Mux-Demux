# Exp-4-Mux-Demux
# Design and Verification of 2:1 Multiplexer and 1:2 Demultiplexer
## Aim
To design and verify a 2:1 Multiplexer and a 1:2 Demultiplexer using Verilog HDL.
## Theory / Synopsis
A 2:1 Multiplexer (MUX) selects one of two data inputs, A or B, and routes it to a single output Y based on the value of the select line S. A 1:2 Demultiplexer (DEMUX) performs the reverse operation: it routes a single data input D to one of two outputs, Y0 or Y1, based on the select line S.
Boolean Functions
### 2:1 Multiplexer:
•	Y = S'.A + S.B
### 1:2 Demultiplexer:
•	Y0 = S'.D
•	Y1 = S.D
## Files to be Created

<img width="575" height="65" alt="image" src="https://github.com/user-attachments/assets/04c2d90c-0b6b-4b35-b603-d0e10e1e4f00" />

## Design / RTL Program

module dff (
    input  wire clk,   // clock
    input  wire rst,   // asynchronous active-high reset
    input  wire D,     // data input
    output reg  Q      // output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 1'b0;
        else
            Q <= D;
    end
endmodule
 
module tff (
    input  wire clk,   // clock
    input  wire rst,   // asynchronous active-high reset
    input  wire T,     // toggle input
    output reg  Q      // output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 1'b0;
        else if (T)
            Q <= ~Q;
        else
            Q <= Q;
    end
endmodule
## **Testbench Program
// gedit tb5.v
 
module tb5;
    reg  clk, rst, D, T;
    wire Qd, Qt;
 
    // Instantiate the D Flip-Flop
    dff dff_uut (
        .clk(clk),
        .rst(rst),
        .D(D),
        .Q(Qd)
    );
 
    // Instantiate the T Flip-Flop
    tff tff_uut (
        .clk(clk),
        .rst(rst),
        .T(T),
        .Q(Qt)
    );
 
    // ---- Clock generation: 10 time-unit period ----
    initial clk = 0;
    always #5 clk = ~clk;
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("dff_tff.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb5);            // dump all signals in this testbench hierarchy
 
        $monitor("Time=%0t rst=%b clk=%b | D=%b Qd=%b | T=%b Qt=%b",
                  $time, rst, clk, D, Qd, T, Qt);
 
        // ---- Apply asynchronous reset ----
        rst = 1; D = 0; T = 0;
        #7  rst = 0;
 
        // ---- Apply test vectors on successive clock edges ----
        @(negedge clk); D = 1; T = 1;
        @(negedge clk); D = 0; T = 1;
        @(negedge clk); D = 1; T = 0;
        @(negedge clk); D = 0; T = 0;
        @(negedge clk); D = 1; T = 1;
        @(negedge clk); D = 1; T = 1;
 
        // ---- Apply reset again mid-simulation ----
        @(negedge clk); rst = 1;
        @(negedge clk); rst = 0; D = 1; T = 1;
 
        #20 $finish;
    end
endmodule


## Truth Table

### 2:1 Multiplexer

<img width="573" height="154" alt="image" src="https://github.com/user-attachments/assets/96dd3fa2-7747-402d-b88f-17c11e28fcbb" />

### 1:2 Demultiplexer

<img width="583" height="90" alt="image" src="https://github.com/user-attachments/assets/4bc21b52-1db5-4302-afab-ebb0b9d5a74b" />

## Simulation Procedure

STEP 1 – Open Terminal
Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs mux_demux.v mux_demux_tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the input combinations and corresponding outputs Y, Y0, and Y1.
A VCD waveform file is also generated:
mux_demux.vcd

STEP 5 – Open DVE

dve -full64
Other option
dve -full64 &
A DVE environment will open.
DVE Waveform Verification
In DVE:
•	Open the testbench hierarchy.
•	Locate the signals:
o	A
o	B
o	S
o	Y
o	D
o	Y0
o	Y1
•	Add the signals to the waveform window.
•	Run/inspect the waveform.
•	Verify that Y = A whenever S = 0, and Y = B whenever S = 1.
•	Verify that Y0 = D and Y1 = 0 whenever S = 0, and Y1 = D and Y0 = 0 whenever S = 1.
•	Confirm that Y0 and Y1 are never both high at the same time, since only one output is active per select value.
The waveform should agree with the truth table.

## Expected Result

The 2:1 Multiplexer and 1:2 Demultiplexer were realized using Verilog HDL:

•	Y = S'.A + S.B
•	Y0 = S'.D, Y1 = S.D

The design was compiled and simulated using Synopsys VCS, and the functionality was verified using DVE waveform analysis, matching the expected truth table.

## Output
<img width="1600" height="900" alt="WhatsApp Image 2026-10-22 at 7 01 40 PM (2)" src="https://github.com/user-attachments/assets/a6db7f27-5cd0-4da1-81fd-78798120abf2" />

<img width="1600" height="900" alt="WhatsApp Image 2026-10-22 at 7 01 58 PM (2)" src="https://github.com/user-attachments/assets/5756615d-6fd3-4278-83b6-ffbd33d0f4f3" />



## Viva-Voce Questions

•	What is a Multiplexer, and why is it called a 'data selector'?

•	What is a Demultiplexer, and how does it differ functionally from a Multiplexer?

•	How many select lines are required for an 8:1 MUX and a 1:8 DEMUX?

•	Write the Boolean expression for a 4:1 Multiplexer.

•	What is the purpose of a Verilog testbench?

•	Why is a VCD file generated, and what does $dumpvars(0, tb) do?

•	What is the purpose of ./simv?

•	What is the difference between simulation and synthesis?

•	How would you extend the 2:1 MUX design to a 4:1 MUX using two 2:1 MUXes?

## Result

Thus, the 2:1 Multiplexer and 1:2 Demultiplexer were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.



