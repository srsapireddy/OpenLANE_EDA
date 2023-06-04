# OpenLANE-Sky130-Physical-Design-Workshop
* This is the compilation of my notes for the 5 Day Workshop: Advanced Physical Design using OpenLANE/Sky130. The goal is to cover the complete RTL2GDS flow using the open-source flow OpenLane with SKY130nm PDK.

## Day 1
### 1. How to Talk to Computers?
![package](https://github.com/srsapireddy/Images/blob/main/1.PNG?raw=true) </br>
* The chip will be sitting at the center of the package. Chip is connected to the package, as shown in the figure. 
  * PADS – To send the signals inside the chip inside or outside the chip. </br>
  * CORE – Where all the digital logic is placed.
  * DIE – The size of the entire chip. Die will be manufactured on the silicon wafer.
![core](https://github.com/srsapireddy/Images/blob/main/2.PNG?raw=true) </br>
* Other details
  * Foundry – Place where the chips get manufactured.
  * IP (Intelligent Properties) – An Intellectual Property (IP) core in Semiconductors is a reusable unit of logic or functionality or a cell or a layout design that is normally developed with the idea of licencing to multiple vendor for using as building blocks in different chip designs. 
  * Macros – Digital logic blocks
  * RISC-V Instruction Set Architecture 
  * ISA – The way we interact with computers.
![macrovsip](https://github.com/srsapireddy/Images/blob/main/3.PNG?raw=true) </br>
* Suppose we want a C program to run on a particular layout. The C program is compiled into an assembly language program (RISC-V assembly language program). This assembly language program is converted into a machine language program (binary language program).  The bits here will be executed in a layout, and we will get the required output.
![riscvisa](https://github.com/srsapireddy/Images/blob/main/4.PNG?raw=true) </br>
* The interface between RISC-V architecture and the layout is hardware description language. We implement RISC-V specifications in RTL. 

### From software applications to hardware
* The application software enters a block called system software. The system software converts the application software into binary language. </br>
![software](https://github.com/srsapireddy/Images/blob/main/5.PNG?raw=true) </br>
### The flow of System Software:
 
1. OPERATING SYSTEM 
* OSHandles IO operations and allocates memory and low-level system functions. Converts the app into an assembly language program and finally into the binary language program so that the hardware understands it. 
The output of the operating system is the small functions in C, C++, Java, and VB.   

2. COMPILER 
* These functions are taken by the respective compiler and convert them into instructions. The syntax of the instructions will depend on the type of hardware used (If the hardware is ARM, then the instructions will be in the ARM format) - .exe file. 
* The instructions here will act as the abstract ISA between the hardware and the C programs. 
* ![stopwatch](https://github.com/srsapireddy/Images/blob/main/7.PNG?raw=true) </br>
* We need an RTL to implement instruction specifications. The RTL is converted into a synthesized netlist (high-level specification into synthesized netlist). This will be in the form of gates. Above netlist generation, we follow physical design implementation.
![stopwatch](https://github.com/srsapireddy/Images/blob/main/8.PNG?raw=true) </br>

3. ASSEMBLER
* The job of the assembler is to take the instructions and convert them into binary numbers (machine language program). Based on machine language programs, the hardware executes the functions and accordingly generates the output. </br>
Example: Stopwatch app
![stopwatch](https://github.com/srsapireddy/Images/blob/main/6.PNG?raw=true) </br>
### 2. SOC DESIGN AND OPENLANE
#### Introduction to all components of open-source digital ASIC design
* Digital ASIC design requires RTL IPs, EDA Tools (qflow, OpenROAD, OpenLANE), and PDK data.
  * What is a PDK? The interface between the FAB and the designers.
  * PDK – Process Design Kit
  * Collection of files used to model a fabrication process of EDA tools used to design IC.
    -	Process design rules: DRC, LVS, PEX
    -	Device Models
    -	Digital Standard Cell Libraries
    -	I/0 Libraries </br>
![foss](https://github.com/srsapireddy/Images/blob/main/9.PNG?raw=true) </br>
 
* Google releaseD open-source PDK for ASIC implementation using open-source or close source tools.
 ![distribution](https://github.com/srsapireddy/Images/blob/main/10.PNG?raw=true) </br>
* The fabrication process of SkyWater 130 nm is cheaper than advanced nodes. It covers over 6% of IC technology nodes used in the market. </br>
![130nm](https://github.com/srsapireddy/Images/blob/main/11.PNG?raw=true) </br>
 
### ASIC Flow objective: RTL to GDS II - Also called Automated PnR and/or Physical Implementation.
![tools](https://github.com/srsapireddy/Images/blob/main/12.PNG?raw=true) </br>
#### Simplified RTL to GDSII Flow
![flow](https://github.com/srsapireddy/Images/blob/main/13.PNG?raw=true)  </br>
#### 1. Synthesis
![Synthesis](https://github.com/srsapireddy/Images/blob/main/14.PNG?raw=true) </br>
* Converts RTL to a circuit out of components from the standard cell library (SCL). The resulting circuit is referred to as a gate-level netlist. Gate level netlist is functional equivalent to RTL. </br>
* The library building blocks, or the cells, have regular layouts. Typically, the cell layout is enclosed by a fixed-height rectangle. The cell width is variable but discrete. It is an integer of multiple units called site width.
* Standard cells have a regular layout – Each has different views/ models.
* Liberty View – Has an electrical model for the cells, such as delay and power models. 
* HDL and SPICE behavior views for the cells. Layout view for the cells. 
* GDS View, LEF view- abstract view.

#### 2. Floor planning and power planning
![fp](https://github.com/srsapireddy/Images/blob/main/15.PNG?raw=true) </br>
* The objective here is to plan the silicon area and create robust power distribution to the circuits. 
* Chip floor-planning: Partition the chip die between different system building blocks and place the I/P pads.
* Macro Floor Planning: Dimensions, pin locations, rows or routing tracks are definition.
![pp](https://github.com/srsapireddy/Images/blob/main/16.PNG?raw=true) </br>
* In power planning the power network is constructed.
* A chip is powered by multiple VDD and GND pins. The power pins are connected to all the components through rings and horizontal/ vertical power straps. Such parallel structures are meant to reduce the resistance hence the IR Drop, and to address the electromigration problem. Usually, the power distribution network uses upper metal layers as they are thicker than the lower metal layers. Hence have less resistance. 

#### 3. Placement: 
![Placement](https://github.com/srsapireddy/Images/blob/main/17.PNG?raw=true) </br>
* Place the cells on the floorplan rows aligned with the site rows. Connected cells are placed very close to each other to reduce the interconnect delay and allow successful routing afterward. 
* Done in two steps: 
  * Global - Tries to find the optimal placement of all the cells. Such positions are not necessarily legal. So cells may overlap or go off the sites. 
  * Detailed - The positions obtained from the global placement are minimally altered to be legal. 
![Placement_2](https://github.com/srsapireddy/Images/blob/main/18.PNG?raw=true) </br>
 
#### 4. Clock Tree Distribution:
![CTS](https://github.com/srsapireddy/Images/blob/main/19.PNG?raw=true) </br>
* Deliver the clock to all sequential elements (eg, FF) with minimum skew.
* Clock skew – The arrival of the clock to the different components at different times.

#### 5. Routing: 
![Routing](https://github.com/srsapireddy/Images/blob/main/20.PNG?raw=true) </br>

* Given the placement and fixed number of metal layers, it’s required to find a valid pattern of horizontal and vertical wires to implement the nets that connect the cells together. The routing uses the available metal layers defined by the PDK. The PDK defines the pitch, tracks, and minimum width. Also, it defines the vias that are used to connect the wire signals present on the different metal layers. 

* The Skywater 130 nm PDK defines six routing layers. The lowest layer is called the local interconnect layer (TiN layer). All the other metal layers are aluminum layers.  
  * Most of the routers are grid routers. They construct the routing grid out of the metal layer tracks. As the routing grid is huge, we follow the divide-and-conquer method for routing.
    - Global Routing: Generated routing guides.
    - Detailed Routing: Uses the routing guides to implement the actual wiring.
* Once done with routing, we can construct the final layout.

#### 6. Physical Verification: 
* DRC -  To make sure that the final layout follows the design rules.
* LVS – To ensure the final layout matches the gate level netlist.
* Timing Verification: 
  * STA – To make sure that all the timing constraints are met. And the circuit will run at a designated clock frequency.

### Files of Physical Design:
* Tech file: .tech contains the metal layer, connectivity between layers, DRC rules, and other definitions needed by Magic layout tool to view a single cell. </br>

* LEF file: .lef is combination of tech lef (contains metal layer geometries) and cell lef (contains geometries for all cells in the standard cell library). This lef file does not contain the logic part of cells, only the footprint that is needed by the PnR tool. </br>

* DEF file: .def is derived from LEF file and is used to transfer the design data from one EDA tool to another EDA tool and contains connectivity of cells of the design and is just a footprint (does not contain the logic part of cells) that the PnR needs. Each EDA tool to run will need to read first the LEF file runs/[date]/tmp/merged.nom.lef and the DEF file output of the previous stage's EDA tool (e.g. CTS EDA tool TritonCTS must first read DEF file from placement stage). So before running a stage, make sure the $::env(CURRENT_DEF) points to DEF file of previous stage or else there will be bunch of errors. </br>


### 2.3. Introduction to OpenLANE and Strive chipsets
* OpenLANE started as an open-source flow for a true open-source tape-out experiment.
* striVe is a family of open sourcing everything, including SoCs. (Open PDK, Open EDA, Open RTL)
![striVe](https://github.com/srsapireddy/Images/blob/main/21.PNG?raw=true) </br>

* OpenLANE main goal: To produce a clean GDSII with no human intervention. Tuned for SkyWater 130nm Open PDK. Also, support XFAB180 and GF130G. It can harden Macros and Chips to generate the final layout.
* Two modes of operation: 
  * autonomous – push button flow 
  * Interactive – run commends one by one to check intermediate steps.

### 4. OpenLANE ASIC DESIGN FLOW
![ASIC](https://github.com/srsapireddy/Images/blob/main/22.png?raw=true) </br>
### RTL Design
  * The flow starts with the design RTL and generating the final layout in GDSII format. OpenLANE is based on several open-source projects. </br>
![source](https://github.com/srsapireddy/Images/blob/main/23.PNG?raw=true) 
  * The flow starts with the RTL synthesis. The RTL is fed to yosys with the design constraints. Yosys translates the RTL to logic circuits. These circuits can be optimized and mapped into a standard cell library using the ABC tool. ABC should be guided during the optimization. This guidance will come in the form of the ABC script. OpenLANE comes with several open-source scripts. We refer to them as synthesis strategies. We have strategies to target the least area and for the best timing. Different designs can use different strategies to achieve the best objective. For this, we have synthesis exploration utility. This is used to generate a report that shows the design delay concerning the area effected by synthesis strategy. </br>
![strategies](https://github.com/srsapireddy/Images/blob/main/24.PNG?raw=true) </br>

### Design Exploration Utility
  * OpenLANE also has the design exploration utility. This can be used to sweep the design configurations. And generates a report, as shown below. </br>
![Exploration](https://github.com/srsapireddy/Images/blob/main/25.PNG?raw=true) </br>
  * This report shows the different design metrics. We have more than 35 of them in a report. It also shows the number of violations after generating the final layout. It is recommended to explore the design first and then use the best configurations for a particular design to result in a clean layout.
  * Also, design exploration can be used for regression testing (CI). So, we can run the design on several exploration configurations to get the best-optimized configurations. Currently, we have 70 designs. This utility will generate a report as shown below. </br>
![testing](https://github.com/srsapireddy/Images/blob/main/26.PNG?raw=true) </br>
  * This report shows the design metrics given different configurations and the number of violations. The results will be compared to the best-known results. 

### Design for Test
  * If we want our design to be tested after the fabrication, we can enable DFT. This step uses open-source project Fault. To perform scan insertion, ATPG, test patterns comparison, fault coverage, and fault simulation.

### Place and Route
  * After this comes the physical implementation, also called automated Place and Route, done by the OpenROAD application. 

### Logic Equivlence Check
  * We need to perform LEC checking using yosys. We compare the netlist resulting from the optimization step in PD flow with the gate-level netlist generated in synthesis. To make sure they are functionally equivalent. 
  * During the physical implementation, we have a step–fake antenna diodes insertion script to address the antenna rules violations. 

### ANTENNA Rules Violations
* Dealing with Antenna Rules Violations:
  * When a metal wire segment is fabricated, it can act as an antenna.
    -	Reactive ion etching causes charge to accumulate on the wire.
    -	Transistor gates can be damaged during fabrication.   
![ANTENNA](https://github.com/srsapireddy/Images/blob/main/27.PNG?raw=true)  </br>
 * Usually, this is the job of the router.
   * Solutions:
     1. Bridging – Attaches a higher layer intermediary. Requires the router awareness.
     2.  Antenna diode – Add antenna diode cell to leak away charges. Antenna diodes are provided by the SCL. 
![diode](https://github.com/srsapireddy/Images/blob/main/28.PNG?raw=true)  </br>
* Fake Antenna Diode Cells are used for every cell input after placement and run antenna checker (Magic) on the routed layout. If the checker reports a violation, we replace the fake diode with the real one. 

### STA, DRC and LVS
* The final steps in OpenLANE involve STA, DrC, and LVS.
  * Timing sign-off involves doing RC extraction from the routed layout using OpenSTA to highlight any timing violations, if there are any.
  * DRC – Magic has used for DRC and SPICE extraction from the layout.
  * LVS – Magic and Netgen are used for LVS. Extracted SPICE by MAGIC vs. Verilog netlist.

## Open Source EDA Tools
### Environment
* PDK - Process Design Kit
* PDK has the timing libraries, the LEF files, TECH files, and Cell LEF. 
open_pdks directory: These foundry files are compatible and made to work with commercial EDA tools. Open PDK mitigates the issue by using scripts and files to convert the foundry level PDKs to be compatible with the opensource EDA tools (like MAGIC, NetGen)
  * sky130A directory: The PDK that is made compatible with our open-source environment. Here sky130A is the PDK variant. </br>
  ![sky130A](https://github.com/srsapireddy/Images/blob/main/29.png?raw=true) </br>
  * libs.ref: contains all the process-specific files (timing, lef and cell lef)
  * libs.tech: specific to the tool </br>
  ![libs](https://github.com/srsapireddy/Images/blob/main/30.png?raw=true) </br>
* We are working with sky130_fd_sc_hd </br>
Here, sky130: process name, 130 nm
  - fd: for skywater foundry
  - sc: for standard cell library files
  - hd: variant of the PDK (hd: high density

![techlef](https://github.com/srsapireddy/Images/blob/main/31.png?raw=true) </br>
* techlef contains all the layer information.
* mag: all the MAGIC-related files
* lib: all the timing files containing all the process corners.

![corners](https://github.com/srsapireddy/Images/blob/main/32.png?raw=true) </br>
* tt: for typical, ss: for slow slow, ff: for fast fast and PVT corners
![lef_files](https://github.com/srsapireddy/Images/blob/main/32.png?raw=true) </br>
* lef files

* Tool environment directory: </br>
![tool_directory](https://github.com/srsapireddy/Images/blob/main/35.png?raw=true) </br>

### Designing Preparation Step
#### Running OpenLANE EDA </br>
![Running](https://github.com/srsapireddy/Images/blob/main/36.png?raw=true) </br>
   - Without the interactive switch, the tool will run the complete flow.

* Importing the required packages </br>
![packages](https://github.com/srsapireddy/Images/blob/main/37.png?raw=true) </br>
  - All the designs run by OpenLANE will be extracted from the design folder.
 
* Here we consider the picorv32a design </br>
![picorv32a](https://github.com/srsapireddy/Images/blob/main/38.png?raw=true) </br>
  - src: where the Verilog file for our RTL will be present. As well as the SDC information.
  - config.tcl: by default, characteristics of the design information are mentioned. This will override the default OpenLANE settings. 

![design](https://github.com/srsapireddy/Images/blob/main/39.png?raw=true) </br>
* Setting up the file system for the design: </br>
![Setting](https://github.com/srsapireddy/Images/blob/main/41.PNG?raw=true) </br>
  - Here the tech.lef (layer level information) and cell level lef merged into one.

#### Reviewing files after design prep and run synthesis
* Here the runs directory has been created after running command `run-synthesis`.
  * tmp folder: where the temporary files are stored, and the rest of the folders will be empty. </br>
![runs](https://github.com/srsapireddy/Images/blob/main/42.png?raw=true)  </br>
* config.tcl: show all the default parameters taken by the run.

* For synthesis: this will run the yosys and abc 
![synthesis](https://github.com/srsapireddy/Images/blob/main/43.png?raw=true)  </br>
* OpenLANE project Git link: https://github.com/efabless/openlane

* Calculating the D-Flip Flop count for the design: 1613/14876= 10.8%

* If we check the synthesis folder for results after running run_synthesis we can see that there is a new file generated named picorv32a.synthesios.v </br>
![results](https://github.com/srsapireddy/Images/blob/main/44.png?raw=true)  </br>
* To check the synthesis statistic report  </br>
![report](https://github.com/srsapireddy/Images/blob/main/45.png?raw=true)  </br>

## DAY 2
### Utilization factor and aspect ratio
* Define the width and height of the core and die. This is the first step in the physical design flow.
![report](https://github.com/srsapireddy/Images/blob/main/46.PNG?raw=true)  </br>
* Netlist defines the connectivity between all the components. 
* In defining the dimensions of the chip, it mostly depends on the dimensions of the logic gates. 

![Utilization](https://github.com/srsapireddy/Images/blob/main/47.PNG?raw=true)  </br>
* To get the dimensions of the core and die, we are interested in the dimensions of the standard cells, not considering the wires in between them.
* Standard cells are given rough dimensions as shown below.

![calculate](https://github.com/srsapireddy/Images/blob/main/48.PNG?raw=true)  </br>
* Let’s calculate the area occupied by the below netlist on a silicon wafer by bringing the cells together. </br>
 
* What is the core and die section of a chip? </br>
![core](https://github.com/srsapireddy/Images/blob/main/49.PNG?raw=true)  </br>
  * Core: where we place the fundamental logic.
  * Die piece of the area where our circuit is built so that our circuit will not exceed this area. We imprint this die multiple times on a silicon wafer to increase its throughput. </br>
![die](https://github.com/srsapireddy/Images/blob/main/50.PNG?raw=true)  </br>
 
* As we can see that the netlist of 4 sq units occupies a complete area of the core. This means we have utilized the core 100%. </br>
![units](https://github.com/srsapireddy/Images/blob/main/51.PNG?raw=true)  </br>

* Utilization Factor: Area occupied by netlist/ Total area of the core = 4 x 1sq.unit/ 2 unit x 2 unit = 1
  * In practice, we go for a 50 to 60% utilization.
* Aspect Ratio = Height/ width = 2 unit/ 2 unit = 1
* When the aspect ratio is 1, the chip is squared in shape.
Ex:
![Factor](https://github.com/srsapireddy/Images/blob/main/52.PNG?raw=true)  </br>

### Concept of pre-placed cells
![placed](https://github.com/srsapireddy/Images/blob/main/53.PNG?raw=true)  </br>
* The figure above shows that the utilization factor is 25%, and 75% is utilized for the optimization. 
  - On top of this, we do the routing.
![routing](https://github.com/srsapireddy/Images/blob/main/54.PNG?raw=true)  </br>

* It is supposed to be a squared chip where we have the aspect ratio as 1.

#### Locations of pre-placed cells:
* The cells can be divided into two blocks, as shown in the figure below. These blocks are implemented separately.
![blocks](https://github.com/srsapireddy/Images/blob/main/55.PNG?raw=true)  </br>
![cells](https://github.com/srsapireddy/Images/blob/main/56.PNG?raw=true)  </br>

* These blocks are invisible if we look from the top. Looking into the main netlist. 
![netlist](https://github.com/srsapireddy/Images/blob/main/57.png?raw=true)  </br>

* These blocks can be used multiple times in the netlist. This concept is useful when we want the blocks to be reused. Similarly, we can have other IPs also available.
* All these blocks can be implemented once and can be instantiated multiple times on to a netlist.
![instantiated](https://github.com/srsapireddy/Images/blob/main/58.PNG?raw=true)  </br>
* The placement of these cells on the top-level netlist is fixed. They are called pre-placed cells since they are placed before placement and routing. Once these locations are placed, they are not moved by automated placement or routing tools. 

### De-coupling Capacitors
* These pre-placed cells should be surrounded by de-coupling capacitors.
 
#### De-coupling Capacitors
![power](https://github.com/srsapireddy/Images/blob/main/61.jpg?raw=true) </br>
* If there is a large distance between the power supply and a circuit in the design, there might be a voltage drop across the wire from vdd to the circuit due to the resistance, inductance, and capacitance of the wire being used. This problem is solved using de-coupling capacitors.   
![noise](https://github.com/srsapireddy/Images/blob/main/60.PNG?raw=true)  </br>
* De-coupling capacitors are huge capacitors that are filled with charge. The voltage across the de-coupling capacitor is like that across the power supply. Whenever the circuit switches, the de-coupling capacitance provides a power supply to the circuit. This means the de-coupling capacitor de-couples the circuit from the main supply. Whenever the switching happens, the de-coupling capacitor will send the current to the circuit. These decoupling capacitors are placed close to the circuit to reduce the voltage drop. The de-coupling capacitor supplies the amount of current needed by the circuit during switching. 
We use the de-coupling capacitor to charge the circuit. Whenever there is a switching activity, the de-coupling capacitor loses some charge to the circuit. Whenever there is no switching activity, the de-coupling capacitor replenishes its charge from the power supply.</br>
![Capacitors](https://github.com/srsapireddy/Images/blob/main/59.PNG?raw=true)  </br>
* This also reduces the crosstalk problem. 

### Power Planning
* Consider a scenario where a signal is sent from the driver to load. We must ensure the load receives the exact shape of the signal. 
* Tapping of blocks to Vdd, and all the block ground lines are tapped to the ground.
![Power](https://github.com/srsapireddy/Images/blob/main/62.PNG?raw=true) </br>

* To retain the same signal from the driver to the load, we need the power supply. In this region, we don’t have any de-coupling capacitor that will take the power supply switching. So, there is a possibility of voltage drop across the line (shown in orange). Let’s assume that the line from driver to load is a 16-bit bus. </br>
![supply](https://github.com/srsapireddy/Images/blob/main/63.PNG?raw=true) </br>

* The capacitors shown here are charged to Vdd for logic 1 and GND for logic 0. When we pass this 16-bit bus as input to the inverter, we get the inverted output. So all the capacitors with logic 0 as output is discharged, and with logic 1 will be charged to Vdd. 
![discharged](https://github.com/srsapireddy/Images/blob/main/64.PNG?raw=true) </br>

* We have a single ground line for the 16-bit bus. Due to this, there is a bump over the ground line. If the size of the bump exceeds the noise margin, it will enter an undefined state. Due to the undefined state, it might logic 1 or logic 0. This phenomenon is called a ground bounce. 
* When all the capacitance is charged from logic 0 to logic 1 we have a voltage droop as all the capacitors are demanding the power supply at the same time. Here the voltage droop can get into to undefined region.
![logic](https://github.com/srsapireddy/Images/blob/main/65.PNG?raw=true) </br>

* The problem arises as the power supply is provided from one point. 
  * Solution:
    - Instead of having single power supply we need to have multiple power supplies.
![multiple](https://github.com/srsapireddy/Images/blob/main/66.PNG?raw=true) </br>

* Then the current demanded by the circuit can be taken from the nearest power supply. Or it will drop the current to the nearest ground. This is the reason we have multiple power and ground pins in chips.
![current](https://github.com/srsapireddy/Images/blob/main/68.PNG?raw=true) </br>

### Pin placement and logical cell placement blockage
![placement](https://github.com/srsapireddy/Images/blob/main/69.png?raw=true) </br>
* The connectivity information between the gates is coded using VHDL/ Verilog language called the “netlist.”
* The placement of input and output ports depends according to the cell placement in the core. And no FFs can be placed in this area where the blocks are placed. The backend team must decide on the pin placement. 
* The CLK ports are bigger in size than the data ports. The reason for this is the CLK is the one driving all the FFs in the core. So, we need the least resistance path for clocks 1 and 2. Bigger the size lowers the resistance. 
* We do logical placement blockage for blocking the area so that the automated place and route tool doesn’t place cells in this area, as this area is reserved for the pin locations. 

### Running floorplan in OpenLANE
* Standard cells placement is done in the placement stage.
![multiple](https://github.com/srsapireddy/Images/blob/main/70.png?raw=true) </br>

* Here we have all the variables for the floorplan stage in the READ.md file.
* These variables are also called as switches in the floorplan stage.
![multiple](https://github.com/srsapireddy/Images/blob/main/71.png?raw=true) </br>
 
* All the default values of the variables are shown here.
* To check all the variables associated with all PD stages.
* ![multiple](https://github.com/srsapireddy/Images/blob/main/72.png?raw=true) </br>
 
 
* Default parameters set for floorplan stage in OpenLANE
![multiple](https://github.com/srsapireddy/Images/blob/main/73.png?raw=true) </br>

 
* The lowers priority will be given to system defaults (floorplan.tcl in configurations folder), and height priority will be given to config.tcl and then sky120A_fd_sc_hd_config.tcl for setting floorplan stage variables.
![multiple](https://github.com/srsapireddy/Images/blob/main/74.png?raw=true) </br>

* Contents of config.tcl file
![multiple](https://github.com/srsapireddy/Images/blob/main/75.png?raw=true) </br>

* In OpenLANE flow, the vertical and horizontal metal is one more than what we specify here.
* After changing the config.tcl file with core utilization of 65%, clock period 10 and vertical metal as 4, and horizontal metal as 3.</br>
![multiple](https://github.com/srsapireddy/Images/blob/main/76.png?raw=true) </br>

* Command to run floorplan: `run_floorplan`

### Floorplan Files
![multiple](https://github.com/srsapireddy/Images/blob/main/77.png?raw=true) </br>
* Check for the variable settings
![multiple](https://github.com/srsapireddy/Images/blob/main/78.png?raw=true) </br>
* The highest priority will be given to sky130_sky130_fd_sc_hd_config.tcl. So, the core utilization should be changed here—then config.tcl, and then the system default variable values will be given priority for setting variables in .tcl files.
* To check the floorplan results – looking at the def file, we don’t know where what is placed.
![multiple](https://github.com/srsapireddy/Images/blob/main/79.png?raw=true) </br>
* To see the actual layout after the floorplan
* Command to run magic layout: `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/bibs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def`
![multiple](https://github.com/srsapireddy/Images/blob/main/81.PNG?raw=true) </br>

![floorplan](https://github.com/srsapireddy/Images/blob/main/82.png?raw=true) </br>
### Review floorplan layout in MAGIC
![layout](https://github.com/srsapireddy/Images/blob/main/83.png?raw=true) </br>
* To check vertical metal type </br>
![MAGIC](https://github.com/srsapireddy/Images/blob/main/84.png?raw=true) </br>
![vertical](https://github.com/srsapireddy/Images/blob/main/85.png?raw=true) </br>
* Tap Cells: Avoids latch-up conditions in the CMOS devices. They connect n-well to Vdd and substrate to the ground. </br>
![Tap](https://github.com/srsapireddy/Images/blob/main/86.png?raw=true) </br>
* Standard cell are placed during the placement stage

### Placement and Routing
* Here binding the netlist with physical cells takes place. All the gates have a physical view while representing the core like an FF, which is square in shape.
* In the real world, we give physical dimensions for all the gates by giving width and height. So every component of the netlist is given a proper width and height.
![Placement_1](https://github.com/srsapireddy/Images/blob/main/87.PNG?raw=true) </br>
* And the block already has the proper shape.
![Placement_2](https://github.com/srsapireddy/Images/blob/main/88.PNG?raw=true) </br>

* The dimensions of the cells in a design are present in a library. Library also have the timing information of all the gates. 
  * Library files are divided into two categories:
    1.	Library files containing the shapes and sizes of cells.
    2.	Timing information of the cells.
* It also contains the various shapes of all the gates. Gates that are bigger and have the least resistance path. And it will be faster.
![Placement_3](https://github.com/srsapireddy/Images/blob/main/89.PNG?raw=true) </br>

### Placement
![Placement_4](https://github.com/srsapireddy/Images/blob/main/90.PNG?raw=true) </br>
* The library has various flavors of cells in the library. We select these cells based on the timing conditions (delay information) and the size of the floorplan requirements. 
Placement.
![Placement_5](https://github.com/srsapireddy/Images/blob/main/91.PNG?raw=true) </br>
* We must place the physical view of the netlist for the placement. We placed the cells according to the logical connectivity and placed them close together concerning the input and output pin placement.

* The cells are placed according to the input and output ports of the circuit, as shown in the figure. We estimate the wire length and capacitance, and based on that, we insert repeaters.
* If we have a large wire, we have huge resistance and capacitance associated with that wire. To maintain the signal integrity, we will reproduce the signal using buffers which are called repeaters.
![Placement_5](https://github.com/srsapireddy/Images/blob/main/92.png?raw=true) </br>
* Based on the wire length estimation, we calculate the capacitance using the transition of a waveform using timing analysis. The higher the value of the capacitor, the amount of charge required to charge the capacitor will be high and have the worst slew.
* We use buffers from Din2 to FF1, which will receive the proper signal even though we have a large wire length.
* The internal routes of FF2 (Yellow) are in some layers, say metal 1 and metal 2. And the connection from 1 to 2 (green) will be on metal 3.
* Based on the ideal conditions of the clock, we do a timing analysis. Based on this setup timing analysis, we will determine whether the placement is reasonable based on the given specifications.  

### Need for library characterization:
1. Logic Synthesis: Convert functionality into legal hardware. The output of logic synthesis is an arrangement of gates that represent the original functionality described using an RTL. This is the proper connection of the gates to represent the original functionality.2
2. Floorplanning: We import the netlist that we get from logic synthesis and decide the size of the core and the die. So the width and height of the core and die are dependent on the number of gates and their sizes. 
3. Placement: We place the logic cells in the chip in such a fashion that the initial timing is met. 
4. CTS: We want the clock signal to be spread to the logic cells simultaneously at an equal time. The buffers in the figure will take care of the clock signal has got equal rise and fall times.
5. Routing: Routing the cells. There are certain properties of the cell that should be taken care of while routing.
6. STA: Here, we find the maximum achievable frequency of the circuit. 
![library_2](https://github.com/srsapireddy/Images/blob/main/93.PNG?raw=true) </br>
![library](https://github.com/srsapireddy/Images/blob/main/94.PNG?raw=true) </br>
* One thing that is common across all stages is GATES or Cells. The collection of these cells is referred to as library. The EDA tool needs to understand are timing characteristics of the gate and how it is represented in a tool.
![librar#](https://github.com/srsapireddy/Images/blob/main/95.PNG?raw=true) </br>

### OpenLANE Placement
* Placement takes place in 2 stages:
1.	Global Placement: Corse placement and there is no legalization. 
Legalization: The standard cell should be placed inside the standard cell rows, with no overlaps. We require legalization from a timing point of view. 
2.	Detailed placement
* Command: `run_placement`
* Then global placement happens. In OpenLANE, the placement takes place using a half-parameter wire length. Our objective is to reduce the overflow. If the overflow value converges, we can assume the placement is right.
* Command to run magic layout: `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/bibs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def`
![librar#](https://github.com/srsapireddy/Images/blob/main/96.png?raw=true) </br>
* Then we need to check our design after placement:
![librar#](https://github.com/srsapireddy/Images/blob/main/98.png?raw=true) </br>
* We know that in placement the standard cells are placed and fixed. 
* The power and ground network will be created during the placement. 

* Standard cells are placed in a section called a library. We also have DECAP cells and MACROS placed in the library. 

* The library also contains different gates with different functionality. It also got cells of different sizes. 
Based on the sizes of cells, we can decide the drive strength of cells. The bigger cells have larger drive strength. For example, the smaller buffers have the least drive strength. It also contains cells with different threshold voltages. The variation in threshold voltage decides the speed of the cell. For example, a 0.4 Vth inverter takes more time to switch than a 0.3 Vth inverter.


### CELL DESIGN FLOW
A cell, for example, an inverter, should follow a cell design flow.
Steps:
1.	Inputs: PDKs (DRC and LVS rules, SPICE models, library, and user-defined specs) 
![FLOW_](https://github.com/srsapireddy/Images/blob/main/99.PNG?raw=true) </br> 
SPICE model parameters: We get these parameters from the foundry.</br> 
![FLOW_2](https://github.com/srsapireddy/Images/blob/main/100.PNG?raw=true) </br>

### Library and User-defined Specs:
* The separation between the power rail and the ground rail decides the cell height. The cell width is dependent on the timing information. This depends on the drive strength of the cell. Lowering the value of the drive strength of the cell can drive a few other cells at the output. 

* The library developer must consider the supply voltage to design the cell, and they also need to take care of noise margin levels to take care of the supply voltage of the cell.
* Certain libraries should be built on certain metal layers. This should be taken into consideration.
* Pin locations are also important for developing a library. 
![FLOW_3](https://github.com/srsapireddy/Images/blob/main/101.PNG?raw=true) </br>
 
The library developer is responsible for taking these inputs and developing a library cell that adheres to these inputs.
2.	Design steps: circuit design, layout design, and characterization
Circuit Design: We should design PMOS and NMOS transistors in such a fashion to meet the library requirements. These are mostly based on SPICE simulations.
![FLOW_4](https://github.com/srsapireddy/Images/blob/main/102.PNG?raw=true) </br>
![FLOW_5](https://github.com/srsapireddy/Images/blob/main/103.PNG?raw=true) </br>

* Characterization: This is a step to get timing, noise, and power information (power.libs) and circuit functionality.

* Layout Design: 
![layout_6](https://github.com/srsapireddy/Images/blob/main/104.PNG?raw=true) </br>
* First, we need to implement the function and derive the pmos and nmos network graphs. 
* Art of layout – Euler’s path + stick diagram
* Euler’s path: The path that is traced out only once.
![layout_6](https://github.com/srsapireddy/Images/blob/main/105.PNG?raw=true) </br>
* Based on Euler’s path we need to draw a stick diagram out of it. 
![layout_6](https://github.com/srsapireddy/Images/blob/main/106.png?raw=true) </br>
* Then we convert this stick diagram into a layout that adheres to the DRC rules given by the foundry.
![layout_6](https://github.com/srsapireddy/Images/blob/main/107.png?raw=true) </br>

* Typical layout from MAGIC Design tool
* From the layout, we can calculate cell width and cell height.
* Then we need to extract parasitic out of the layout and characterize that in terms of timing.

3. CDL File: Circuit Description Language
* Output from a layout is GDSII, LEF (defines the width and height of the cell), extracted spice netlist (.cir)- R, C of every element in the circuit.

### Typical characterization flow
![characterization_1](https://github.com/srsapireddy/Images/blob/main/108.PNG?raw=true) </br>
![characterization_2](https://github.com/srsapireddy/Images/blob/main/109.PNG?raw=true) </br>
Steps:
1.	Read the model file.
2.	Read extracted SPICE netlist.
3.	Recognize the behavior of the buffer.
4.	Read the subcircuits of the inverter.
5.	Attach the necessary power sources.
6.	Apply the stimulus.
7.	Vary output load capacitance.
8.	Provide necessary simulation commands.
9.	Feed in all these inputs through a configuration file to characterization software called GUNA.
![characterization_3](https://github.com/srsapireddy/Images/blob/main/110.png?raw=true) </br>
* GUNA – Generates timing, noise, and power.libs

Timing Characterization
Timing threshold definitions: variables related to waveform
- slew_low_rise_thr: points toward the lower side of the power supply. (About 20%)
![Timing_1](https://github.com/srsapireddy/Images/blob/main/111.PNG?raw=true) </br>
- slew_high_rise_thr: points towards the higher side of the power supply. (About 20%)
![Timing_2](https://github.com/srsapireddy/Images/blob/main/112.PNG?raw=true) </br>
- slew_low_fall_thr
![Timing_3](https://github.com/srsapireddy/Images/blob/main/113.PNG?raw=true) </br>
- slew_high_fall_thr
![Timing_4](https://github.com/srsapireddy/Images/blob/main/114.PNG?raw=true) </br>
- in_rise_thr: Related to the input waveform. The threshold for the input waveform delay.  (About 50%)
![Timing_5](https://github.com/srsapireddy/Images/blob/main/115.PNG?raw=true) </br>
- in_fall_thr
![Timing_6](https://github.com/srsapireddy/Images/blob/main/116.PNG?raw=true) </br>
- out_rise_thr: Related to the output waveform. The threshold for the output waveform delay.  (About 50%)
![Timing_8](https://github.com/srsapireddy/Images/blob/main/117.PNG?raw=true) </br>
- out_fall_thr
![Timing_7](https://github.com/srsapireddy/Images/blob/main/118.PNG?raw=true) </br>

* Below are the timing variables for slew. This is two inverters in series, red is output of first inverter and blue is output of second inverter:
![Timing_9](https://github.com/srsapireddy/Images/blob/main/119.png?raw=true) </br>
* Below are the timing variables for propagation delay. The red is input waveform and blue is output waveform of the buffer. The left side is rise delay and right side is fall delay.
![Timing_10](https://github.com/srsapireddy/Images/blob/main/120.png?raw=true) </br>
* Negative propagation delay is unexpected. That means the output comes before the input so designer needs to choose correct threshold point to produce positive delay. Delay threshold is usually 50% and slew rate threshold is usually 20%-80%.

### IO Placer
* PnR is an iterative flow.
* Run synthesis and floorplan steps.
* How to change input and output pins along the core?
* After floorplan input and output pins are placed randomly around the core area.
![Placer_1](https://github.com/srsapireddy/Images/blob/main/121.png?raw=true) </br>
* Open the MAGIC layout tool to check the floorplan of the design
![Placer_2](https://github.com/srsapireddy/Images/blob/main/122.png?raw=true) </br>
* From the figure, we can see that the pins are placed at equal distances with respect to each other.
![Placer_3](https://github.com/srsapireddy/Images/blob/main/123.png?raw=true) </br>
* IO placer supports 4 strategies to place our IO pins.
![Placer_4](https://github.com/srsapireddy/Images/blob/main/124.png?raw=true) </br>
![Placer_5](https://github.com/srsapireddy/Images/blob/main/125.png?raw=true) </br>
* From floorplan.tcl we can see that the IO pins are placed with mode 1 with `set ::env(FP_IO_MODE)` variable. With mode 1 all IO’s are placed with equal distance.
* Set this variable to 2 and run the floorplan again.
* Then again check the IO placement with the MAGIC layout tool.
![Placer_6](https://github.com/srsapireddy/Images/blob/main/126.png?raw=true) </br>
* From the layout, we can see that the IO pins are stacked on top of one another.

### VTC – SPICE Simulations (Spice deck creation for CMOS inverter)
* SPICE deck </br>
![VTC_1](https://github.com/srsapireddy/Images/blob/main/127.png?raw=true) </br>
1. connectivity information about the netlist. It also has the inputs that are to be provided to the simulation and tap points from where we collect the output.
In the SPICE deck, we need to mention the connectivity of the substrate too. 
Here we take the example of the inverter and assume the Cload value is 10fF.
2. Define component values: The values for PMOS and NMOS. Ideally, the size of PMOS should be bigger than the NMOS. Define the values of input gate voltage. The voltages are kept in the multiples of channel length. Also, assume the supply voltage is 2.5V.
3. Identify the nodes: Those two points between which there is a component.
4. Name nodes </br>
![VTC_2](https://github.com/srsapireddy/Images/blob/main/128.png?raw=true) </br>
![VTC_2](https://github.com/srsapireddy/Images/blob/main/129.png?raw=true) </br>

### Running ngspice
![VTC_2](https://github.com/srsapireddy/Images/blob/main/130.png?raw=true) </br>
* SPICE waveform conditions: Wn=Wp=0.375u, Ln,p=0.25u device (Wn/Ln=Wp/Lp=1.5)
![VTC_2](https://github.com/srsapireddy/Images/blob/main/131.png?raw=true) </br>
* SPICE waveform conditions: Wn=0.375u, Wp=0.9375u, Ln,p=0.25u device (Wn/Ln=1.5, Wp/Lp=2.5)
![VTC_2](https://github.com/srsapireddy/Images/blob/main/132.png?raw=true) </br>

### CMOS robustness depends on:
* Switching threshold = Vin is equal to Vout. This the point where both PMOS and NMOS is in saturation or kind of turned on, and leakage current is high. If PMOS is thicker than NMOS, the CMOS will have higher switching threshold (1.2V vs 1V) while threshold will be lower when NMOS becomes thicker.
* Propagation delay = rise or fall delay
* DC transfer analysis is used for finding switching threshold. SPICE DC analysis below uses DC input of 2.5V. Simulation operation is DC sweep from 0V to 2.5V by 0.05V steps

### Dynamic Simulation of CMOS Inverter
![Dynamic_1](https://github.com/srsapireddy/Images/blob/main/133.png?raw=true) </br>
* Here we do a transient analysis of a CMOS inverter with a pulse as an input waveform.
![Dynamic_2](https://github.com/srsapireddy/Images/blob/main/134.png?raw=true) </br>
* Calculating rise delay: Here, we need to find a point where output rises
![Dynamic_3](https://github.com/srsapireddy/Images/blob/main/135.png?raw=true) </br>
* Select points at 1.25V on the graph for input pulse and output waveform.
  - Rise delay = around 148 ps delay (subtracting x values)
  - ![Dynamic_4](https://github.com/srsapireddy/Images/blob/main/136.png?raw=true) </br>

* Calculating fall delay: Here, we need to find a point where output falls
![Dynamic_5](https://github.com/srsapireddy/Images/blob/main/137.png?raw=true) </br>
  - Fall delay = around 71 ps delay (subtracting x values)
  - ![Dynamic_6](https://github.com/srsapireddy/Images/blob/main/138.png?raw=true) </br>

### Standard cell design and characterization using openlane flow GitHub repo: </br> https://github.com/nickson-jose/vsdstdcelldesign
* Copying sky130A.tech files to git repo folder </br>
![Dynamic_7](https://github.com/srsapireddy/Images/blob/main/139.png?raw=true) </br>
* Verify if the tech file is copied </br>
![Dynamic_8](https://github.com/srsapireddy/Images/blob/main/140.png?raw=true) </br>
* Open inverter circuit with MAGIC layout tool with sky130A.tech file
 - Command: `magic -T sky130A.tech sky130_inv.mag &`
![Dynamic_9](https://github.com/srsapireddy/Images/blob/main/141.png?raw=true) </br>

### CMOS Fabrication Process (16-Mask CMOS Process):
1. Selecting a substrate = Layer where the IC is fabricated. Most commonly used is P-type substrate
2. Creating active region for transistor = Separate the transistor regions using SiO2 as isolation

Mask 1 = Covers the photoresist layer that must not be etched away (protects the two transistor active regions)
Photoresist layer = Can be etched away via UV light
Si3N4 layer = Protection layer to prevent SiO2 layer to grow during oxidation (oxidation furnace)
SiO2 layer = Grows during oxidation (LOCOS = Local Oxidation of Silicon) and will act as isolation regions between transistors or active regions
![cell_1](https://github.com/srsapireddy/Images/blob/main/142.png?raw=true)

3. N-Well and P-Well Fabrication = Fabricate the substrate needed by PMOS (N-Well) and NMOS (P-Well)

Phosporus (5 valence electron) is used to form N-well
Boron (3 valence electron) is used to form P-Well.
Mask 2 protects the N-Well (PMOS side) while P-Well (NMOS side) is being fabricated then Mask 3 while N-Well (PMOS side) is being fabricated
![cell_2](https://github.com/srsapireddy/Images/blob/main/143.png?raw=true) </br>

4. Formation of Gate = Gate fabrication affects threshold voltage. Factors affecting threshold voltage includes:

![cell_3](https://github.com/srsapireddy/Images/blob/main/144.png?raw=true) </br>

Main parameters are:

Doping Concentration = Controlled by ion implantation (Mask 4 for Boron implantation in NMOS P-Well and Mask 5 for Arsenic implantation in PMOS N-Well)
Oxide capacitance = Controlled by oxide thickness (SiO2 layer is removed then rebuilt to the desire thickness)
Mask 6 is for gate formation using polysilicon layer.

![cell_4](https://github.com/srsapireddy/Images/blob/main/145.png?raw=true) </br>

5. Lightly Doped Drain formation = Before forming the source and drain layer, lightly doped impurity is added:

Mask 7 for N- implantation (lightly doped N-type) for NMOS
Mask 8 for P- implantation (lightly doped P-type) for PMOS.
Heavily doped impurity (N+ for NMOS and P+ for PMOS) is for the actual source and drain but the lightly doped impurity will help maintain spacing between the source and drain and prevent hot electron effect and short channel effect.
![cell_5](https://github.com/srsapireddy/Images/blob/main/146.png?raw=true) </br>

6. Source and Drain Formation = Mask 9 is for N+ implantation and Mask 10 for P+ implantation

Channeling is when implantations dig too deep into substrate so add screen oxide before implantation
The side-wall spacers maintains the N-/P- while implanting the N+/P+
![cell_6](https://github.com/srsapireddy/Images/blob/main/147.png?raw=true) </br>

7. Form Contacts and Interconnects = TiN is for local interconnections and also for bringing contacts to the top. TiS2 is for the contact to the actual Drain-Gate-Source. Mask 11 is for etching off the TiN interconnect for the first layer contact.

![cell_7](https://github.com/srsapireddy/Images/blob/main/148.png?raw=true) </br>

8. Higher Level Metal Formation = We need to planarize first the layer via CMP before adding a metal interconnect. Aluminum contact is used to connect the lower contact to higher metal layer. Process is repeated until the contact reached the outermost layer.

Mask 12 is for first contact hole
Mask 13 is for first Aluminum contact layer
Mask 14 is for second contact hole
Mask 15 is for second Aluminum contact layer. Mask 16 is for making contact to topmost layer.
![Cell_8](https://github.com/srsapireddy/Images/blob/main/149.png?raw=true) </br>

### MAGIC Inverter Layout
* In MAGIC the first layer is the local interconnect layer (locali). 
  - metal 1: purple color
  - metal 2: pink color
  - nwell: solid dashed lines
  - ndiff: green color
  - pdiff:   brown color
* When a poly crosses a ndiff it’s a nmos. Similarly, when a ploy crosses a pdiff it’s a pmos.
![Image](https://github.com/srsapireddy/Images/blob/main/150.png?raw=true) </br>
NMOS </br>
![Image](https://github.com/srsapireddy/Images/blob/main/151.png?raw=true) </br>
PMOS </br>
* To check whether the drain of PMOS is connected to the drain of NMOS. (Press s 3 times)
![Image](https://github.com/srsapireddy/Images/blob/main/152.png?raw=true) </br>
* According to the CMOS definition, the source of the PMOS should be connected to VDD and the source to the ground. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/153.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/154.png?raw=true) </br>
* Refer to the GitHub link for more information on how to build a CMOS.
* For placement of any macro, we don’t need information on the logic. We only need to know the boundaries (LEF file).
* LEF also serves to protect the IP. LEF is also called as frame view.

* N-substrate diff: The N-SUBSTRATE DIFFUSION layer represents the substrate region where N-type (negative charge carriers) diffusion occurs. This diffusion process creates the N-well, commonly used in complementary MOS (CMOS) technology, to form the body of P-channel transistors.
* N-substrate contact: In CMOS technology, the N-well is a substrate region doped with N-type impurities to create a well of positive charge carriers. The N-SUBSTRATE CONTACT is a contact structure that establishes an electrical connection between the metal layer and the N-well region. It allows for the routing of signals or the application of voltage to the N-well.
* metal 1: In an NMOS (N-channel Metal-Oxide-Semiconductor) transistor, Metal 1 connections refer to the metal layer used for routing signals and providing interconnections in the integrated circuit layout. Metal 1 is typically the first metal layer above the active transistor area. In NMOS transistors, Metal 1 connections are used to establish electrical connections to various transistor components, such as the source, drain, and gate. These connections are typically made using metal traces or wires patterned on the integrated circuit layout.
* nwell: nwell refers to the N-type well region that is created within the P-type substrate.
![Image](https://github.com/srsapireddy/Images/blob/main/155.jpg?raw=true) </br>
* licon is the connectivity between the iocali layer and metal 1.  There need to be a contact between the nwell and licon. The crossed line is the n-substrate contact (first contact). This is the contact between nwell and locali.
* PMOS: Nwell -> above that locali layer -> above that metal 1
* Nwell and locali connection by nsubstrate contact. 
* For locali and Metal 1, we have licon.
* Since nmos has not well, it is directly we have direct psubstrate. We have first contact between psubstrate and locali. Then the contact between locali and metal 1.
 
* We need to ensure that the final design is DRC clean.
* To know the logical function of the cell, we need to extract the SPICE. Then, we do simulations using ngspice tool. 

### Extracting SPICE:
![Image](https://github.com/srsapireddy/Images/blob/main/156.png?raw=true) </br>
### Checking extracted file:
![Image](https://github.com/srsapireddy/Images/blob/main/157.png?raw=true) </br>
* We use this extracted file to create a spice file using ngspice tool:
![Image](https://github.com/srsapireddy/Images/blob/main/158.png?raw=true) </br>
### Check the spice file:
![Image](https://github.com/srsapireddy/Images/blob/main/159.png?raw=true) </br>

### Lab steps to create final SPICE deck using sky130 tech
* The alphabets represented in the SPICE file are nodes. For PMOS, the gate is connected to node A, the drain is connected to node Y, and the source is connected to VPWR.
* For NMOS, the drain is connected to Y, the gate is connected to A, and the source/ substrate is to VGND.
* VGND should be connected to VSS. The supply voltage should be connected to VSS. We create a new node 0 and connect VDD = 3.3V and pulse signal Vg.
![Image](https://github.com/srsapireddy/Images/blob/main/160.png?raw=true) </br>
* The minimum value of the layout window using single grid in layout:
![Image](https://github.com/srsapireddy/Images/blob/main/161.png?raw=true) </br>
* Set the scale concerning grid box size (scale=0.01u) in the SPICE file.
* We must also include the PMOS and NMOS lib files in the SPICE file.
![Image](https://github.com/srsapireddy/Images/blob/main/162.png?raw=true) </br>
* We must also include the definitions for the supply voltages and commands for transient analysis.
* We also need to change the model files in SPICE file for PMOS and NMOS.
![Image](https://github.com/srsapireddy/Images/blob/main/163.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/164.png?raw=true) </br>
* Then run the SPICE file using ngspice
![Image](https://github.com/srsapireddy/Images/blob/main/165.png?raw=true) </br> 
![Image](https://github.com/srsapireddy/Images/blob/main/166.png?raw=true) </br>
* To remove the spikes, we need to change the cload.
![Image](https://github.com/srsapireddy/Images/blob/main/167.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/168.png?raw=true) </br>
 
### Characterizing a cell means finding 4 parameters.
1.rise transition: the time taken for the output waveform to transit from 20% of the maximum value to 80% of the VDD. 
![Image](https://github.com/srsapireddy/Images/blob/main/169.png?raw=true) </br>
For 20%
![Image](https://github.com/srsapireddy/Images/blob/main/170.png?raw=true) </br>
For 80%
![Image](https://github.com/srsapireddy/Images/blob/main/171.png?raw=true) </br>
* The difference between x0 values gives the rise transition. Around 0.64ns
2. fall transition: the time taken for the output to fall from 80% to 20%.
3. fall cell delay (propagation delay): the difference between the period when the output has fallen to 50% and when the input raised to 50%.
![Image](https://github.com/srsapireddy/Images/blob/main/172.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/173.png?raw=true) </br>
* Around 0.061ns
4. rise cell delay (propagation delay): the time difference between the  50% of the output
We need to use the layout and create a LEF file and use this file in openlane and plug it in PICORV 32 core.

### MAGIC VLSI Layout Tool (efabless): http://opencircuitdesign.com/

### Downloading required files:
![Image](https://github.com/srsapireddy/Images/blob/main/174.png?raw=true) </br>
* .mag files are layout files in MAGIC format.
* .magicrc files are the startup file for the MAGIC. It tells where to find the technology files.

### Opening MAGIC tool:
![Image](https://github.com/srsapireddy/Images/blob/main/175.png?raw=true) </br>
* Open met3.mag file using MAGIC:
![Image](https://github.com/srsapireddy/Images/blob/main/176.png?raw=true) </br>
* Reference: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3
* Click: to redirect to the console window.
![Image](https://github.com/srsapireddy/Images/blob/main/177.png?raw=true) </br>
* By doing this we can see the rules which are violated.
![Image](https://github.com/srsapireddy/Images/blob/main/178.png?raw=true) </br>
* Open file poly.mag </br>
![Image](https://github.com/srsapireddy/Images/blob/main/179.png?raw=true) </br>
* Considering poly.9
* Poly resistor spacing to poly or spacing (no overlap) to diff/tap 0.480 µm
* Reference: https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#poly

![Image](https://github.com/srsapireddy/Images/blob/main/180.png?raw=true) </br>
* To check the layer type </br>
![Image](https://github.com/srsapireddy/Images/blob/main/181.png?raw=true) </br>
* Checking rule violation

![Image](https://github.com/srsapireddy/Images/blob/main/182.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/183.png?raw=true) </br>
* DRC section starts on 4072 line
![Image](https://github.com/srsapireddy/Images/blob/main/184.png?raw=true) </br>
* Search for poly rules </br>
![Image](https://github.com/srsapireddy/Images/blob/main/185.png?raw=true) </br>
* Adding spacing to add whats missing in the file
* Changing all the contact rules pertaining to poly
![Image](https://github.com/srsapireddy/Images/blob/main/186.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/187.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/188.png?raw=true) </br>

DAY 4
### Lab steps to convert grid info to track information
* Open inverter mag file </br>
![Image](https://github.com/srsapireddy/Images/blob/main/189.png?raw=true) </br>
* LEF file have the information about the input, power, and ground ports.
* We need to extract LEF file from the .mag file.
1.The input port and output port must lie on the vertical and horizontal tracks.
2.Width of the standard cell must be in odd multiples of the track pitch.
3.Height should be in the order of vertical pitch.
* What is a track?
  - Tracks are used during the routing stage. Routes can go over the tracks. Routes are the metal traces. PnR is an automated flow. We need to specify where our routes are to go. Tracks give that specification. Using these tracks, PnR can route the metal layers.
![Image](https://github.com/srsapireddy/Images/blob/main/190.png?raw=true) </br>
* Press g to get the grid view.
![Image](https://github.com/srsapireddy/Images/blob/main/191.png?raw=true) </br>
* Input A and output Y are on the horizontal and vertical tracks as shown in figure. This ensures that the route can reach that port for X and Y directions.
![Image](https://github.com/srsapireddy/Images/blob/main/192.png?raw=true) </br>

### Convert magic layout to std cell LEF
* The width of the standard cell should be in the odd multiples of the X-pitch.
* Port information is required only when we need to extract the LEF file. When we extract the LEF file, these ports are defined as pins of the macro.
* Converting labels to ports:
![Image](https://github.com/srsapireddy/Images/blob/main/193.png?raw=true) </br>
* How does the tool know A is input and Y is an output port?
* We use port class and port use attributes.
* Once these parameters are set, we can extract the LEF file from a cell/ macro.
* Before extracting the LEF, we need to give the cell a custom name.
![Image](https://github.com/srsapireddy/Images/blob/main/194.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/195.png?raw=true) </br>
 
* Creating a LEF file </br>
![Image](https://github.com/srsapireddy/Images/blob/main/196.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/197.png?raw=true) </br>

### Plugging this LEF file into PICORV32A – move the LEF file into src folder
![Image](https://github.com/srsapireddy/Images/blob/main/198.png?raw=true) </br>
* We need to have a library which has our cell definition for synthesis.
* Here the tool should map vsdinverter cell to the synthesis flow.
![Image](https://github.com/srsapireddy/Images/blob/main/199.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/200.png?raw=true) </br>

* We need to modify our config.tcl file next.
![Image](https://github.com/srsapireddy/Images/blob/main/201.png?raw=true) </br>
* Run openlane flow
![Image](https://github.com/srsapireddy/Images/blob/main/202.png?raw=true) </br>

* Then `run_synthesis`
![Image](https://github.com/srsapireddy/Images/blob/main/203.png?raw=true) </br>
* We can see that 1554 instances of our custom cell is added to the picorv32a design.

### Introduction to delay tables
![Image](https://github.com/srsapireddy/Images/blob/main/204.png?raw=true) </br>
* There are advantages of these AND gates in clock tree to reduce the power dissipation.
![Image](https://github.com/srsapireddy/Images/blob/main/205.png?raw=true) </br>
* Here we have spitted the load of 4 Flops using 2 buffers. And the load of two buffers is given to the first buffer. We need to swap the buffers to gates, as shown. This technique is called clock gating. 
We have varying input transitions at the input of the buffer and varying output load at the output of any buffer. So, we have a variety of delays. To solve this, we have delay tables.
![Image](https://github.com/srsapireddy/Images/blob/main/206.png?raw=true) </br>

![Image](https://github.com/srsapireddy/Images/blob/main/207.png?raw=true) </br>
* There are advantages of these AND gates in clock tree to reduce the power dissipation.
![Image](https://github.com/srsapireddy/Images/blob/main/208.png?raw=true) </br>
* Here we have spitted the load of 4 Flops using 2 buffers. And the load of two buffers is given to the first buffer. We need to swap the buffers to gates, as shown. This technique is called clock gating. 
* We have varying input transitions at the input of the buffer and varying output load at the output of any buffer. So, we have a variety of delays. To solve this, we have delay tables.
![Image](https://github.com/srsapireddy/Images/blob/main/209.png?raw=true) </br>
* Here x = W/L of PMOS and NMOS. If we increase the PMOS size and NMOS size, we are reducing the resistance. By this, we are varying the RC constant.  
![Image](https://github.com/srsapireddy/Images/blob/main/210.png?raw=true) </br>

### Fixing Slack
SYNTH_BUFFERING: Find any high fanout nets. We want high fanout nets to be buffers.
SYNTH_SIZING: Upsizing or downsizing a buffer based on delay strategy.
SYNTH_DRIVING_CELL: Cell that drives the input port. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/211.png?raw=true)  </br>
Settings to reduce tns and wns.
Run synthesis, floorplan and placement steps.  </br>
![Image](https://github.com/srsapireddy/Images/blob/main/212.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/213.png?raw=true) </br>
 
Check if vsd inverter is added after floorplan stage.
Check layout after placement stage
![Image](https://github.com/srsapireddy/Images/blob/main/214.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/215.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/216.png?raw=true)  </br>
We can see that vsd inverter is inserted after placement stage.
Abatement between the cells takes place to share power and ground between the cells.
![Image](https://github.com/srsapireddy/Images/blob/main/217.png?raw=true) </br>

### Static Timing Analysis (With Ideal Clocks)
* In ideal clock network clock tree is not yet built.
* Identifying combinational path:
* Pre-layout STA will not yet include effects of clock buffers and net-delay due to RC parasitics (wire delay will be derived from PDK library wire model).
![Image](https://github.com/srsapireddy/Images/blob/main/218.png?raw=true) </br>
* Setup timing analysis equation is:

Θ < T - S - SU
Θ = Combinational delay which includes clk to Q delay of launch flop and internal propagation delay of all gates between launch and capture flop
T = Time period, also called the required time
S = Setup time. As demonstrated below, signal must settle on the middle (input of Mux 2) before clock transits to 1 so the delay due to Mux 1 must be considered, this delay is the setup time.
![Image](https://github.com/srsapireddy/Images/blob/main/219.png?raw=true) </br>
SU = Setup uncertainty due to jitter which is temporary variation of clock period. This is due to non-idealities of PLL/clock source.

![Image](https://github.com/srsapireddy/Images/blob/main/220.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/221.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/222.png?raw=true) </br>

### Optimize synthesis to reduce setup violations
* Hold analysis have significance after CTS.
* The delay of any cell is a function of input slew (input transition) and output load. More the values, more the delay.
* Optimizing the fanout value:
* Set parameter for fanout:
![Image](https://github.com/srsapireddy/Images/blob/main/223.png?raw=true) </br>
* Then run synthesis, floorplan, and placement to check slack.
* Commands
  - report_net -connections _02682_
  - replace_cell _41882_ sky130_fd_sc_hd__buf_4
  - report_checks -fields {cap slew nets} -digits 4
  - report_checks -from _18671_ -to _18739_ -fields {cap slew nets} -digits 4
  - report_wns
  - report_tns
  - report_worst_slack -max
  - write_verilog designs/picorv32a/runs/RUN_2022.09.14_05.18.35/results/synthesis/picorv32.v

### Clock Tree Synthesis
![Image](https://github.com/srsapireddy/Images/blob/main/224.png?raw=true) </br>
* H-Tree takes a particular clock route and calculates the distance from clk to all the endpoints. Then come to the midpoint of the tree and build a tree from that point. Using this strategy, the clk reaches to flops almost simultaneously. The skew value will be close to zero.
* There will be a huge number of capacitances in clk paths. Due to this, there will be a huge transition problem. The solution to this is to add repeaters.
![Image](https://github.com/srsapireddy/Images/blob/main/224.png?raw=true) </br>
* The repeaters we use for clock and data paths are different. Here the repeaters have equal rise and fall times.
![Image](https://github.com/srsapireddy/Images/blob/main/225.png?raw=true) </br>

###Timing Analysis with Real Clock
* Clock Net Shielding:  Clock nets are considered critical nets in the design. By shielding, we protect the clock nets from the outside world. 
* Glitch: Any activity in one wire will impact the net without shielding because of the coupling capacitance.
![Image](https://github.com/srsapireddy/Images/blob/main/226.png?raw=true) </br>
* Shielding protect the design from glitches. Shielding will be connected with VDD or GND so that there are no glitches and crosstalk.
* There are three parameters that we need to consider when building a clock tree:

* Clock Skew = In order to have minimum skew between clock endpoints, clock tree is used. This results in equal wirelength (thus equal latency/delay) for every path of the clock.
* Clock Slew = Due to wire resistance and capacitance of the clock nets, there will be slew in signal at the clock endpoint where signal is not the same with the original input clock signal anymore. This can be solved by clock buffers. Clock buffer differs in regular cell buffers since clock buffers has equal rise and fall time.
* Crosstalk = Clock shielding prevents crosstalk to nearby nets by breaking the coupling capacitance between the victim (clock net) and aggressor (nets near the clock net), the shield might be connected to VDD or ground since those will not switch. Shielding can also be done on critical data nets.
![Image](https://github.com/srsapireddy/Images/blob/main/227.png?raw=true) </br>
 
### Run CTS using Triton CTS
Overwriting Verilog file </br>
![Image](https://github.com/srsapireddy/Images/blob/main/228.png?raw=true) </br>
We always have a tradeoff between timing, power, and area. 
Then all steps from synthesis to placement.
Then run `run_cts`
For each stage, we need to check the quality of the results. Lowering the value of qor degraded the results. Higher values produce smaller runtimes but worst qor.
In the CTS stage, clock buffers get added. This will modify our netlist. After running CTS, we get a new .v file in the synthesis folder.
![Image](https://github.com/srsapireddy/Images/blob/main/229.png?raw=true) </br>
CTS variables.

Verify CTS runs
CTS procs: where the command gets to run from.
![Image](https://github.com/srsapireddy/Images/blob/main/230.png?raw=true) </br>
or_cts.tcl file contents. Where the CTS commends gets executed
![Image](https://github.com/srsapireddy/Images/blob/main/231.png?raw=true) </br>
Checking procs in openlane tool:
![Image](https://github.com/srsapireddy/Images/blob/main/232.png?raw=true) </br>

### Setup Timing Analysis (Real Clocks)
* Setup and hold analysis with the real clock will now include clock buffer delays:

* In setup analysis, the point is that the data must arrive first before the clock rising edge to latch that data properly. Setup violation happens when the path is slow. This is affected by parameters such as combinational delay, clock buffer delay, period, setup time, and setup uncertainty (jitter).

* Hold analysis is the delay that the MUX2 model inside the flip-flop needs to move the data to the outside. This is when the launch flop must hold the data before it reaches the capture flop. Hold analysis is done on the same rising clock edge for launch and capture flop, unlike setup analysis, which spans between two rising clock edges. Hold violation happens when the path is too fast. This is affected by parameters such as combinational delay, clock buffer delays, and hold time. (Time period and setup uncertainty does not matter since launch and capture flops will receive the same rising clock edges for hold analysis). FF2 holds the data until it sends the data outside. It should not receive any data at this time. 
![Image](https://github.com/srsapireddy/Images/blob/main/233.png?raw=true) </br>
* The goal is to have a positive slack on setups and hold analysis.
![Image](https://github.com/srsapireddy/Images/blob/main/234.png?raw=true) </br>
 
* Skew = |delta 1 – delta 2|

### Analyze timing with a real clock using OpenSTA
* Run `openroad` to do timing analysis. OpenSTA is integrated inside the openroad.
* Here we need to read lef and def files to create db.
![Image](https://github.com/srsapireddy/Images/blob/main/235.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/236.png?raw=true) </br>
 
* Create a db </br>
![Image](https://github.com/srsapireddy/Images/blob/main/237.png?raw=true) </br>
* Check if db is created  </br>
![Image](https://github.com/srsapireddy/Images/blob/main/238.png?raw=true) </br>
* Read db , Verilog, and library files
![Image](https://github.com/srsapireddy/Images/blob/main/239.png?raw=true) </br>
* read_liberty $::env(LIB_SYNTH_COMPLETE) // we are not using LIB_MAX & LIB_MIN because we ran our cts for one corner that is the typical corner. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/240.png?raw=true) </br>
* Read sdc file </br>
![Image](https://github.com/srsapireddy/Images/blob/main/241.png?raw=true) </br>
### SDC FILE UPDATED
![Image](https://github.com/srsapireddy/Images/blob/main/242.png?raw=true) </br>
* PATH: picorv32a/src/my_base.sdc
* To calculate actual cell delay in clock path:
![Image](https://github.com/srsapireddy/Images/blob/main/243.png?raw=true) </br>
* Check Slack: </br>
* Hold Slack: </br>
![Image](https://github.com/srsapireddy/Images/blob/main/244.png?raw=true) </br>
* Setup Slack: </br>
![Image](https://github.com/srsapireddy/Images/blob/main/245.png?raw=true) </br>
* We have built the clock tree for typical but analyzing for min_max corners.
* Exit from the openroad
* Here we need to include the typical library for typical analysis.
![Image](https://github.com/srsapireddy/Images/blob/main/246.png?raw=true) </br>
* Reading typical lib (screenshot with errors)
![Image](https://github.com/srsapireddy/Images/blob/main/247.png?raw=true) </br>
* Slack for typical corner: </br>
![Image](https://github.com/srsapireddy/Images/blob/main/248.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/249.png?raw=true) </br>
* Both are met.
* When openlane is building the CTS, it will try to meet the skew value from left to right clkbuf_1 to clkbuf_8. We want the skew values to be 10% of the clock period.
* If we want to replace any element in tcl. `lreplace ::env(CTS_CLKBUFFER_LIST) 0 0`. This command removes the clock buffer 1. lreplace does not modify the list.
* To modify the list, we have to `set ::env(CTS_CLKBUFFER_LIST) [lreplace ::env(CTS_CLKBUFFER_LIST) 0 0]`
* The check `echo ::env(CTS_CLKBUFFER_LIST) `
* Then again, run `run_cts`
* To kill the process: type `top` then ‘kill -9 process_id’
* To check current def run `echo $::env(CURRENT_DEF)`
* To set current def run ` echo $::env(CURRENT_DEF) path_to_current_def_file`
* Then again, run `run_cts`
* Then open openroad in openlane, create db, and follow the steps again with the updated db. But the area increased for the design.
* Check skew: `report_clock_skew -hold/setup` 
* To insert back buffer:  `set ::env(CTS_CLKBUFFER_LIST) [linsert ::env(CTS_CLKBUFFER_LIST) 0 sky_130_fd_sc_hd___clkbuf_1]`

DAY 5
### Routing
* Routing means finding the best possible way to connect two components/cells.
* Most of the routing tools are based on Lee's Algorithm. For more info on the algorithm, please refer here.
* Routes with a minimum number of bends are preferred by the tool.
* Two stages of Routing: Global and Detailed Routing. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/250.png?raw=true) </br>

### DRC
* There should be a minimum distance between wires when we route wires.
* Rule: The optical wavelength of the wire is so small that the minimum width of the wire should be a minimum value. Wire width should be at least the minimum value.  </br>
![Image](https://github.com/srsapireddy/Images/blob/main/251.png?raw=true) </br>
* Rule: The minimum pitch between two wires. The center-to-center distance between two wires. What we see on the mask, we see the patterns on the silicon. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/252.png?raw=true) </br>
* Rule: Minimum spacing between two wires at least this much. It can be more than this but it cant be less than this. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/253.png?raw=true) </br>
* Rule: </br>
![Image](https://github.com/srsapireddy/Images/blob/main/254.png?raw=true)  </br>
* This might lead to functionality failure.
* Solution: Consider the two nets are of metal 2. One more layer will be introduced on top of it, which is metal 3. Upper metals are wider than lower metals. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/255.png?raw=true) </br>
* Via width should be some minimum value. To connect two different metal layers. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/256.png?raw=true) </br>
* Minimum spacing between two vias rule.
* Then we need to do parasitic extraction to get the resistance and capacitance values of the wires. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/257.png?raw=true) </br>

### Routing Stage and TritonRoute:
* OpenLane routing stage consists of two stages:

* Global Routing - Form routing guides that can route all the nets. The tool used is FastRoute
* Detailed Routing - Uses the global routing guide to connect the pins with the least amount of wire and bends. The tool used is TritonRoute.

### Triton Route
![Image](https://github.com/srsapireddy/Images/blob/main/258.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/259.png?raw=true) </br>
![Image](https://github.com/srsapireddy/Images/blob/main/260.png?raw=true) </br>

* Performs detailed routing and honors the pre-processed route guides (made by global route) and uses MILP-based (Mixed Integer Linear Programming algorithm) panel routing scheme(uses panel as the grid guide for routing) with intra-layer parallel routing (routing happens simultaneously in a single layer) and inter-layer sequential layer (routing starts from bottom metal layer to top metal layer sequentially and not simultaneously).
* The honors preferred the direction of a layer. Metal layer direction alternates (metal layer direction is specified in the LEF file, e.g., met1 Horizontal, met2 Vertical, etc.) to reduce overlapping wires between layers and reduce potential capacitance, which can degrade the signal. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/261.png?raw=true) </br>

### Power Distribution Networking and Routing
* The power and ground rails have a pitch of 2.72um because the customized inverter cell has a height of 2.72, or else the power and ground rails will not be able to power up the cell. Looking at the LEF file `runs/[date]/tmp/merged.lef`, all the cells have a height of 2.72um with a difference in width.
![Image](https://github.com/srsapireddy/Images/blob/main/262.png?raw=true)
* The power and ground flow from power/ground pads to power/ground ring to power/ground straps to power/ground rails.
* To run from the previous day: `prep -design picorv32a -tag date`
![Image](https://github.com/srsapireddy/Images/blob/main/263.png?raw=true)
* To run Power Distribution Network: run `gen_pdn`
* Then run `run_routing`
* This will do both global and detailed routing, taking multiple optimization iterations until the DRC violation is reduced to zero. </br>
![Image](https://github.com/srsapireddy/Images/blob/main/264.png?raw=true)

### Checking Layout in MAGIC:
* power rail, ground rail, input net route and output net route for a cell: 
![Image](https://github.com/srsapireddy/Images/blob/main/265.png?raw=true)
* Final Layout: magic -T /home/vsduser/Desktop/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32.def </br>
![Image](https://github.com/srsapireddy/Images/blob/main/266.png?raw=true)
 
### SPEF Extraction and GDSII Streaming:
* For post-routing STA: `run_parasitics_sta`
* SPEF: Standard Parasitic Extraction Format
* Multi-corner STA will be done with the extracted SPEF.
* SPEF extraction and multi-corner STA will be done on all three corners (min, max, typical).
* The extracted SPEF can be located under `runs/[date]/results/routing` and the STA log files under `runs/[date]/logs/signoff`.

* Timing ECO should be followed to reduce slack to desired level.
![Image](https://github.com/srsapireddy/Images/blob/main/267.png?raw=true)
![Image](https://github.com/srsapireddy/Images/blob/main/268.png?raw=true)

* Finally, run: `run_magic`. This will create the GDS file under: `runs/[date]/results/signoff/picorv32.gds`
![Image](https://github.com/srsapireddy/Images/blob/main/269.png?raw=true) 

### Acknowledgements
Kunal Ghosh - Co-founder of VSD </br>
Nickson Jose - Workshop Instructor





 














 
 










 
























 

 

 







 









 












 






























 









