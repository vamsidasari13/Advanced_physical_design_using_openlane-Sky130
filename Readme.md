# **Advanced Physical Design using OpenLANE/Sky130**


![image](https://user-images.githubusercontent.com/67407412/182882940-4071bf43-69a1-4af0-920c-56e7d934d442.png)

## Table of Content

### Day1 – Inception of open-source EDA, OpenLANE and Sky130 PDK

• How to talk to computers

•	SoC design and OpenLANE

• Starting RISC-V SoC Reference design

•	Get familiar to open-source EDA tools

### Day 2 - Understand importance of good floorplan vs bad floorplan and introduction to library cells

• Chip Floor planning considerations

• Library Binding and Placement

• Cell design and characterization flows

• General timing characterization parameters

### Day 3 - Design and characterize one library cell using Magic Layout tool and ngspice

• Labs for CMOS inverter ngspice simulations

• Inception of Layout – CMOS fabrication process

• Sky130 Tech File Labs

### Day 4 - Pre-layout timing analysis and importance of good clock tree

• Timing modelling using delay tables

• Timing analysis with ideal clocks using openSTA

• Clock tree synthesis TritonCTS and signal integrity

• Timing analysis with real clocks using openSTA

### Day 5 - Final steps for RTL2GDS

• Routing and design rule check (DRC)

• PNR interactive flow tutorial

## Introduction

I learned about the many opensource EDA tools used in the synthesis, physical design, and verification stages during this 5-day session on advanced physical design using openlane/sky130. The tools for physical design flow are Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen, and Magic, among others. Overall, I learned about the fundamentals of PDKs, libraries, and openlane simulation flow throughout this workshop. Using ngspice and magic, adding a custom cell to a library addressing layout violations of design rules Enhancing design by reviewing generated reports and managing design parameters at each stage performing timing analyses before, during, and after post-synthesis.

### Openlane
OpenLANE is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, Fault, OpenPhySyn, CVC, SPEF-Extractor and custom methodology scripts for design exploration and optimization. The flow performs full ASIC implementation steps from RTL all the way down to GDSII. 
![image](https://user-images.githubusercontent.com/67407412/182885908-1a2c200d-e2a0-4216-a39e-f269a552dcc7.png)

## Day1 – Inception of open-source EDA, OpenLANE and Sky130 PDK

On day 1, the fundamental understanding of a semiconductor began with the package and continued through the device's internal macros. Following a brief introduction to the RISC-V ISA, a full explanation of how software interacts with hardware and what intermediate components are involved in the process was provided.
I was then given a brief introduction to the tools and flow of the OpenLane ASIC flow. In a few word, the GDSII will be generated if we provide this flow with the RTL file (often a.v file) and the PDK, in this case the skywater130 PDK. The PnR takes place in the OpenRoad application. and acquired a variety of tools for various steps.

Synthesis, now to carry out the synthesis step First, we must launch Docker, then use the Openlane tool inside of Docker to merge the LEF files.
 ```
 docker
 ./flow.tcl -interactive
 package require openlane 0.9
 prep -design picorv32a
 ```
 ![merging](https://user-images.githubusercontent.com/67407412/183317017-e84854e1-a67a-4452-9d8f-b8137e9a9db3.jpg)
 
 To run synthesis step
 ```
 run_synthesis
 ```
 Summary of cells in the design after performing the synthesis step.
 ![synthesis run](https://user-images.githubusercontent.com/67407412/183317008-9e1dcf93-d006-4271-99ba-a72f671706fb.jpg)
 
## Day 2 - Understand importance of good floorplan vs bad floorplan and introduction to library cells

On Day 2 of the workshop is focused on floorplan, as well as the arrangement of preplaced cells with decoupling capacitors and power planning on the core. In the floor planning stage, we make our choice. pre-placed cells and macros in the chip's arrangement, IO pad locations, power pad counts, and network-wide power distribution.
as well as with the cell deisgn flow.

#### Cell Design Flow
The stages of the cell design flow give the overall flow for the design of typical cells. A library has standard cells that contain data about the various components, their functions, and sizes as well. The drive strength of a component is indicated by its size. Additionally, it contains details on several threshold voltages that unintentionally alter the component's speed. 

![Screenshot 2022-08-08 112422](https://user-images.githubusercontent.com/67407412/183350820-77cab5f9-75cd-46a6-b4ec-b35a348a0cff.jpg)


So before running floorplan step, we can set few parameters for below dir or from synthesis.tcl file

```
home/vamsy.dasari126/Desktop/work/tools/openlane_working_dir/openlane/configuration/README.md

```
Edit the parameters with the below command 
```
set $env(parameter name) value
```
Now open the before openlane flow run floorplan

```
run_floorplan
```
![floorplan step successful](https://user-images.githubusercontent.com/67407412/183350779-be247459-81da-47a8-a9ab-63a9e68f2db4.jpg)

Command to the magic tool to display the floorplan, therefore magic .tech libraries are added to the magic tool for this.
```
magic -T $pdk_path/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
Layout after floorplan stage in Magic tool
![magic tool invoke](https://user-images.githubusercontent.com/67407412/183356554-7c6d5f89-bb9b-487e-99a7-71a95de9f2e2.jpg)

### Placement 

Following floorplan, we place the standard cells and checked the DRCs. In OpenLANE, we perform this by issuing the command run placement. There are  two levels of placement in OpenLANE are Global placement and Detailed placement. When using detailed placement, the placements of the cells are controlled, but when using global placement, all cells are distributed randomly over the floorplan. Legalization ensures that cells do not overlap in this way.

Layout after the Placement step
![placement ](https://user-images.githubusercontent.com/67407412/183409790-089c3c57-b321-4305-b4d4-0bcd17d63a26.jpg)


## Day 3 - Design and characterize one library cell using Magic Layout tool and ngspice

Day 3 describes the fabrication process using the 6-mask CMOS technology, and walks through each step. The custom inverter is cloned form github.Library Characterization 

Step to fabricate 16 mask cmos process
1) slecting a substrate
2) creating active region for tansistors
3) N-well and P-well formation
4) Formation of gate
5) Lightly doped drain(LDD) formation
6) source and drain formation
7) steps to form contacts and interconnects
8) Higher level metal formation

![image](https://user-images.githubusercontent.com/67407412/183392823-4b0de6a7-8668-475b-9dad-85c8ed5a4a10.png)

Here we clone the repo of inverter design to analyze it in the magic tool 
The inverters were created with the Magic tool. The tech file (sky130A.tech) and the.mag file are the inputs for this tool. . DRC is a magic tool that makes sure our layout is free of flaws.
Inverter layout in magic tool
![inverter design of nicson github](https://user-images.githubusercontent.com/67407412/183401790-a35f58a6-eb01-4940-b9ed-5b56d1944033.jpg)

Inverter extract to spice 

![spice file ](https://user-images.githubusercontent.com/67407412/183409154-e6561e41-eeba-4564-bf5a-d96edd439e90.jpg)

Final spice deck using sky130 tech

![flow for spice deck](https://user-images.githubusercontent.com/67407412/183407489-c35172bb-048a-46cc-bc6f-ecf40fab0bbb.jpg)

Once it has been extracted, we use the NGSPice tool to run a transient analysis and identify the four parameters specified at the start of this section.

Transiant Analysis of y vs a plot  

![image](https://user-images.githubusercontent.com/67407412/183408005-159c66ef-3640-47dd-ae85-1674e7a08dbb.png)

These 4 parameters are calculated.

Rise transition delay

Fall transition delay

Cell rise delay

Cell fall delay

## Day 4 - Pre-layout timing analysis and importance of good clock tree

On Day4 I firstly change the grid to ``` grid 044um 0.34um 0.23um 0.17um ```converted the inverter layout to standard cell lef using ``` lef write ``` on tkcon window of magic and later the inverter lef file design is included to our design and tested it. Also the synthesis optimization is done to reduce the slack value to positive. This optimization step is like an iterative step until the slack is below -1 this has to be done continuously. The pre_sta file is configured with the locations of lib, the design synthesied file, sdc file. Thsi .conf file is used to run the sta step. The data generated ie., the hold, setup and tns,wns values. Later here we dive into the openroad within the openlane terminal.

To chanage the grid size ``` grid 0.44um 0.34um 0.23um 0.17um ```

later convert the layout to .lef file ``` lef write ```

![grid to track](https://user-images.githubusercontent.com/67407412/183417514-0e454af8-b863-4dd7-8f5d-54a5b68e6ae3.jpg)

The layout with the coustom inverter cell in the design

![layout view with expand command](https://user-images.githubusercontent.com/67407412/183432339-904cb83f-283a-4748-9c13-559c745320c1.jpg)

The Fanout to reduce the slack ``` set ::env( SYNTH_MAX_FANOUT) 4 ```

![changing fanout to reduce slack ](https://user-images.githubusercontent.com/67407412/183434364-61fd399c-66b2-4445-883f-56265e49c27b.jpg)

Here replacing the cell to reduce the slack can also be done, with 

``` report_net -connections _netnumber_
replace_cell _netnumber_ sky130_fd_sc_hd__buf_(buf no)
report_checks -fields {net cap slew input_pins} -digits 4
```

reduce the slack violations below -1 this is an iterative process until the slack is negligible.

now write the verilog file with updated slack.

```
write_verilog /home/vamsy.dasari126/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/06-08_16-24/results/synthesis/picorv32a.synthesis.v
```
now run floorplan ```run_floorplan```

``` 
run_cts
```

![CTS step](https://user-images.githubusercontent.com/67407412/183454233-08f506f8-2e38-49e7-b3b3-97604b93c71e.jpg)


A cts.v file will be created  newly in the results folder.

STA analysis (with the openlane flow) instead of cts 
run openroad
``` run_openroad
read_db pico_cts.db
read verilgo /openLANE_flow/design/picorv32a/runs/06-08_16-24/results/synthesis/picorv32a.synthesis_cts.v
read_library $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pin} -format full_clock_expanded -digits 4
```
To get the reports of setup and hold skew 
```
report_clock_skew -hold
report_clock_skew -setup
```

## Day 5 - Final steps for RTL2GDS

On Day5 the routing and the post routing is done with the Tritionroute and spef_extractor 

Now run ``` gen_pdn```

a pdn.def file is created which contains the cts also with the power distribution network.

![gen_pds ](https://user-images.githubusercontent.com/67407412/183449016-1013b152-53ef-4e1b-b6ed-e11816786b43.jpg)

The routing is done with the tool tritionroute the inputs for the tool are .lef .def and preprocessed route guides. so in this routing run there might occur some routing errors. The spef extraction is done after the routing stage to remove the parasitic capacitances from the design. 
```run_routing
run_spef_extraction
```
![rotuing stage no violations](https://user-images.githubusercontent.com/67407412/183449639-f03219fa-78a7-4280-aef9-f9bd65b3a178.jpg)

After everything is done the will be 2 files as output ie., the .mag and .gds files. we can view the .mag file in magic tool or can read the .gds file from magic tool.

![final alyout](https://user-images.githubusercontent.com/67407412/183449673-d012a216-2612-40ac-9774-e9f351fe749e.jpg)

![cell expand](https://user-images.githubusercontent.com/67407412/183449695-9289d57b-d8d2-43b5-b376-c5da3488ac35.jpg)
