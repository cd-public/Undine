# Undine
Public Github supporting the Undine tool from the Security132 group.  Undine will be presented at Techcon 2019.

The two directories are selected property outputs from the Undine tool with some notes to contextualize.

Traces within the "outs" directory are output files from Undine runs used when counting numbers of properties to find the steady state sizes and also some reset related properties.

Traces within the "save" directory are output files along the templates used when searching for known properties.  This specific set was used to take timing information.  It includes some notes about how the traces were generated.

## Reading outputs

Let's look at an example property and explain what all the notation means.

G(("FFex_insn[11:8]==0000" & "FFex_insn[15:12]==0000") -> "TFeear==epcr")

We can understand properties to take the form of a linear temporal logic over propositional variables, where each propositional variable is verilog statement over register transfer level processor state prefixed by a tag for the Undine typing system.

Let's look at the example property with propositional variables denoted as "p".

G((p_0 & p_1) -> p_2)

We quickly see this is the linear temporal logic formula for "globally, if p_0 and p_1 are true, this implies that p_2 is true".

Now let's look at eac p_0 and p_2 (p_1 being so similar to p_0 it is uninteresting).

FFex_insn[11:8]==0000
TFeear==epcr

"FF" or "TF" is the two character prefix.  

The first character gives the type.  In the case of p_0, the type is "F" which is an internally type (for "field") in which some indexes over a register are compared to a static value that may be used to encode Undine's "slice-value", "register-value" and in some cases "bit-value" types as the preprocessor allows propositional variables to be generated from different slice sizes of a given register which can include single bit or full register length.  In the case of p_2, the type is "T" which is an internally type (for "test") in which two registers are compared to each other by some equality operator and can be used to encode Undine's "register-register" type.

The second character identifies whether the follow verilog expression is a security signal for the type given by the first character.  In this case, both p_0 and p_2 are associated with security criticality for the given type so both are denoted "F" (for "flagged").  During development this field was ultimately always set to "F" and security signal logic was incorporated into the preprocessor but the field was maintained for future research.

"ex_insn[11:8]==0000" or "eear==epcr" is the verilog expression.

These correspond to the verilog statements that are treated as propositional variables within the context of the linear temporal logic formula.  While there are some architecture specific implementation details here, such as in some cases module names being omitted from the statements, they are as imminently human readable as any other verilog statement.

Let's look back to the overall property once more:

G(("FFex_insn[11:8]==0000" & "FFex_insn[15:12]==0000") -> "TFeear==epcr")

In plain English, this means that globally, when the verilog expressions "ex_insn[11:8]==0000" and "ex_insn[15:12]==0000" are true, correspond to a slice-value Undine type, and are recognized as security signals as that type, this implies that the verilog expression "eear==epcr" is true, corresponds to a register-register Undine type, and is recognized as a security signal as that type.

## Acknowledgements  

This material is based upon work
supported by the National Science Foundation under Grants
No. CNS-1651276 and CNS-1816637, by the Semiconductor
Research Corporation, Intel, and by a Google Faculty Research
Award.
