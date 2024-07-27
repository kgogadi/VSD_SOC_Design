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

##Get familiar to open-source EDA tools
###OpenLane Directory Structure
In order to access the OpenLane tool, we will be needing some basic linux commands.They are listed below

* pwd : It displays the present working directory and its path.
* cd : Using this command we can move in both ways in the directory tree.
* ls : It lists all the sub-directories and files present in the current directory.
* mkdir : Using this command, we can create a new directory.
* rmdir : Using his command, we can delete an existing directory.
* rm : This command is used to delete the files.
* help : using this command we can know the working of any command.
* clear : This command clears the terminal.

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

