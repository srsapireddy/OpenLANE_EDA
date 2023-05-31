# OpenLANE VSD
* This repository will reflect the work done in the Advanced Physical Design Flow workshop, offered by the folks at VSD. It's a 5 day workshop that aims to educate us on the intricacies of Open Source EDA tools and PDKs as used by industry leaders. Furthermore, it uses Openlane flow to help understand the flow of work when it comes to VLSI designs, starting from the RTL level to the GSDII stage, performing the synthesis, placement, floorplanning, routing and sta.

## Taking the First Steps
The beauty of open-source is that it's freely avaialable to everyone. OpenLANE is open-sourced as well, bringing us one step closer to using industry grade vlsi tools. To get OpenLANE up and running locally on your systems, it's essential to keep in mind these 2 

### Prerequisites:
1. Ubuntu OS
2. 40GB+ Disk Space. (If you're comfortable with Virtual Box, you can make use of Partition GUIs to allocate the necessary memory)
Adding on, I would suggest anyone interested to go through these 2 courses, which were extremely helpful in guiding me through the entire installation and testing process. Both courses are freely avaialable on Udemy.

## Day 1
### 1. How to Talk to Computers?
![package](https://github.com/srsapireddy/Images/blob/main/1.PNG?raw=true)
* The chip will be sitting at the center of the package. Chip is connected to the package, as shown in the figure. 
  * PADS – To send the signals inside the chip inside or outside the chip. </br>
  * CORE – Where all the digital logic is placed.
  * DIE – The size of the entire chip. Die will be manufactured on the silicon wafer.
![core](https://github.com/srsapireddy/Images/blob/main/2.PNG?raw=true)
* Other details
  * Foundry – Place where the chips get manufactured.
  * IP (Intelligent Properties) – An Intellectual Property (IP) core in Semiconductors is a reusable unit of logic or functionality or a cell or a layout design that is normally developed with the idea of licencing to multiple vendor for using as building blocks in different chip designs. 
  * Macros – Digital logic blocks
  * RISC-V Instruction Set Architecture 
  * ISA – The way we interact with computers.
![macrovsip](https://github.com/srsapireddy/Images/blob/main/3.PNG?raw=true)
* Suppose we want a C program to run on a particular layout. The C program is compiled into an assembly language program (RISC-V assembly language program). This assembly language program is converted into a machine language program (binary language program).  The bits here will be executed in a layout, and we will get the required output.
![riscvisa](https://github.com/srsapireddy/Images/blob/main/4.PNG?raw=true)
* The interface between RISC-V architecture and the layout is hardware description language. We implement RISC-V specifications in RTL. 

### From software applications to hardware
* The application software enters a block called system software. The system software converts the application software into binary language. </br>
![software](https://github.com/srsapireddy/Images/blob/main/5.PNG?raw=true)
### The flow of System Software:
 
1. OPERATING SYSTEM 
* OSHandles IO operations and allocates memory and low-level system functions. Converts the app into an assembly language program and finally into the binary language program so that the hardware understands it. 
The output of the operating system is the small functions in C, C++, Java, and VB.   

2. COMPILER 
* These functions are taken by the respective compiler and convert them into instructions. The syntax of the instructions will depend on the type of hardware used (If the hardware is ARM, then the instructions will be in the ARM format) - .exe file. 
* The instructions here will act as the abstract ISA between the hardware and the C programs. 
* ![stopwatch](https://github.com/srsapireddy/Images/blob/main/7.PNG?raw=true)
* We need an RTL to implement instruction specifications. The RTL is converted into a synthesized netlist (high-level specification into synthesized netlist). This will be in the form of gates. Above netlist generation, we follow physical design implementation.
![stopwatch](https://github.com/srsapireddy/Images/blob/main/8.PNG?raw=true)

3. ASSEMBLER
* The job of the assembler is to take the instructions and convert them into binary numbers (machine language program). Based on machine language programs, the hardware executes the functions and accordingly generates the output. </br>
Example: Stopwatch app
![stopwatch](https://github.com/srsapireddy/Images/blob/main/6.PNG?raw=true)
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
![foss](https://github.com/srsapireddy/Images/blob/main/9.PNG?raw=true)
 
* Google releaseD open-source PDK for ASIC implementation using open-source or close source tools.
 ![distribution](https://github.com/srsapireddy/Images/blob/main/10.PNG?raw=true)
* The fabrication process of SkyWater 130 nm is cheaper than advanced nodes. It covers over 6% of IC technology nodes used in the market. </br>
![130nm](https://github.com/srsapireddy/Images/blob/main/11.PNG?raw=true)
 
### ASIC Flow objective: RTL to GDS II - Also called Automated PnR and/or Physical Implementation.
![tools](https://github.com/srsapireddy/Images/blob/main/12.PNG?raw=true)
#### Simplified RTL to GDSII Flow
![flow](https://github.com/srsapireddy/Images/blob/main/13.PNG?raw=true) 
#### 1. Synthesis
![Synthesis](https://github.com/srsapireddy/Images/blob/main/14.PNG?raw=true) 
* Converts RTL to a circuit out of components from the standard cell library (SCL). The resulting circuit is referred to as a gate-level netlist. Gate level netlist is functional equivalent to RTL. </br>
* The library building blocks, or the cells, have regular layouts. Typically, the cell layout is enclosed by a fixed-height rectangle. The cell width is variable but discrete. It is an integer of multiple units called site width.
* Standard cells have a regular layout – Each has different views/ models.
* Liberty View – Has an electrical model for the cells, such as delay and power models. 
* HDL and SPICE behavior views for the cells. Layout view for the cells. 
* GDS View, LEF view- abstract view.

#### 2. Floor planning and power planning
![fp](https://github.com/srsapireddy/Images/blob/main/15.PNG?raw=true) 
* The objective here is to plan the silicon area and create robust power distribution to the circuits. 
* Chip floor-planning: Partition the chip die between different system building blocks and place the I/P pads.
* Macro Floor Planning: Dimensions, pin locations, rows or routing tracks are definition.
![pp](https://github.com/srsapireddy/Images/blob/main/16.PNG?raw=true) 
* In power planning the power network is constructed.
* A chip is powered by multiple VDD and GND pins. The power pins are connected to all the components through rings and horizontal/ vertical power straps. Such parallel structures are meant to reduce the resistance hence the IR Drop, and to address the electromigration problem. Usually, the power distribution network uses upper metal layers as they are thicker than the lower metal layers. Hence have less resistance. 

#### 3. Placement: 
![Placement](https://github.com/srsapireddy/Images/blob/main/17.PNG?raw=true) 
* Place the cells on the floorplan rows aligned with the site rows. Connected cells are placed very close to each other to reduce the interconnect delay and allow successful routing afterward. 
* Done in two steps: 
  * Global - Tries to find the optimal placement of all the cells. Such positions are not necessarily legal. So cells may overlap or go off the sites. 
  * Detailed - The positions obtained from the global placement are minimally altered to be legal. 
![Placement_2](https://github.com/srsapireddy/Images/blob/main/18.PNG?raw=true) 
 
#### 4. Clock Tree Distribution:
![CTS](https://github.com/srsapireddy/Images/blob/main/19.PNG?raw=true) 
* Deliver the clock to all sequential elements (eg, FF) with minimum skew.
* Clock skew – The arrival of the clock to the different components at different times.

#### 5. Routing: 
![Routing](https://github.com/srsapireddy/Images/blob/main/20.PNG?raw=true)

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

### 2.3. Introduction to OpenLANE and Strive chipsets
* OpenLANE started as an open-source flow for a true open-source tape-out experiment.
* striVe is a family of open sourcing everything, including SoCs. (Open PDK, Open EDA, Open RTL)
![striVe](https://github.com/srsapireddy/Images/blob/main/21.PNG?raw=true)

* OpenLANE main goal: To produce a clean GDSII with no human intervention. Tuned for SkyWater 130nm Open PDK. Also, support XFAB180 and GF130G. It can harden Macros and Chips to generate the final layout.
* Two modes of operation: 
  * autonomous – push button flow 
  * Interactive – run commends one by one to check intermediate steps.

### 4. OpenLANE ASIC DESIGN FLOW
![ASIC](https://github.com/srsapireddy/Images/blob/main/22.png?raw=true) 
### RTL Design
  * The flow starts with the design RTL and generating the final layout in GDSII format. OpenLANE is based on several open-source projects. </br>
![source](https://github.com/srsapireddy/Images/blob/main/23.PNG?raw=true) 
  * The flow starts with the RTL synthesis. The RTL is fed to yosys with the design constraints. Yosys translates the RTL to logic circuits. These circuits can be optimized and mapped into a standard cell library using the ABC tool. ABC should be guided during the optimization. This guidance will come in the form of the ABC script. OpenLANE comes with several open-source scripts. We refer to them as synthesis strategies. We have strategies to target the least area and for the best timing. Different designs can use different strategies to achieve the best objective. For this, we have synthesis exploration utility. This is used to generate a report that shows the design delay concerning the area effected by synthesis strategy. </br>
![strategies](https://github.com/srsapireddy/Images/blob/main/24.PNG?raw=true) 

### Design Exploration Utility
  * OpenLANE also has the design exploration utility. This can be used to sweep the design configurations. And generates a report, as shown below. </br>
![Exploration](https://github.com/srsapireddy/Images/blob/main/25.PNG?raw=true) 
  * This report shows the different design metrics. We have more than 35 of them in a report. It also shows the number of violations after generating the final layout. It is recommended to explore the design first and then use the best configurations for a particular design to result in a clean layout.
  * Also, design exploration can be used for regression testing (CI). So, we can run the design on several exploration configurations to get the best-optimized configurations. Currently, we have 70 designs. This utility will generate a report as shown below. </br>
![testing](https://github.com/srsapireddy/Images/blob/main/26.PNG?raw=true) 
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
![ANTENNA](https://github.com/srsapireddy/Images/blob/main/27.PNG?raw=true)  
 * Usually, this is the job of the router.
   * Solutions:
     1. Bridging – Attaches a higher layer intermediary. Requires the router awareness.
     2.  Antenna diode – Add antenna diode cell to leak away charges. Antenna diodes are provided by the SCL. 
![diode](https://github.com/srsapireddy/Images/blob/main/28.PNG?raw=true)  
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
  ![sky130A](https://github.com/srsapireddy/Images/blob/main/29.png?raw=true)
  * libs.ref: contains all the process-specific files (timing, lef and cell lef)
  * libs.tech: specific to the tool </br>
  ![libs](https://github.com/srsapireddy/Images/blob/main/30.png?raw=true)
* We are working with sky130_fd_sc_hd </br>
Here, sky130: process name, 130 nm
  - fd: for skywater foundry
  - sc: for standard cell library files
  - hd: variant of the PDK (hd: high density

![techlef](https://github.com/srsapireddy/Images/blob/main/31.png?raw=true)
* techlef contains all the layer information.
* mag: all the MAGIC-related files
* lib: all the timing files containing all the process corners.

![corners](https://github.com/srsapireddy/Images/blob/main/32.png?raw=true)
* tt: for typical, ss: for slow slow, ff: for fast fast and PVT corners
![lef_files](https://github.com/srsapireddy/Images/blob/main/32.png?raw=true)
* lef files

* Tool environment directory: </br>
![tool_directory](https://github.com/srsapireddy/Images/blob/main/35.png?raw=true)

### Designing Preparation Step
#### Running OpenLANE EDA </br>
![Running](https://github.com/srsapireddy/Images/blob/main/36.png?raw=true) 
   - Without the interactive switch, the tool will run the complete flow.

* Importing the required packages </br>
![packages](https://github.com/srsapireddy/Images/blob/main/37.png?raw=true) 
  - All the designs run by OpenLANE will be extracted from the design folder.
 
* Here we consider the picorv32a design </br>
![picorv32a](https://github.com/srsapireddy/Images/blob/main/38.png?raw=true) 
  - src: where the Verilog file for our RTL will be present. As well as the SDC information.
  - config.tcl: by default, characteristics of the design information are mentioned. This will override the default OpenLANE settings. 

![design](https://github.com/srsapireddy/Images/blob/main/39.png?raw=true) 
* Setting up the file system for the design: </br>
![Setting](https://github.com/srsapireddy/Images/blob/main/41.PNG?raw=true) 
  - Here the tech.lef (layer level information) and cell level lef merged into one.

#### Reviewing files after design prep and run synthesis
* Here the runs directory has been created after running command `run-synthesis`.
  * tmp folder: where the temporary files are stored, and the rest of the folders will be empty. </br>
![runs](https://github.com/srsapireddy/Images/blob/main/42.png?raw=true)  
* config.tcl: show all the default parameters taken by the run.

* For synthesis: this will run the yosys and abc 
![synthesis](https://github.com/srsapireddy/Images/blob/main/43.png?raw=true)  
* OpenLANE project Git link: https://github.com/efabless/openlane

* Calculating the D-Flip Flop count for the design: 1613/14876= 10.8%

* If we check the synthesis folder for results after running run_synthesis we can see that there is a new file generated named picorv32a.synthesios.v </br>
![results](https://github.com/srsapireddy/Images/blob/main/44.png?raw=true)  
* To check the synthesis statistic report  </br>
![report](https://github.com/srsapireddy/Images/blob/main/45.png?raw=true)  

## DAY 2
### Utilization factor and aspect ratio
* Define the width and height of the core and die. This is the first step in the physical design flow.
![report](https://github.com/srsapireddy/Images/blob/main/46.PNG?raw=true)  
* Netlist defines the connectivity between all the components. 
* In defining the dimensions of the chip, it mostly depends on the dimensions of the logic gates. 

![Utilization](https://github.com/srsapireddy/Images/blob/main/47.PNG?raw=true)  
* To get the dimensions of the core and die, we are interested in the dimensions of the standard cells, not considering the wires in between them.
* Standard cells are given rough dimensions as shown below.

![calculate](https://github.com/srsapireddy/Images/blob/main/48.PNG?raw=true)  
* Let’s calculate the area occupied by the below netlist on a silicon wafer by bringing the cells together.
 
* What is the core and die section of a chip?
![core](https://github.com/srsapireddy/Images/blob/main/49.PNG?raw=true)  
  * Core: where we place the fundamental logic.
  * Die piece of the area where our circuit is built so that our circuit will not exceed this area. We imprint this die multiple times on a silicon wafer to increase its throughput. 
![die](https://github.com/srsapireddy/Images/blob/main/50.PNG?raw=true)  
 
* As we can see that the netlist of 4 sq units occupies a complete area of the core. This means we have utilized the core 100%. </br>
![units](https://github.com/srsapireddy/Images/blob/main/51.PNG?raw=true)  

* Utilization Factor: Area occupied by netlist/ Total area of the core = 4 x 1sq.unit/ 2 unit x 2 unit = 1
  * In practice, we go for a 50 to 60% utilization.
* Aspect Ratio = Height/ width = 2 unit/ 2 unit = 1
* When the aspect ratio is 1, the chip is squared in shape.
Ex:
![Factor](https://github.com/srsapireddy/Images/blob/main/52.PNG?raw=true)  
### Concept of pre-placed cells


 






























 









