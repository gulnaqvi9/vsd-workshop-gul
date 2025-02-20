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

![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/floorplan%20successful.png)


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


# Cloning and plugging the custom cell in the OpenLane flow
1. vsdstdcelldesign folder was git cloned
   1. This contains CMOS inverter 
2. Layout was viewed in magic using the following command
   - magic -T/<path to sky130A.tech file> <path to mag.file>
   - magic -T sky130A.tech sky130_inv.mag &
     ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/inverter%20from%20spice.png)
3. Then the SPICE file was extracted using the following commands in Magic
   - extract all
   - ext2spice cthresh 0 rthresh 0
   - ext2spice
   - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/commands%20for%20extracting%20spice%20file.png)
4. SPICE file was created
   ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/spice%20file%20created.png)
5. Then the Inverter was characterised (in NGSPICE):
   - By applying a pulse input (0 to 2.5V)
   - Below are the input and output waveforms
   - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/inverter%20ngspice%20simulation%20results.png)
   - Rise Transition Delay: time taken by output to go from 20% to 80% of maximum value -> 44.13ps
   - Fall Transition Delay: time taken by output to go from 80% to 20% of maximum value -> 26.3ps
   - Cell Rise Delay: time taken by output to rise from 0 to 1 after the input switches
        - time output signal reaches 50% of its max value - time when the input signal reaches 50% of its max value
        - ->27.87ps
   - Cell Fall Delay: time taken by output to fall from 1 to 0 after the input switches
        - Time when the output signal reaches 50% of Vdd while transitioning from high (1) to low (0) - time when              the input signal reaches 50% of Vdd while transitioning from high (1) to low (0).
        - ->2.93ps
     ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/inverter%20characterization.png)
 6. Then a file (drc_tests.tgz) was downloaded using the below command:
    - wget HTTP://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
    - tar  xfz drc_tests.tgz -> command was used to extract, decompress and unpack the contents of file.
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/tar%20file.png)

 7. File (met3.mag) was opened in magic
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/met3.mag%20file%20in%20magic%20.png)
    - To check the drc rule violation, command -> drc why
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/drc%20rule%20violation%20in%20metal3.png)
      
 8. Exercise: To load file (poly.mag) and run drc check
    - poly.mag file
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/poly.mag.png)
    - Different layers
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/poly.mag%20different%20layers.png)
    - DRC Rule Violation: minimum spacing between poly layer and poly resistor layer
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/rule%20violation%20minimum%20spacing%20between%20poly%20resistor%20to%20poly.png)
    - DRC Rule violation: when n-well is untapped
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/drc%20rule%20violation%20when%20nwell%20is%20untapped.png)

9. Extracted LEF file from .mag file
    -![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/extracted%20lef%20file%20from%20.mag%20file.png)
   
11. Copied LEF file to picorv32a/src
12. Copied following timing files to picorv32a/src
    - sky130_fd_sc_hd_typical.lib
    - sky130_fd_sc_hd_fast.lib
    - sky130_fd_sc_hd_slow.lib

13. Modified the config.tcl file
    - Added lib file using LIB_SYNTH command (lib file for abc mapping)
    - add the below line to point to the LEF location which is required during spice extraction.
    - set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef

15. Run OpenLane and run the following commands:
    - docker
    - /.flow.tcl -interactive
    - package require openlane 0.9
    - prep -design picorv32a -tag 12-02_14-32 -overwrite
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/synthesis%20of%20custom%20inv%20success.png)



16. Synthesis step preformed for the custom cell: Inverter
    - Include the below command to include the additional lef into the flow
    - set lefs [glob $::env(DESIGN_DIR)/src*.lef] 
    -  add_lefs -src $lefs
    -  run_synthesis
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/custom%20cell%20slac%20violation%20at%20synthesis.png)
   

17. Floorplan step
    - run the following commands:
    - init_floorplan
    - place_io
    - tap_decap_or
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/flooorplan%20success.png)
   
18. Placement step
    - use the following command
    - run_command: run_placement
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/placement%20done.png)
   
19. To reduce slack post placement
    - Max fanout was reduced to 4 from 6 using: set ::env(SYNTH_MAX_FANOUT)
    - Negative slack was made zero and synthesis was implemented again
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/changed%20%20fanout%20to%204%20from%206%20and%20running%20synthesis%20again%20post%20placement.png)

20. Layout of a custom cell included in the picorv32a design post placement
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/custom%20cell%20layout%20post%20placement.png)

21. Clock Tree Synthesis
    - command: run_cts
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/cts%20success%20.png)

22. Generating the power distribution network
    - command: run_pdn
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/pdn%20success%20post%20cts.png)
      
22. Then open OpenRoad, use the following commands
    1. For the post CTS STA 
       - open openroad
       - read_def /<path_to_cts.def>
       - read_lef /<path_to_merged.lef>
       - write_db <filename.db> 
       - read_db <filename.db>
       - read_verilog <path_to_verilog_file_created_post_cts> 
       - read _librerty <path_to_typical_timing_library>
       - read_sdc <path_to_my_base.sdc>
       - link_design picorv32a
       - set_propagated_clock [all_clocks]
       - report_checks -path_delay min_max -fields{slew trans net cap input pin} -format full_clock_expanded -digits 4
       - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20cts%20slack%20met.png)
       - exit openroad

23. Routing
    - use the command: run_routing
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/routing%20completed.png)
    - Post routing layout
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20route%20layout.png)
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20route%20layout%20zoom.png)

24. Post Routing STA
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20rout_sta.png)
    - STA for Setup (min path)
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20rout_stasetup.png)
    - STA for Hold (max path)
    - ![](https://github.com/gulnaqvi9/vsd-workshop-gul/blob/main/post%20rout_sta%20hold.png)







      
     


