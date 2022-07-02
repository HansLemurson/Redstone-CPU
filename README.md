# Redstone-CPU
Plans for creating a CPU in Minecraft

This is my working draft for an 8-bit instruction set that is suitable for the Redstone CPU that I am planning in Minecraft.

The CPU uses an Accumulator architecture, which greatly simplifies the Instructions, since you only need to provide 1 operand.

#ALU Principles
Internally, the ALU uses an Adder which has various modifications added onto it which break and alter its function in order to produce entirely new functions.

Addition is the basic function it can perform, but this can be converted to Subtraction, for example. 
To create Subtraction, you invert the input to be subtracted, and then add 1 to the sum.  
The design of the adder contains a mostly un-used Carry-In input (what Carry could the lowest bit possibly receive?), which does the job of adding an extra +1 to whatever sum you're computing.

Other functions are created by splitting apart the individual actions of the Adder.
Adders consist inherently of XOR and AND gates.  By forcing subcomponents of the Adder into the "HIGH" state (something you can only easily do in mono-polar systems like Redstone, but would not be reasonable to do in electronics) you can effectively disable them, meaning the only part of the Adder that still responds to inputs does only the AND or XOR internal function.

The way the hardware works, gaining access to the XOR gates adds a forced inversion, so the function you isolate is actually XNOR, but that's just as easy to work with, since you're just one inversion away from getting back to good old XOR.

The AND function is somewhat trickier, since it comes from the Carry-Out components, which inherently deliver their output to the next higher bit (as they should).  This means that the function you isolate is actually a LEFT-Shifted AND.  In order to get a pure AND function out of the ALU, you have to right-shift the output.  But given that RIGHT-Shift is one of the important required functions anyways, it's no hardship to add this to the ALU.

Thus, a handful of tweaks, when applied in the right combination to the Adder, can create a multitude of useful functions that you might wish to perform on your Data.

Extracting the Carry (by disabling secondary summation): Yields <<($A AND $B)