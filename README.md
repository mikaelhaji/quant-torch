# quant-torch

some motivation =>
- 7-bill param model llama model @ 32 bits = 28 gb ((7x10^9x32)/(8x10^9)) -- which is obv not ideal bc we must load all params in memory
- computers are bad at floating point ops vs int ops - ie 3x6 vs 2.9x6.1
- thus, we must quantize! convert fp to integers (does not mean round down/up fps obv)

-----------------------

ieee-754 review =>
1) signed bit:
    - 0 = positive, 1 = negative
2) exponent:
    - 8 bits
    - 2^n
    - range (we treat as biased-127):
        - 0 to 255 [the range of STORED exponent]
        - -126 to 127 [the range of ACTUAL exponent] (128 is reserved for inf/nan & -127 is reserved for denormals -- closert to 0)
3) mantissa:
    - 23 bits


--------------------------------

types of quantization =>
1) asymmetric quantization: mapping to 0-255 (ie 0-255 is the range of the quantized values)
2) symmetric quantization: mapping to -127 to 127 (ie -127 to 127 is the range of the quantized values -- -128 is sacrificed for 0)
  - xq = clamp(round(xf/scale), -128, 127), scale = max(abs(x))/2^n-1
  - xf = xq*scale
3) 
