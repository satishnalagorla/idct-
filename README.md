# idct-
idct processor using verilog


Inverse Discrete Cosine Transform (IDCT) Theory
Introduction to IDCT

The Inverse Discrete Cosine Transform (IDCT) is the mathematical process used to reconstruct 
the original signal from its Discrete Cosine Transform (DCT) coefcients. IDCT is widely used in 
image and video compression standards such as JPEG, MPEG,  to restore image blocks from 
their compressed forms.
The image can be reconstructed from the DCT (u,v) by evaluating the (inverse 
DCT) product of the matrix: 


IDCT = C^T (DCT) C

IDCT MAIN Module
1. idct_main is a Verilog module implementing Inverse Discrete Cosine Transform (IDCT) for image and video compression.  
2. It takes 64-bit quantized input (di) processes it, and outputs an 8-bit reconstructed pixel (idct).  
3. A **dual-port RAM dual_ram stores input data, while a ROM (dual_port_rom) holds transformation coefficients.  
4. The first matrix multiplication (C^T * Q) is performed using 8×8 multipliers (multi_8_8).  
5. An **adder (adder12s) sums up intermediate multiplication results.  
6. A **register (dctreg2x8xn) temporarily stores the summed values for the second transformation step.  
7. The second matrix multiplication (C * Sum) is done using **signed multipliers (mult11s_8s)  
8. Another adder (adder14s) sums the final results, producing the IDCT output (dct[7:0])  
9. The control unit (idct_controller) manages data flow, signals, and processing synchronization.  
10. The module efficiently reconstructs compressed image data, making it ideal for **real-time image/video processing

DUAL_RAM Module

The dual_ram module implements a dual-port RAM system with two memory banks, controlled by clock signals and a read/write enable signal. 
It uses two instances of the ram_rc_ram1 module, where one operates in read mode while the other is in write mode, depending on the rnw signal.
Data is written into memory based on byte enable signals and read column-wise from the stored locations.
A delayed rnw signal ensures proper synchronization of data output selection. The ram_rc_ram1 module manages 8x64-bit memory, 
updating values based on byte-wise control and clock synchronization.

DUAL_PORT_ROM Module

The given Verilog module implements a dual-port ROM with two independent read addresses (addr1 and addr2), allowing simultaneous access to stored data. The ROM stores an 8-location, 64-bit wide dataset representing values from matrix C and its transpose (C^T). The module uses pipeline registers to ensure stable data output (dout1 and dout2) synchronized with the clock signal. The data retrieval is handled through combinational logic using case statements, selecting the appropriate stored value based on input addresses. Two pipeline stages (dout1_reg1, dout2_reg1) help in reducing timing delays and ensuring stable outputs.

MULTIPLIER_8_8 Module

This is an 8x8 signed multiplier implemented in Verilog using a pipelined approach. The inputs are two 8-bit signed numbers (n1 and n2), and the output is a 16-bit signed result. The design utilizes partial product generation, addition, and multi-stage registers for pipelining. The sign of the result is determined using XOR of input sign bits, and two’s complement is applied when needed. The input c is transposed from a dual-port ROM, which allows efficient retrieval and processing of values

ADDER12S Module
The given Verilog module implements a 12-bit adder with pipelined multi-stage addition for summing eight input values (n0 to n7). It performs hierarchical addition in three stages, first summing least significant bits (LSB) and then most significant bits (MSB) with carry propagation. Pipeline registers store intermediate results to improve timing performance across clock cycles. The final sum (sum[14:0]) is computed by combining the accumulated MSB and LSB results.

IDCTREG Module
The sum is tempororily stored in this module 

MULTIPLIER_11_8S Module
This is second stage multiplication here inputs  c matrix from  dual_port_rom and previous sum output multiplication takes place 

ADDER14s Module
The adder14s module is a 14-bit pipelined adder that sums eight 14-bit signed numbers using a multi-stage addition approach. It processes the least significant bits (LSB) and most significant bits (MSB) separately while using pipeline registers to optimize timing and performance. The addition is performed in three hierarchical stages, progressively summing the partial results while preserving carry bits. Clocked pipeline registers store intermediate sums to maintain synchronization and avoid timing bottlenecks. Finally, the results are concatenated to produce a but we taking  8-bit signed sum output 

Controller Moule
The IDCT controller manages the timing and control signals for inverse discrete cosine transform processing. It utilizes multiple counters (cnt1_reg, cnt2_reg, cnt3_reg, addr) to synchronize data flow, enabling and disabling operations at specific stages. The module controls read/write (rnw), ensures valid IDCT output (idct_valid), and manages the processing sequence based on start and hold signals. It toggles states efficiently to process IDCT blocks while maintaining proper synchronization
