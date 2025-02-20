# Digital VLSI SoC design and planning


Digital VLSI SoC design and planning

The workshop started with an introduction to Openlane


## The Goal of the workshop
1. To get hands-on experience with the openlane flow and all the embedded tools like Yosys, ABC, Magic, OpenSTA, SPICE, TritonCTS.
2. To get the whole idea of the ASIC flow.
3. To be able to apply the concepts of physical design in practice.
4. And last but not least to git clone a custom cell and plug it into the openlane flow and obtain the GDS II flow.

### To start openlane:
Three commands were used:
1. docker
2. ./flow.tcl -interactive
3. package require openlane 0.9
   
![Like below](open lane success.png)
![Like below!](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/8e58efdf68d27227e8c5d1ad74f3b970216bd96f/open%20lane%20success.png)

### The first task was to calculate the flip-flop ratio which is defined as:

![Equation](https://quicklatex.com/cache3/72/ql_94e6e412e796a525cf1f9240fe2da472_l3.png)


![Equation](https://quicklatex.com/cache3/f3/ql_b772c4a73aac73eebaab208bc248abf3_l3.png)


