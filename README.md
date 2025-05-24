MOTIVATION

As part of the VLSI5 course offered in the spring semester of 2025 (for the first and maybe last time), I and 24 other selected students had the opportunity to expand the 130nm open-source library of the PULP group.
Each of us had 3 standard cells to implement, both in X1 and X2 strengths. Mine were:

– NOR4, a NOR gate with four inputs
– AO31, an AND gate with three inputs whose output is fed into a two-input NOR together with a fourth input
– XNOR3, a three-input XNOR gate

WORKFLOW

First, I designed the schematics, doing a bit of logic restructuring as follows.

– NOR4: nothing fancy — just a simple static CMOS implementation
– AO31: I had to do some logic restructuring, since static CMOS logic is naturally inverting; in the end, my implementation used only 10 transistors in total
– XNOR3: the design was quite complex; the naive implementation, cascading two two-input XNORs, required 24 transistors. However, I was able to reduce it to just 20 transistors by using transmission gates and buffering both the inputs and the output

Then it was time to do the layout. It needed to comply not only with the standard DRC rules but also with the EZ DRC rules, defined to facilitate the integration of the cells inside a larger layout. These special rules are listed in the file LayoutChecklist.pdf. For each cell, here’s what I did:

– NOR4: again, nothing fancy
– AO31: in my first iteration, I used two separate diffusion zones, but then I realized that I could merge them into a single one by choosing a different Euler path in the schematic, reducing the width by 0.42 µm (one track, as per EZ DRC rules)
– XNOR3: I managed to use just 3 diffusion zones despite having 20 transistors. The tricks I used were: using polysilicon as a routing layer together with metal1, moving diffusion zones to fit vias, and replacing the series between a transmission gate and an inverter with a tristate inverter. This allowed me to eliminate contacts between two pairs of NMOS and two pairs of PMOS, bringing the total width down to a neat 15 tracks

Once I verified that the cells were both DRC and LVS clean, I ran PEX and characterized them using Cadence Liberate. Then, I created the LEF files, followed by the GDS files, and finally exported the schematic as a CDL.
To assure that everything worked correctly, we had to synthesize a logic design using our cells, generate its backend using provided scripts, run DRC and LVS checks on it, and finally perform functional verification using QuestaSIM.