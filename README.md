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
1. idct_main is a Verilog module implementing **Inverse Discrete Cosine Transform (IDCT)** for image and video compression.  
2. It takes **64-bit quantized input (`di`) processes it, and outputs an **8-bit reconstructed pixel (`idct`)**.  
3. A **dual-port RAM dual_ram stores input data, while a ROM (dual_port_rom) holds transformation coefficients.  
4. The first matrix multiplication (`C^T * Q`) is performed using **8×8 multipliers (`multi_8_8`)**.  
5. An **adder (`adder12s`)** sums up intermediate multiplication results.  
6. A **register (`dctreg2x8xn`)** temporarily stores the summed values for the second transformation step.  
7. The second matrix multiplication (`C * Sum`) is done using **signed multipliers (`mult11s_8s`)**.  
8. Another **adder (`adder14s`)** sums the final results, producing the **IDCT output (`dct[7:0]`)**.  
9. The **control unit (`idct_controller`)** manages data flow, signals, and processing synchronization.  
10. The module efficiently reconstructs compressed image data, making it ideal for **real-time image/video processing**. 🚀
