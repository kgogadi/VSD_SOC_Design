# VSD_SOC_Design
# Inception of open-source EDA, OpenLANE and Sky130 PDK
## How to talk to computers
### Introduction to QFN-48 Package, chip, pads, core, die and IPs
**Aurdino Board**: The image below is an Aurdino Microcontroller Board. Here we focus more on the encircled area, which contains the 'Microprocessor',that we will be designing from abstract level till fabrication level by using RTL to GDS flow
![Arduino](https://github.com/user-attachments/assets/58d01c40-c4c4-4e05-b9a7-4b0ccb87a53b)

While we cosider the chip, there are 3 main components:

(1) Pads : Through these pads signals can travel into the chip from external sources and viceversa.

(2) Die : It is the whole area of the chip that will be manufactured on the silicon wafer.

(3) Core : It is the area where our entire logic will be implemented.

![image](https://github.com/user-attachments/assets/e629fb04-a820-478a-8f77-d2f7a3c7c4d5)

The above mentioned MicroProcessor in an aurdino board is a package that consists of MicroProcessor chip and some other Foundry IP's(Intellectual Property). All of these combined called as a package.

For example, let's consider a package that consists of RISC V SOC and some other IP's such as SRAM , ADC , DAC , PLL etc..

![image](https://github.com/user-attachments/assets/6c0f6ec9-774a-412b-8774-c6ce4bdced4e)

## Introduction to RISC-V
**ISA**: ISA is known as "Instruction Set Architecture". It is nothing but a way of communicating with the computer. In general we write codes that need to be executed by the system, using coding languages like C , Java etc. , but machine can't understand those languages. It is where ISA comes into picture. By using ISA the codes that were written wil be converted into assembly language and from there to binary i.e Machine understandable language. This is the purpose of the ISA, and the RISC V ISA is the latest ISA released.
![image](https://github.com/user-attachments/assets/71a79d0f-a7b1-4fbb-b075-8502b80cd0b6)
#From Software Applications to Hardware
In real life we generally deal with Application Softwares(Apps) in order to communicate with the Hardware.But how it's done exactly? In between Application software and Hardware, there will be a software called System Software. The Applications enter into the System Software and will be converted into the Hardware understandable language i.e Binary Language.

The System Software consists of different layers:

(1) O.S : Other than the general operations like Handling I.O operations, Allocating Memory, Low level System Functions the O.S converts the Application software to respected C,C++,Java etc.. codes.

(2) Compiler : The compiler takes output of O.S as input and converts the codes in C,C++,Java etc.. into Instruction set(.exe files). These instructions will be dependent on type of hardware used.

(3) Assembler : Assembler converts the .exe files into binary language and provides it to the hardware, and hardware performs the respective operations.
![image](https://github.com/user-attachments/assets/6f75d165-9be9-4cc9-9dda-9d6bfe01ba8e)

##SOC Design and OpenLANE
###Introduction to all components of open-source digital asic design
In order to design a Open Source Digital ASIC, we primarily need some components. Such as

(1) RTL Designs

(2) EDA Tools

(3) PDK Data

There are some open sources to get these required components.

For example, In case of RTL Designs we have librecores.org , opencores.org , github.com etc...

In case of EDA Tools we have Qflow , OpenRoad , OpenLane etc...

In case of PDK Data, Recently in 2020 Google collabarated with SkyWater Technology and made FOSS 130nm Production PDK OpenSource.
![image](https://github.com/user-attachments/assets/e0698af4-3110-473d-a864-a8dfa302fa8f)

What are RTL Designs?

RTL(Register-Transfer-Level) design is a crucial step in the VLSI design flow, which involves the creation of electronic circuits using integrated circuits (ICs). It involves the specification of a digital circuit in terms of the flow of digital signals between hardware registers, and the logical operations performed on those signals.

What are EDA Tools?

EDA(Electronic Design Automation) Tools are used to design and verify the functionality of an Integrated Circuit to ensure it deliver's the required performance and density.

What is PDK Data?

PDK(Process Design Kit) is a collection of files used to model a fabrication process for EDA Tools used in the design of an IC.The collection consists of

Process Design Rules : DRC , LVS , PEX
Device Models
Digital Standard Cell Libraries
I/O Libraries etc....
Simplified RTL2GDS Flow
The simplified RTL to GDS Flow starts with an RTL file. After going through some set of stages we will get the output as a GDS file, which can be readily sent to foundry for fabrication. The steps involved in the RTL2GDS Flow is of following:
![image](https://github.com/user-attachments/assets/3a2c3346-bb59-49fb-afad-b3b4b997726a)
(1) synthesis :

In synthesis stage RTL file will be converted into a circuit by using the components from the Standard Cell Library.
The cells in the Standard Cell Library are called Standard Cells and they have the regular layout of same height but different widths.
Each cell has different models based on Electrical,HDL,Spice,Layout(Abstract and Detailed) etc....
![image](https://github.com/user-attachments/assets/06db9961-5936-40b9-bfc7-cf9ebde37f68)

(2) Floor Planning & Power Planning :

Floor Planning is a stage where the position of the components on the chip will be decided by keeping the area of the chip as minimal as possible by following a set of rules.
During the Floor Planning Stage itself the position of I/O pins,ports,pads will be determined.

![image](https://github.com/user-attachments/assets/2535e118-118e-42ca-9551-00cb825132a5)

In Power Planning stage the Power supply network i.e VDD & GND of the chip will be laid out. During Power PLanning 3 components Power Rings , Power Straps , Power Pads will be laid out.
For Power Network Top Metal Layers will be used because the Power Network should have minimum delay as possible and the top layers of metal will have the low resistance, So they are used.

![image](https://github.com/user-attachments/assets/95551fe5-eb61-4bea-9fda-9b225a5f9201)

(3) Placement :

In Placement stage the components are placed within the areas planned during the FloorPlanning Stage.
Along with them Standard Cells that are required in the design are also placed within the cell boundaries.
Placement will be performed in 2 stages. They are Global Placement and Detailed Placement.
During Global Placement the Standard cells may overlap and does not follow the Placement Rules.
In Detailed Placement every standard cell will be placed in its optimal position by following the Placement rules.
![image](https://github.com/user-attachments/assets/a12df90b-94a3-453a-a76d-51290b82da06)

(4) CTS(Clock Tree Synthesis) :

Before Performing Routing of the signals, we should perform Clock Routing.
While Routing the clock tha major problem is CLock Skew.
Clock Skew is the difference in time taken for the clock to reach two different destinations in the design.
In order to eliminate the clock skew, we should deploy the technique called Symmetric Tree Structure.
There are different types of Symmetric Tree Structures. They are H-tree,I-tree,X-tree etc...
![image](https://github.com/user-attachments/assets/36fd039d-323b-4f1f-abf0-a1e01ff1b63c)

(5) Routing :

Afetr the clock routing is done, now signals must be routed.
Router uses the remaining metal layers and makes conections in the design.
Routing will be carried out in 2 stages.They are Global Routing and Detailed Routinng.
In the stage of Global Routing, the tool generates a Routing guide by following the instructions given in the PDK.
In the stage of Detailed Routing, actual routing will be done according to the guide generated in the Global Routing Stage.

![image](https://github.com/user-attachments/assets/5acebd47-5140-4c30-b814-69ce38f41c2b)

(6) Sign-off :

After Routing is done, the chip will be considered as completed and during Sign-off stage different types of checks will be performed.
Physical Verification Checks : Design Rule Check(DRC) , Layout Vs Schematic(LVS)
In DRC, the chip will be verified for the violations in the Design Rules provided by the PDK.
In LVS, the chip will be checked for the functionality whether it matches to the gate level netlist functinality.
Timing Checks : Static Timing Analysis(STA)
During STA, the design is checked for Timing violations.

##Introduction to OpenLANE and Strive chipsets
OpenLane is started as an Open Source Flow for a true Open Source Tape-out experiment.It was from e-fabless.It is a platform which supports different tools such as Yosys,OpenRoad,Magic,KLlayout and some other Open source tools.It integrates the various steps of Silicon Implementation and abstracts it. At e-fabless they have an SOC family called Strive. Strive is a family of open everything SOCs having Open PDK, Open RTL, Open EDA.

The Strive SOC family of several members and the list is given below
![image](https://github.com/user-attachments/assets/e4016dd0-8095-495f-8365-36ccc1d1ab69)
The main goal of OpenLane is to "Produce a clean GDS file with no human intervention with no LVS,DRC,Timing violations. It is majorly tuned for Skywater 130nm OpenPDK but also supports XFab180 and GF130G. It is functional out of the box.It can be used Harden Macros and chips. It has two modes of operation , autonomous and interactive. It has a feauture called Design Space Exploration which helps in finding the best set of flow configurations.Presently it also has large no.of design examples nearly 43 and more will be added soon.

##Introduction to OpenLANE detailed ASIC design flow
![image](https://github.com/user-attachments/assets/e07b6a29-1365-4bca-9437-7b7cfb382ad5)
The image shows the OpenLane detailed ASIC Design Flow.The flow starts with Design RTL, It goes through RTL synthesis by Yosys and abc giving an optimised gate level netlist. Now we perform STA on the optimised netlist inorder to check for Timing violations. After STA we perofrm DFT and this step is optional and for this we use FAULT tool.

Fault(for DFT) :

Scan Insertion
Automatic Test Pattern Generation(ATPG)
Test Pattern Compaction
Fault coverage
Fault Simulation
![image](https://github.com/user-attachments/assets/cfbf8712-e3c0-4e75-b7d6-34ebc244c85c)

After performing DFT next comes Physical Implementation.It is also called as Automated PnR(Place and Route) and for this we use OpenRoad.

OpenRoad(for Physical Implementation) :

Floor/Power Planning
End Decoupling Capacitors and Tap Cells Insertion
Placement: Global and Detailed
Post placement optimization
Clock Tree Synthesis
Routing: Global and Detailed
During PnR for every change in the design, we should check for LEC(Logic Equivalance Checking). LEC is used to confirm that the function did not change after modifying the netlist and also during physical implementation we have a important step called "Fake Antenna Diodes Insertion Script".

Dealing with Antenna Rule violations : When a metal wire segment is fabricated, it can act as an antenna.

Reactive ion etching causes charge to accumilate on the wire.
Transistor gates can be damaged during fabrication.
There are two solutions for this problem

Bridging attaches a higher layer intermediary. It requires Router awareness.
![image](https://github.com/user-attachments/assets/638c9faa-70eb-43e1-a3cb-beb21f014163)
* Add antenna diode cell to leak away the charges. Antenna diodes are provided by the SCL. For this we took a preventive approach.
  * Add a Fake antenna didoe next to every cell input after placement.
  * Run the Antenna Checker(Magic) on the routed layout.
  * If the checker reports violation on the cell input pin, replace the fake diode cell by a real one
![image](https://github.com/user-attachments/assets/3573c4e7-2bb6-40ba-88e6-57e8a26cfa11)
And at the end, we perform Physical Verification. Which includes DRC(Design Rule Checking) , LVS(Layout Vs Schematic). Along with the P.V we also performs STA to check for timing violations in the design.
* MAGIC is used for DRC and SPICE Extraction from Layout.
* MAGIC and Netgen are used for LVS by comparing Extracted SPICE by MAGIC and Verilog Netlist.


###Design Preparation Setup
In order to enter into BASH, by being in OpenLane directory we should use a command called **docker**. By using docker command we will enter into the Bash. After entering into bash we have to use the script flow.tcl, because this .tcl file contains the steps that need to be executed in the OpenLane and along with the tcl file we need to use **-interactive** switch in order to perform step by step process. If not used interactive switch the whole flow i.e RTL to GDS will be executed once and the final report will be given. The command that we should use for this is **./flow.tcl -interactive**.Now OpenLane is opened and we can observe the change in prompt and now we have to input packages required to run the flow and for this we use the command **package require openlane 0.9**.
![Screenshot from 2024-07-26 08-48-34](https://github.com/user-attachments/assets/b735288a-0b2b-41bf-af3d-4ca82c8aa986)
![Screenshot from 2024-07-27 23-27-46](https://github.com/user-attachments/assets/6e443b2d-7b45-4146-9967-3bd2a7b7a7fc)
At the end of the terminal we can see that Preparation is complete.
![Screenshot from 2024-07-27 23-34-11](https://github.com/user-attachments/assets/d75d278d-1fb1-428d-92e0-962c3cda0bab)

One of the files will be "merged.lef" file, it contains metal layer level and cell level information.
step 2: **run_syntheses**
Tool will take some time to perform synthesis, when completed it displays the Synthesis was successful message.
![Screenshot from 2024-07-28 00-12-43](https://github.com/user-attachments/assets/3b003d14-1cb8-4732-a68d-26da772ef2b0)
to find out the flipflop percentage in total cells. For this we use reports from Synthesis stage.
![Screenshot from 2024-07-28 00-29-17](https://github.com/user-attachments/assets/6777292b-c8a6-4ef7-aa45-17342038bd34)
In the above image,we can see that the total no.of cells used in the design are 14876 and the count of D-flipflops in the design are 1613. So, the flipflop percentage is calculated as

Flop Ratio = ((no.of flipflops) / (Total no.of cells))100 = (1613/14876)100 = 10.84%

# Good FloorPlan Vs Bad FloorPlan and Introduction to Library Cells
## Chip FloorPlanning Considerations
### Utilization Factor and Aspect Ratio
**FLOOR PLANNING**
1. Define height and width of core and die areas.

Core is an area in a chip which is used to place all the logic cells and components in a chip. It is the place where logic lies in a chip.
Die is an area that encircles the core area and used for placing I/O related components

![image](https://github.com/user-attachments/assets/c4855bb1-643f-4aff-b65a-c0c05a163576)

The height and width of core area will be decided by the netlist of the design. It will be based on the no.of components required in order to execute the logic and the height and width of the die area will be dependent on the core area height and width.

![image](https://github.com/user-attachments/assets/eaa00b0e-6d74-4678-bdc5-2294892c9642)

consider a netlist that is having two logic gates and two flipflops each having area of 1 sq.unit. The netlist contains 4 elements and the minimum total area required for the core area will be 4 sq.units.

![image](https://github.com/user-attachments/assets/8cb3c9c9-384b-4d6c-9d37-5bd404d21899)

![image](https://github.com/user-attachments/assets/1a664ace-fc02-4f42-8bf6-92a5e30e1d82)

Utilization Factor : Utilization Factor is defined as "The ratio of the core area occupied by the netlist to the total core area".For a good FloorPlan, The Utilization Factor should never be '1' because when the Utilization factor becomes '1' , there will be no place for adding additional logic if needed and it will be considered as a bad FloorPlan.

**Utilization Factor = (Area occupied by netlist / Total core area)**

Aspect Ratio : Aspect Ratio is defined as "The ratio of Height of the core to the width of the core". If the Aspect ratio is '1' , then the core is said to be in a square shape and other than '1' the core will be a rectangle.

**Aspect Ratio = (Height of the core / Width of the core)**

Considering different cases 
Case 1:
![image](https://github.com/user-attachments/assets/0267d674-63ea-4b1b-84d5-5fed90f72ee1)

**Utilization factor**= (4 squnits)/(4 squnits) = 1
**Aspect Ratio** = (2 units)/(2 units) = 1 //The core is in a square shape.

case 2:
![image](https://github.com/user-attachments/assets/b1ae6e65-d726-48dc-8742-f1078af4c34c)
**Utilization factor** = (4 squnits)/(8 squnits) = 0.5
**Aspect Ratio** = (2 units)/(4 units) = 0.5 //The core is in a rectangular shape.
case 3:
![image](https://github.com/user-attachments/assets/d883df3c-5128-48e0-b94d-e5b9cd8ed00b)

**Utilization factor** = (4 squnits)/(16 squnits) = 0.25
**Aspect Ratio** = (4 units)/(4 units) = 1 //The core is in a square shape.

2. Define Locations of **Preplaced Cells**

The concept of pre-placing cells is nothing but reusing already designed blocks by not designing them again and again. The most commonly used pre-placed blocks are Memory , comparators , Mux etc.. , These blocks can be called as Macros (or) I.P's .We need to place these macros very carefully in such a way that if these blocks are more connected to input pins, then we should place these close to those input pins. These should be placed in a way such that the wiring length should be decreased.

The term Pre-placed refers to "Placing those blocks prior to placement stage that is in Floorplan stage. After placing those blocks in Floorplan stage we need to define some placement blockages in order to avoid Placing of other standard cell near to those blocks by the tool during placement stage. By using this pre-placed cells the Time-to-Market can be reduced.

![image](https://github.com/user-attachments/assets/1ef60d40-8127-48e3-8103-0ee21d417d46)

## De-coupling Capacitors
Generally these pre-placed blocks will be high-power draining blocks. In some cases, the power they recieve from the power source will not be sufficient for them to perform switching i.e the signal will not be in the range of its noise margin because there will be a voltage drop in the inter-connecting wires. In this case, the De-coupling Capacitors comes into the picture.
![Screenshot (72)](https://github.com/user-attachments/assets/48c804f9-b69f-4d86-bc77-a9124d799446)
![Screenshot (73)](https://github.com/user-attachments/assets/e721b0b1-cab4-4bac-8307-f7aa93091c7d)
These De-cap cells will be placed near to the blocks that will drain high power. When there is no switching is being performed the De-cap cell will be connected to power source and gets charged to its high level and when the switching is being performed the De-cap cells will be connected to the blocks and the power required for the block will be supplied by the De-cap cell, and when ever the switching stops again the De-cap cell will start to getting charged. This is the working of De-cap cells and these cells plays a crucial role in the circuit design.
![Screenshot (74)](https://github.com/user-attachments/assets/49e18fa8-6c66-4a88-a102-91b57ca67ade)
![Screenshot (75)](https://github.com/user-attachments/assets/491a4155-5378-4f7b-9e28-4e381416dd53)
![Screenshot (76)](https://github.com/user-attachments/assets/0759a826-ad83-426a-97ab-f4cbb510ecda)
![Screenshot (77)](https://github.com/user-attachments/assets/94582e74-b128-4afd-bd72-f56904df9960)
![Screenshot (78)](https://github.com/user-attachments/assets/859e4d10-ba71-49b5-935f-ec885e11bc30)

## Power Planning
In the previous section we used De-cap cells to manage power for different blocks.But Decap cells have some limitations such as Leakage power and increase in the area of chip. To overcome these we use a technique called Powerplanning. In some areas of the chip when there is more switching happening, two tyoes of phenomena can occur

* Voltage drop
* Ground bounce
![image](https://github.com/user-attachments/assets/7ccf47a3-6b96-4c4f-bc58-5cb81d225666)
![Screenshot (79)](https://github.com/user-attachments/assets/034dda85-d1c5-4be1-a58f-82e058663305)
**Ground Bounce**: When a group of cells are simultaneoisly switching from 1 to 0, then every cell dumps the power to th ground simultaneously to the same ground pin. In this case the ground instead of being at 0 experiences a short rise in the voltage and this is called as "Ground Bounce".The problem occurs only when the voltage level goes above the noise margin.
![Screenshot (80)](https://github.com/user-attachments/assets/e4b6a6b6-d334-4ebe-bc08-03d049d0d7c9)
**Voltage drop** : When a group of cells are simultaneously switching from 0 to 1, then every cell needs the power and In case the power is supplying from one source, there may occur the shotage of power and drop in the input voltage happens at that place. This is called as "Voltage Drop". The problem occurs only when the voltage level goes below the noise margin.
![Screenshot (81)](https://github.com/user-attachments/assets/31d0d03d-b6d8-4556-b2d0-803135201137)
![Screenshot (82)](https://github.com/user-attachments/assets/0e9379b1-2232-416a-bba8-233462c0569f)
In order to avoid these abnormalities, a technique called Power Planning is used. In this technique two different Power mashes are used, one for Vdd and another one for Ground.These meshes are prepared by using top two metal layers because they should have less voltage drop. These meshes will be spread across the design and are connected to multiple sources of Vdd and Ground.
![Screenshot (83)](https://github.com/user-attachments/assets/82c88af5-bf70-40c3-bea8-de02dbaee1ce)
With this technique whenever a cell needs power to switch from 0 to 1, it takes from nearest Vdd layer and if a cell needs to drain the power it will drain it to the nearest Ground Layer.
![Screenshot (84)](https://github.com/user-attachments/assets/636f8138-10c3-4886-ba68-f5c806c8ffe2)

## Pin Placement
Pin Placement is one of the crucial step in the design process. Bad pin placement results increase in the length of wire used for connectivity, which inturn results in some adverse affects. Pins should be placed in such a way that the required for connecting them to the blocks should be as less as possible. For example if an input pin is driving two blocks then that pin should be placed near to those two blocks.

Let's consider the below design
![image](https://github.com/user-attachments/assets/4be36da3-cda6-4003-8eb2-332303fb67dc)
For the above design the effective pin placement will look like as follows
![image](https://github.com/user-attachments/assets/12232ad8-f068-4bbf-ac12-8d54e5dcd2c1)

In the above pin placement, we can observe two things

* The order of input pins and output pins is random. As already mentioned the pins should be placed based on the connectivity not based on the order.
* The pins used for clock signals are larger in size when compared to pins used for signals, this is because clock is one of the important signal in the design and delays and voltage drops in the clock signal leads to failure of the chip. That is the reason why we use higher metal layers for routing the clock in the design.

After finishing the pin placement, we should use placement blockages outside of the core area and inside of the die area inorder to avoid placement and routing tool using that space for placement and routing, because it is the area dedicated only for Pin Placement purpose.
![image](https://github.com/user-attachments/assets/a2901a68-f6c4-4c4a-8754-ed9a9ef4dace)

## Steps to run floorplan using OpenLANE
For Floorplan to run smoothly, as a designer we should take care of some switches, that makes changes to the floorplan when changed. For example Utilization factor and aspect ratio are also part of switches. Designer should cross check these switches before initializing floorplan whether they are alligned with the project or not. Below image shows different types of switches in floorplan stage.

![Screenshot from 2024-07-28 03-36-47](https://github.com/user-attachments/assets/652aa591-e635-406f-807c-34e9b82acc2d)
![Screenshot from 2024-07-28 03-36-58](https://github.com/user-attachments/assets/1586c139-c439-44e4-8824-42e60eb839ae)
In the OpenLane lowest priority is given to system default(Floorplanning.tcl) and the second highest priority will be given to config.tcl and the highest priority will be given to PDK_varient.tcl for considering the values for switches.
![Screenshot from 2024-07-28 03-37-56](https://github.com/user-attachments/assets/fa3194a2-b6c0-4d02-a59f-39cbd9b66ffc)
## Review Floorplan files and steps to view floorplan
After confirming that all the switches are as per requirement, now we should executr a command run_floorplan , in order to start the floorplan stage.

If any errors occur during the floorplan stage it will display those errors.If it does not display any errors then our floorplan stage has succesfully completed.
![Screenshot from 2024-07-28 03-39-48](https://github.com/user-attachments/assets/d2bfb49e-21dc-4f82-8929-c9c4b283503f)
After completion of the floorplan we can check the report generated by the tool and check some of the aspects like die area etc.. , but in order to view the design in GUI we should use MAGIC tool.
In order to enter into the MAGIC tool we need to use the command

 **magic -T** **/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &**
![Screenshot from 2024-07-28 03-47-00](https://github.com/user-attachments/assets/9196b18a-0a2d-497d-b154-9e5a28279acf)
![Screenshot from 2024-08-02 03-23-12](https://github.com/user-attachments/assets/5e837074-3be6-41df-83ee-55361d53f04d)

![Screenshot from 2024-08-02 03-16-47](https://github.com/user-attachments/assets/4769f1c8-0dc7-418b-96c2-da33efbafabb)


## Library Binding and Placement
### Netlist binding and initial place design
In the netlist every element has its own shape, for example And gate has a different shape and or gate has a different shape. But in a library every element has only a square or rectangle shape. A Library consists of every elements that can be readily used and also the elements comes with their respective properties such as area, delay etc.. . We will have different versions of the same element with different properties.
![image](https://github.com/user-attachments/assets/aaf3a6f5-e2b6-493e-aa94-44d99f7ee950)
In the above picture we have 3 different sets of same elements. The elements which are larger in size are faster but occupies larger area and the smaller set will occupy less area but are slower when compared to larger ones.
![Screenshot (85)](https://github.com/user-attachments/assets/1946802a-cd75-4a0e-8c0f-997cdf2107e4)

### Optimize placement using estimated wire-length and capacitance
During Placement we should definitely consider the estimated wire length and place the cells according to it. Wire length is estimated by calculating the distance from input source of those cells and the distance to the output sinks that are being driven by them.

For above example tool will place the blocks by using the estimated wire length as shown in the below figure
![image](https://github.com/user-attachments/assets/09eeb7a1-3fdb-47df-9899-237d71db1077)

![Screenshot (86)](https://github.com/user-attachments/assets/26b65605-9409-42ba-a6bf-5effe500b790)

### Congestion aware placement using replace
After successful Floorplanning, the next step in the design process is Placement. Placement stage will consist of two stages

Global Placement - In Global Placement stage tool decides the places for all standard cells in the design.
Detailed Placement - In Detailed Placement stage the tool places all the standard cells in their designated places and legalization of the Placement will be done. Legalization is nothing but making sure that standard cells are not overlapped on each other in the design and are placed with in the site rows of the design.
In order to start the placement we need to use the command run_placement.

After Placement is done to check whether the cells are placed correctly or not, we need to check GUI and that will be done using MAGIC tool with the following command

**magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &**

![Screenshot from 2024-08-02 03-30-02](https://github.com/user-attachments/assets/5212b3c0-75f4-4e24-9b72-3b8a6f8445f6)

![Screenshot from 2024-08-02 03-30-18](https://github.com/user-attachments/assets/51655cab-432a-463a-aca9-6e12e24128d7)

![Screenshot from 2024-07-28 05-27-24](https://github.com/user-attachments/assets/0fea7fab-0cfb-400e-85df-e799aa2f5f60)

# Design library cell using Magic Layout and ngspice characterization
## Inception of Layout and CMOS fabrication process
### Lab steps to create std cell layout and extract spice netlist
We need to extract the spice netlist from the given Inverter from MAGIC Tool inorder to spice simulation in ngspice tool.
First we need to create an extraction file of the inverter. we can do this by using the command extract all in the tkcon window. This will create an extracted file in the vsdstdcelldesign directory.

![Screenshot from 2024-08-02 03-44-58](https://github.com/user-attachments/assets/227019c4-89d5-42ca-bc73-a740b1c9cadd)
![Screenshot from 2024-08-02 03-38-20](https://github.com/user-attachments/assets/23ac9dfb-d277-404b-8258-f13259f4ff78)
Next we need to create a spice file using this extracted file to use within the ngspice tool.For this the command will be
**ext2spice cthresh 0 rthresh 0,** this will not create any new file.

![Screenshot from 2024-08-02 03-38-16](https://github.com/user-attachments/assets/7389d6d4-1411-434f-84b0-b0e91cfbc31d)
![Screenshot from 2024-08-02 03-38-20](https://github.com/user-attachments/assets/5f6bbb99-40d2-4077-a186-b5e3bfe14ab1)

![Screenshot from 2024-08-02 21-42-42](https://github.com/user-attachments/assets/004cf878-7a12-464b-b2bb-39e3b8b6b027)
![Screenshot from 2024-08-02 04-11-43](https://github.com/user-attachments/assets/05f69a37-fb69-4233-bfc2-d8518eb5c278)
# SKY130 Tech File Labs
## Lab Steps to create final SPICE deck using Sky130tech
![Screenshot from 2024-08-02 22-17-44](https://github.com/user-attachments/assets/fee6e157-09b9-404c-93ea-9aee35b2a564)
After that we need to run the SPICE file in the ngspice tool by using the command **ngspice sky130_inv.spice**
![Screenshot from 2024-08-02 22-16-22](https://github.com/user-attachments/assets/d32ecd6c-3f2c-440a-9a87-faae6680451b)

![Screenshot from 2024-08-02 22-18-48](https://github.com/user-attachments/assets/3e83bec0-de87-40e7-9b45-29c0d71ffb32)

## Lab Steps to characterize the Inverter using sky130 model files
From the plot that we got from ngspice, we need to characterize four parameters of the Inverter.
Rise time : It is the time taken for the output waveform to go to 80% of its max value from 20% of its max value.
x0 = 6.16138e-09, y0 = 0.660007

x0 = 6.20366e-09, y0 = 2.64009

From the above values, Rise time = 0.0422 ns

Fall time : It is the time taken for the output to fall from 80% of its max value to 20% of its max value.

x0 = 8.04034e-09, y0 = 2.64003

x0 = 8.06818e-09, y0 = 0.659993

From the above values , Fall Time = 0.0278 ns

Propogation Delay : It is the time taken for the 50% of transition from 0 to 1 at the output for the 50% transistion from 1 to 0 at the input side.

## Introduction to Magic and steps to load Sky130 tech-rules
m3-layer

![Screenshot from 2024-08-02 23-20-17](https://github.com/user-attachments/assets/5c096c09-b3a8-4c0b-a02f-c33aa507f9f4)
m3- Peripheral Rules

![Screenshot from 2024-08-02 23-21-46](https://github.com/user-attachments/assets/51f1a4bb-93ae-47dd-b041-ae90409a6b92)

![Screenshot from 2024-08-02 23-32-15](https://github.com/user-attachments/assets/f11d0ebf-cd08-428e-b518-d49151354728)

## Fixing Poly 9 error in Sky130 tech file
![Screenshot from 2024-08-02 23-37-58](https://github.com/user-attachments/assets/b06bbd16-eda1-4280-8693-17a1a1cf8903)

![Screenshot from 2024-08-02 23-39-17](https://github.com/user-attachments/assets/98bfc43a-bb97-496a-9f08-26416263a30f)

Open the Sky130a.tech file, which is in the drc_tests directory and check for poly.9 keyword and make the changes that are shown in the images below and save it.

![Screenshot from 2024-08-02 23-44-07](https://github.com/user-attachments/assets/387e6ef5-d711-4b2a-adf3-ed76e6626083)

![Screenshot from 2024-08-02 23-57-35](https://github.com/user-attachments/assets/61e055a0-b844-4fe2-bfa1-90259f3e4d35)

Now again load the tech file by using the command tech **load sky130A.tech** , and again check drc by using command drc check in the tkcon terminal.

![Screenshot from 2024-08-02 23-59-43](https://github.com/user-attachments/assets/0a7325aa-06b8-4f1f-9cf8-a4b29569b3a2)

### Lab challenge exercise to describe DRC error as geometrical construct
Now load nwell.mag file into the magic and check for violations.
![Screenshot from 2024-08-03 00-12-26](https://github.com/user-attachments/assets/7cb5ca23-d7b5-4f97-b171-f3f5296acc3b)
![Screenshot from 2024-08-03 00-15-59](https://github.com/user-attachments/assets/aff3f7fc-1929-4b1a-9538-7345a3ffda81)
![Screenshot from 2024-08-03 00-21-54](https://github.com/user-attachments/assets/c495ee83-6c3c-4fae-a087-3b952b943381)
After updating the tech file load it again and check for errors

![Screenshot from 2024-08-03 00-35-11](https://github.com/user-attachments/assets/c28a5c1f-72a7-4876-a2ab-f6b1011eff65)

![Screenshot from 2024-08-03 00-38-37](https://github.com/user-attachments/assets/49fc1fda-848b-4c46-8197-dd2b6a99be4d)
Now after tapping the nwell violations are resolved.
# Pre-layout timing analysis and importance of good clock tree
## Timing modelling using delay tables
### Steps to convert grid info to track info
We will be needing LEF file of the Inverter cell. we need to extract if from the current Inverter cell.
From PNR point of view, while designing standard cell set two things must be considered
* The Input and output ports must lie on the intersection of the Vertical and Horizontal tracks.
* The width of the standard cell should be an odd multiple of the track pitch and height should be an odd multiple of track vertical pitch.
Open the tracks.info file to know more about tracks

![Screenshot from 2024-08-04 03-53-12](https://github.com/user-attachments/assets/7da4aafd-f516-4524-920d-a7bb16473680)

In the cell design input and output ports are on the li1 layer.We need to convert the grid into tracks.
Open the tkcon window and give the command for grid according to the track file.
Now we can see that both input and output ports are placed at the intersection of the tracks. Here our second condition also satisfies as 3 boxes are covered between the boundaries.
### Lab steps to convert magic layout to standard cell LEF
Now we need to extract the LEF file.First save .mag file by using the command save **sky130_vsdinv.mag** in the tkcon terminal.

![Screenshot from 2024-07-28 07-58-24](https://github.com/user-attachments/assets/5bc727e8-6d4e-4b54-bfe9-a4410d066e0c)

Now in the tkcon terminal use the command lef write in order to create a LEF file.

![Screenshot from 2024-08-02 20-32-24](https://github.com/user-attachments/assets/9e6d5728-17ab-4221-a200-2dc151479934)

### Introduction to timing libs and steps to include new cell in synthesis
To proceed futher lets keep all the required files at single place thet is in src directory. First copy the extracted LEF file into src directory.

After LEF file we need to copy the required libraries, here we will have different types of libraries such as fast , slow, typical etc.. , we need to copy all those .lib files to src directory by using cp command.
Now we need to make some changes in the .config file as shown in the image

![Screenshot from 2024-08-04 04-47-10](https://github.com/user-attachments/assets/9bf50fc7-214c-4e91-bf9f-f9f0ddc4d693)

After that we need to open bash using command docker being in openlane directory. And enter into the open lane and prepare the design as shown in figure. Once preparation is complete we need to use following commands

**set lefs [glob $::env(DESIGN_DIR)/src/*.lef]**

**add_lefs -src $lefs**
And now again use the command **run_synthesis** and check whether it maps our custom vsdinverter or not.

![Screenshot from 2024-08-05 19-55-31](https://github.com/user-attachments/assets/9e4173eb-d65a-45a1-b456-0a8f4596f9ec)

![Screenshot from 2024-08-05 23-44-57](https://github.com/user-attachments/assets/91d45a58-4180-4966-a846-75b2e6f94042)

### From the above figure we can see that synthesis was succesful and also we have 1554 instances of our vsdinverter. So this stage is successful.

Lab steps to configure synthesis settings to fix slack and include vsdinv
As we completed with synthesis stage, now we need to perform floorplan by using the following commands

**init_floorplan**

![Screenshot from 2024-08-05 23-58-20](https://github.com/user-attachments/assets/0a7866ab-bc1a-43c5-bf48-8a58dea2fab7)
**place_io**

![Screenshot from 2024-08-05 23-58-31](https://github.com/user-attachments/assets/f14abbfb-1ccd-49b4-b580-ab6d499dc88b)

**tap_decap_or**

![Screenshot from 2024-08-05 23-59-21](https://github.com/user-attachments/assets/fc043d93-d6d1-4aab-a152-52c373b31f0f)

Now as we done with Floorplan stage, we can proceed to placement stage by using the command **run_placement**

![Screenshot from 2024-08-06 00-14-13](https://github.com/user-attachments/assets/6670adbb-8762-4d73-ac49-7a26a90eb0fa)

Now after the placement is done,lets check whether the cell that we have created is placed in the design. For this being in the placement directory we should use the command

**magic -T** **/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &**

![Screenshot from 2024-08-06 00-24-22](https://github.com/user-attachments/assets/b1da4885-e4d7-4162-9f31-8550f279c1c7)

Clearly we can see that the cell that we have created " sky130_vsdinv" is placed in the design and now lets check whether it is alligned correctly with other cells or not by using the command expand in the tkcon terminal and it's alligned.

![Screenshot from 2024-08-06 00-55-13](https://github.com/user-attachments/assets/c961ba34-57c3-4263-963a-17207a804a3d)

## Timing analysis with ideal clocks using openSTA
### Steps to configure OpenSTA for post-synth timing analysis
Next step is to perform STA on the design. For this first we need to complete the synthesis stage. After synthesis is done some steps need to be followed.

First, we need to create a new file **pre_sta.conf** in the openlane directory.
After that we need to create another file called my_base.sdc in the src directory which is picorv32a directory.
![Screenshot from 2024-08-06 18-50-18](https://github.com/user-attachments/assets/d3b9b543-260d-4d5b-adbf-ad3e30dc1cf5)


![Screenshot from 2024-08-06 03-03-19](https://github.com/user-attachments/assets/f81f276e-1b45-42db-9941-72d90a9a66c9)
![Screenshot from 2024-08-06 03-26-02](https://github.com/user-attachments/assets/b8716f6d-7060-499c-a828-cddb0223a240)
![Screenshot from 2024-08-06 03-26-56](https://github.com/user-attachments/assets/bb1a51b6-6966-424b-b348-e8048c40129a)
![Screenshot from 2024-08-06 03-38-13](https://github.com/user-attachments/assets/383cc74c-ae03-4a77-b763-0c214fa7637d)
As we can see that Slack is equal to of that we got in synthesis stage. So STA is succesful.
## Clock tree synthesis TritonCTS and signal integrity
### Lab steps to run CTS using TritonCTS
After improving the timing of the design, the previous design should be replaced with improved design by using the command

**write_verilog //path of the previous design//**

Now the design will get updated with the improved version.

Now we can start working on it, starting with Floorplan by using the same commands that were used before. After succesful completion of Floorplan we should do placement by using the command **run_placement**.
![Screenshot from 2024-08-06 19-22-47](https://github.com/user-attachments/assets/10c41551-40fe-437d-9c75-cd0ea1ae6b95)
After placement is done, we can proceed with cts stage. To perform CTS we should use the command **run_cts**.
After completion of the cts we can observe that in the synthesis results directory a new .cts file is added. The newly added CTS file contains both the previous netlist and also the clock buffers that were added during the cts stage.
![Screenshot from 2024-08-06 19-27-38](https://github.com/user-attachments/assets/9897727d-c1f4-449a-a926-79f40782dca1)
![Screenshot from 2024-08-06 19-31-59](https://github.com/user-attachments/assets/c2246cde-9305-4ac3-925f-bf637a9f1080)
![Screenshot from 2024-08-06 19-32-35](https://github.com/user-attachments/assets/15096edb-e0a0-4d17-8876-b5e3420afa25)

## Lab steps to execute OpenSTA with right timing libraries and CTS assignment
To remove sky130_fd_sc_hd__clkbuf_1 from the list

**set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]**

To check the current value of CTS_CLK_BUFFER_LIST

**echo $::env(CTS_CLK_BUFFER_LIST)**

To check the current value of CURRENT_DEF

**echo $::env(CURRENT_DEF)**

To set def as placement def

**set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/04-05_21-50/results/placement/picorv32a.placement.def**

To run cts

**run_cts**

To check the current value of C**TS_CLK_BUFFER_LIST**

**echo $::env(CTS_CLK_BUFFER_LIST)**

![Screenshot from 2024-08-06 19-49-10](https://github.com/user-attachments/assets/3abac2a1-2085-4dd8-9599-69740c84c60a)
![Screenshot from 2024-08-06 19-51-24](https://github.com/user-attachments/assets/179ec5ea-5b49-494c-b9bb-0a5ecf84421f)

![Screenshot from 2024-08-06 20-11-17](https://github.com/user-attachments/assets/eedd1d7e-8e12-4bb0-90c7-0be2f1ef1cb0)
![Screenshot from 2024-08-06 20-12-38](https://github.com/user-attachments/assets/f9535580-6084-4c90-a3dd-b8924987dbc3)

![Screenshot from 2024-08-06 21-20-33](https://github.com/user-attachments/assets/3041a73c-1a0b-49b1-a1ff-3c3670f8b617)
### Steps to observe impact of bigger CTS buffers on setup and hold timing
We need to follow the similar steps that we have followed earlier in the openroad.

**read_lef /openLANE_flow/designs/picorv32a/runs/04-05_21-50/tmp/merged.lef**

**read_def /openLANE_flow/designs/picorv32a/runs/04-05_21-50/results/cts/picorv32a.cts.def**

**write_db pico_cts1.db**

**read_db pico_cts1.db**

**read_verilog /openLANE_flow/designs/picorv32a/runs/04-05_21-50/results/synthesis/picorv32a.synthesis_cts.v**

**read_liberty $::env(LIB_SYNTH_COMPLETE)**

**link_design picorv32a**

**read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc**

**set_propagated_clock [all_clocks]**

**report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4**

**report_clock_skew -hold**

**report_clock_skew -setup**

![Screenshot from 2024-08-06 21-41-39](https://github.com/user-attachments/assets/5ab2f092-27fc-4f59-86ff-acbf01a983b1)
![Screenshot from 2024-08-06 21-41-42](https://github.com/user-attachments/assets/6bbf2c48-ed6a-4523-beb8-3796c76e6e67)


![Screenshot from 2024-08-06 21-49-52](https://github.com/user-attachments/assets/200d14bb-ed6a-421b-bb44-18213b34934f)
## Power Distribution Network and routing
### Steps to build power distribution network
After completion of CTS, now we need to lay down power distribution network(PDN) for the design and it is done by using the command **gen_pdn** .
![Screenshot from 2024-08-06 21-51-53](https://github.com/user-attachments/assets/21679afe-9ad8-4ef9-9150-d780e260ee17)
![Screenshot from 2024-08-06 21-59-02](https://github.com/user-attachments/assets/f43f4472-19cd-4936-a925-5de613f1b48e)
### Steps from power straps to std cell power
In the below figure we can observe the path through which power is delivered all the way to standard cells.

The blocks that we can observe around the design are I/o pads and those with Red and Blue colours are Power Pads. Red pad is for power and the Blue one is for Gnd.

Those blocks are connected to Power and Gnd Rings, which go around the design and supply power to straps.

The vertical connections that we can observe for the rings are called Power Straps.

From Power straps and Rings connections will be made to the Power rails. The standard cell will be placed in between these power rails. The height of the standard cells should be the multiples of the pitch of the rails in order to get power and gnd supplies accurately.

This is the overview of the PDN structure.

![image](https://github.com/user-attachments/assets/607cdd73-39a2-4905-beb1-b78b9fb988f5)

## Basics of global and detail routing and configure TritonRoute
The Final stage in the flow is ROUTING. we can start routing by using the command **run_routing
**
![Screenshot from 2024-08-06 22-12-15](https://github.com/user-attachments/assets/7bffaeae-8270-4efa-b519-807202940972)
![Screenshot from 2024-08-06 22-10-46](https://github.com/user-attachments/assets/738b35e6-c934-4ab9-afb2-4676769317bd)
From tha above figures we can see that routing is done and it is done with 0 violations, So our routing is succesful but we can see the negative slack. We need to eliminate that negative slack for succesful completion of Physical design flow.

Acknowlegements 

Thank you Kunal Gosh for conducting the workshop and AnoushkaTripathi's clearly guided documentation on workshop has helped me complete the VSD_SoC workshop
