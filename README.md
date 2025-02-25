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
