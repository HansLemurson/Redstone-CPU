The encoding for this instruction set contains space for 30 instructions.

14 instructions use an F-Code, plus two F-codes (0000 and 1000) dedicated to the special R-Code instructions

16 R-Code instructions have what would have selected a Register instead choose a function.  These functions can thus take in no other inputs.

But regardless, I need to make a Lookup Table that will handle these 30 cases.

The whole table can be subdivided into 4 Sub-Tables, each with instructions in their own category.

Table A: 1-Operand Arithmetic
F-codes 0001-0111

Table B: 1-Operand Memory
F-Codes 1001-1111

Table C: 0-Operand Arithmetic
F-Code: 0000
R-Codes 1000-1111

Table D: Branching
F-Code: 1000
R-Codes 1000-1111

What I need is a scheme that can easily select the proper table segment

F-Codes in truth can and should be looked at as S-FFF rather than FFFF, the S defining which...Set of codes is used.

Scheme: 1-RRR-S-FFF

A: rrr-0-FFF
B: rrr-1-FFF
C: RRR-0-000
D: RRR-1-000

F-Codes select the function in A and B, so I need to decode FFF
R-Codes select the function in C and D, so I need to separately decode RRR

R-Codes are disabled whenever FFF == 000
S is used as the 4th bit in both A&B and C&D to choose between the two tables.

#Lookup Tables
Plan for how the control-lines for the 4 different tables should be grouped

Table A:
Carry In, Xnor, Left-And, Right-Shift, B Invert, Accumulator Invert,  Block Acc Write, Flag Write
    Cin Xnr Lan Rsh Biv Aiv Bwr Fwr
ZOP~ 
ADD 
CMP *               *       *   *
SUB *               *
AND         *   *
XOR     *           *
ANB         *   *   *
NOR?        *   *   *   *

Table B:
B invert, B Flood, Divert Accumulator, Block Acc Write, Register Write, Data Stack Push, Data Stack Pop
    Biv Bfl Dac Bwr Rwr Dps Dpo
BRC~
LOD         *
MOV *   *   *       *
CPY         *   *   *
?
SWP         *       *
PSH                     *
POP         *       *       *

Table C:
Carry-in, Left-And, Right-Shift, Flood B, Invert B, Invert Accumulator, Duplicate Highest Bit
    Cin Lan Rsh Bfl Biv Aiv Dhb
CLR     *       *   *
INC *           *   *
RSH         *   *   *
INV             *   *   *
LSH     *       *
NEG *           *   *   *    
HLV         *   *   *       *
DEC             *

Table D:
Block Acc Write, Branch, Zero Flag Check, Sign Flag Check, Invert Flag Check, Instruction Push, Instruction Pop, Halt
    Bwr Brc Zfc Sfc Ivf Ips Ipo Hlt
JMP *   *
BLT *   *       *       
BEQ *   *   *
CAL *   *               *
RET *   *                   *
BNE *   *   *       *
BGE *   *       *   *
WFI *                           *