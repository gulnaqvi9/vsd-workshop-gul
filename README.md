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
   
![    Like below!](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/8e58efdf68d27227e8c5d1ad74f3b970216bd96f/open%20lane%20success.png)

# Then cell and Tech file were merged using the command
prep -design picorv32a 

# Then Synthesis was run using the command:
run_synthesis
![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/synthesis%20successful.png)


## The first task was to calculate the flip-flop ratio which is defined as:

![Equation](https://quicklatex.com/cache3/72/ql_94e6e412e796a525cf1f9240fe2da472_l3.png)
![Equation](https://quicklatex.com/cache3/f3/ql_b772c4a73aac73eebaab208bc248abf3_l3.png)

![] (https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/flip%20flop%20post%20synthesis.png)


# Then floorplan was done using the command:
run_floorplan

![] (https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/floorplan%20successful.png)


# Layout post floorplan was viewed in Magic using the following command:
magic -T/<path to sky130.tech file> lef read <path to merge.lef file> def read <path to the DEF file created post floorplan>
for example: magic -T/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![]( https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/layout%20after%20floorplan.png)


To check out on which metal layer io pins are placed
In the below image, the selected pin: __mem_addr[30]__ (horizontal pin) is placed on metal layer 3
![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/horizontal%20io%20pin%20layer%20post%20floorplan.png)

In the below image, the selected pin: __mem_addr[24]__ (vertical pin) is placed on metal layer 
![] (https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/vertical%20io%20pin%20layer%20post%20floorplan.png)

# Following this came, Placement
command used: run_placement
![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/placement%20success.png)


# Layout post placement!
![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/layout%20post%20placement.png)

After zooming in!
![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/layout%20post%20placement%20zoomin.png)


