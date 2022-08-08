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

On Day2 
 
 
