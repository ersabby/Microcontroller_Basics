# Microcontroller_Basics

Bit-banding is a term that ARM uses to describe a feature that is available on the Cortex M3 and M4 CPU cores. Basically, the 
device takes a region of memory (the Bit-band region) and maps each bit in that region to an entire word in a second memory 
region (the Bit-band Alias Region).

The benefit of Bit-banding is that a write to a word in the alias region performs a write to the corresponding bit in the Bit-
band region. Also, reading a word in the alias region will return the value of the corresponding bit in the Bit-band region. 
These operations take a single machine instruction thus eliminate race conditions. This is especially useful for interacting 
with peripheral registers where it is often necessary to set and clear individual bits.

The image below shows a byte in the Bit-band region on the top. The bottom row of bytes represent the bit-band alias region. 
For demonstration purposes I choose to use 8-bit words. On the Cortex M3 and M4 the alias region would contain 32-bit words.

# bit-banding

To use this feature, you must first get the address of the word in the alias region that corresponds to the bit you wish to read
/write. This is done in C with a simple pre-processor macro. For a detailed example of how to use this feature, check out the 
ARM InfoCenter page on bit-banding.
