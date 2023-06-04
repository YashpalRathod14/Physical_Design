# Physical-Design

## Content
1. [Overview](#1-overview)    
2. [How to talk to computers ?](#2-how-to-talk-to-computers-)   
  -[Introduction to basic terminologies.](#i-introduction-to-basic-terminologies)   
  -[Introduction to RISC V](#ii-introduction-to-risc-v)   
  -[From Software applications to Hardware](#iii-from-software-applications-to-hardware)   
3. [SoC Design and OpenLane](#3-soc-design-and-openlane)   
  -[Introduction to components of open source digital ASIC design](#i-introduction-to-components-of-open-source-digital-asic-design)   
  -[RTL2GDS flow](#ii-rtl2gds-flow)   
  -[Introduction to OpenLane](#iii-introduction-to-openlane)   
  -[OpenLane detailed ASIC design flow](#iv-openlane-detailed-asic-design-flow)   
4. [OpenSource EDA Tools.](#4-opensource-eda-tools)   
  -[OpenLane Directory structure](#i-openlane-directory-structure)   
  -[Design Preparation steps](#ii-design-preparation-steps)   
  -[Review files after design preparation and run synthesis](#iii-review-files-after-design-preparation-and-run-synthesis)   
  -[Steps to characterize sysnthesis results](#iv-steps-to-characterize-sysnthesis-results)    
5. [Chip Floorplanning and Considerations](#5-chip-floorplanning-and-considerations)    
  -[Utilization Factor and Aspect Ratio](#i-utilization-factor-and-aspect-ratio)    
  -[Concept of pre_placed cells](#ii-concept-of-pre-placed-cells)   
  -[De-coupling Capacitors](#iii-de-coupling-capacitors)    
  -[Power Planning](#iv-power-planning)   
  -[Pin-PlacementSteps to run floorplan using openLane](#v-pin-placement-)
  -[Steps to run floorplan using openLane](#vi-steps-to-run-floorplan-using-openlane)    
  -[Review floorplan adn steps to view floorplan](#vii-review-floorplan-adn-steps-to-view-floorplan)     
 6. [Library Binding and Placement](#6-library-binding-and-placement)   
  -[Netlist binding and initial place design](#i-netlist-binding-and-initial-place-design)   
  -[Optimize placement using estimated wirelength and capacitance](#ii-optimize-placement-using-estimated-wirelength-and-capacitance)    
  -[Final placement optimization](#iii-final-placement-optimization)    
  -[Need for library characterization and Congestion aware placement using Replace](#iv-need-for-library-characterization-and-congestion-aware-placement-using-replace)    
7.[Cell design and characterization flow](#7-cell-design-and-characterization-flow)    
  -[inputs for cell design flow](#i-inputs-for-cell-design-flow)    
  -[Layout design steps.](#ii-layout-design-steps)    
  -[Typical Characterization Flow](#iii-typical-characterization-flow)   
8. [General timimg characerization parameters](#8-general-timimg-characerization-parameters)    
9. [Labs for CMOS inverter using ngspice](#9-labs-for-cmos-inverter-using-ngspice)     
  -[IO placer revision](#i-io-placer-revision)    
  -[SPICE deck creation for CMOS inverter](#ii-spice-deck-creation-for-cmos-inverter)    
10. [Inception of layout 16-mask CMOS fabrication process](#10-inception-of-layout-16-mask-cmos-fabrication-process)     
11. [Labs for layout & SKY130 tech file labs](#11labs-for-layout--sky130-tech-file-labs-)    
  -[steps to git clone vsdstdcell design](#i-steps-to-git-clone-vsdstdcell-design)    
  -[create final SPICE deck using sky130 tech](#ii-create-final-spice-deck-using-sky130-tech)     

# **1. Overview**

This repository is about the open-source tool OpenLane which includes OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, CU-GR, Klayout and other scripts for design exploration and observation. Here, we will implement full ASIC flow from RTS to GDS on PicoRV32a which is based on RISC V. 

# 2. How to talk to computers ?

## i. Introduction to basic terminologies.

 **Pads:**  Pads are components on a chip that serve as interfaces between the internal circuitry and the external world. They provide a means to send signals into and out of the chip. Pads are typically located along the periphery of the chip and are used for various purposes such as power supply connections, input/output (I/O) connections, and testability features. They allow the chip to communicate with other devices or systems.   
 
 **Core:** a core refers to the central processing unit (CPU) or the part of the chip where the digital logic is located. The core consists of various functional units, such as multiplexers (mux), gates, arithmetic logic units (ALUs), and registers, which are responsible for performing computations and executing instructions. The core is the primary component responsible for carrying out the main tasks and operations of the chip.   
 
 **Die:** The die refers to the individual piece or unit of a semiconductor chip. It is essentially a small, square or rectangular slice of silicon wafer on which the integrated circuits (ICs) and other components are fabricated. The die contains the active components of the chip, including transistors, resistors, capacitors, and interconnects, which are interconnected to form the desired circuitry. The size of the die determines the overall physical dimensions of the chip. Multiple dies can be manufactured on a single silicon wafer during the semiconductor fabrication process.   
  
## ii. Introduction to RISC V
RISC-V is an open-source instruction set architecture (ISA) that defines the set of instructions a computer processor can execute. RISC-V stands for "Reduced Instruction Set Computer Five," where "Five" represents the fifth major version of the architecture.

The RISC-V ISA is designed to be simple and modular, making it easier to implement in hardware and software. It follows the principles of a reduced instruction set computer (RISC), which emphasizes a small number of simple instructions that can be executed efficiently. RISC-V supports both 32-bit and 64-bit instruction formats, providing flexibility for different computing needs.

When programming in a high-level language like C or C++, the code is typically written using instructions that are more abstract and easier for humans to understand. To execute this high-level code on a RISC-V processor, it needs to be translated into lower-level instructions. This translation process involves converting the high-level code into assembly language, which is a human-readable representation of the processor instructions. Assembly language instructions correspond directly to the machine code instructions that the RISC-V processor can understand and execute.

The RISC-V processor core is responsible for executing the machine code instructions. It contains the necessary digital logic circuits, such as arithmetic logic units (ALUs), registers, and control units, to perform the operations specified by the instructions. The RISC-V core interprets the machine code instructions sequentially, executing them one by one to perform the desired computations or operations.

After the design of the RISC-V core, it needs to be fabricated into an actual physical layout. This involves manufacturing the integrated circuit (IC) that incorporates the RISC-V processor. The fabrication process typically starts with a silicon wafer, onto which the various components of the RISC-V core are etched using techniques like photolithography. Once the fabrication is complete, the RISC-V core is integrated with other components, such as memory, I/O interfaces, and cache, to form a complete system-on-chip (SoC) or a microcontroller unit (MCU).

In summary, RISC-V is an open-source instruction set architecture used for designing computer processors. High-level language code is translated into assembly language and then further into machine code, which is executed by a RISC-V processor core. The designed RISC-V core is then fabricated into a physical layout to create a functional chip.
 
## iii. From Software applications to Hardware. 
 
 
Here the entire flow is shown from application to hardware.      
**System Software :**     
Application: The process begins with the application, which is typically written in a high-level programming language like C, C++, or Java. The application contains the desired functionality or logic that needs to be executed on a computer system.   

System Software: The system software plays a crucial role in translating the application code to hardware instructions. It consists of three main components: the operating system (OS), the compiler, and the assembler.   
- Operating System (OS): The OS provides an interface between the application software and the underlying hardware. It manages resources, handles input/output operations, and provides services to the application programs.   
- Compiler: The compiler takes the high-level code (e.g., C, C++) as input and translates it into the corresponding instructions of the target CPU architecture.     These instructions are written in the instruction set architecture (ISA) specific to the chosen processor, such as x86, RISC-V, ARM, etc.   
- Assembler: The assembler takes the output of the compiler, which is the code in ISA, and converts it into machine code. Machine code consists of binary instructions that can be directly executed by the hardware.   
- RTL Hardware Code: The machine code generated by the assembler serves as input to the register-transfer level (RTL) hardware code. RTL is a low-level hardware description language that describes the digital logic circuits and the interconnections between them. The RTL code specifies the behavior and functionality of the hardware components.   

Synthesis: The RTL hardware code is synthesized, which means it is transformed into a gate-level representation. During synthesis, the RTL code is converted into a netlist, which consists of a collection of logic gates (e.g., AND, OR, XOR) and flip-flops. This gate-level representation represents the hardware implementation of the desired functionality.   

Physical Design and Fabrication: After synthesis, the physical design phase begins. The synthesized netlist is further processed to create a physical layout of the chip. This involves placing the gates and interconnects on the silicon wafer and optimizing the layout for factors like performance, power consumption, and signal integrity. Once the physical design is complete, the fabrication process takes place, where the chip is manufactured based on the layout.   
      
# 3. SoC Design and OpenLane    
## i. Introduction to components of open source digital ASIC design.
The three main components required for open-source ASIC (Application-Specific Integrated Circuit) design are RTL IP's, EDA (Electronic Design Automation) tools, and PDKs (Process Development Kits).

RTL IP's: RTL (Register Transfer Level) IP's are pre-designed and pre-verified building blocks or modules that can be integrated into a larger ASIC design. These IP's provide specific functionality or logic and can save time and effort in the design process. Open-source RTL IP's are available from various sources, allowing designers to utilize and integrate them into their designs without reinventing the wheel.

EDA Tools: EDA tools are software tools used in the design, verification, and analysis of integrated circuits. Open-source EDA tools are available to perform tasks such as synthesis, simulation, place and route, and timing analysis. These tools enable designers to perform the necessary steps to convert their RTL code into a physical layout that can be fabricated into an ASIC.

PDKs (Process Development Kits): PDKs serve as the interface between designers and the fabrication process. A PDK contains files and information necessary to model a specific fabrication process in EDA tools. It typically includes Process Design Rules (DRC), Layout versus Schematic (LVS) checks, parasitic extraction (PEX), device models, digital standard cell libraries, I/O libraries, and other relevant data.

PDKs are essential for accurately representing the fabrication process during the design phase. They provide guidelines, constraints, and models that ensure the design is compatible with the chosen process technology. However, PDKs often come with non-disclosure agreements (NDAs) due to the proprietary nature of the fabrication process.

In June 2020, Google and SkyWater Technology released an open-source PDK for a 130nm process. This open-source PDK allowed designers to access the necessary information and tools to design and fabricate ASICs using the 130nm technology node, thus enabling a more accessible and transparent approach to ASIC design for the open-source community.

Overall, open-source RTL IP's, EDA tools, and PDKs play crucial roles in enabling open-source ASIC design, providing designers with the necessary resources and tools to create custom integrated circuits.

## ii. RTL2GDS flow.   

The RTL-to-GDS (Register Transfer Level to Graphic Data System) flow is a sequence of steps in the physical design process of an integrated circuit, starting from the Register Transfer Level (RTL) description and ending with the Graphic Data System (GDS) file, which contains the layout information for fabrication. Here's an overview of the main steps involved in the RTL-to-GDS flow:

**Synthesis**: In the synthesis step, the RTL description, which represents the desired functionality of the circuit, is converted into a gate-level netlist. This involves mapping the RTL components to corresponding components from a standard cell library, which contains pre-designed and pre-verified digital logic components.

**Floor Planning** and **Power Planning**: Floor planning involves determining the placement and arrangement of major components on the chip, considering factors like chip size, power distribution, and performance goals. Power planning ensures an efficient distribution of power and ground networks throughout the chip.

**Placement**: During placement, the gate-level netlist is mapped onto the physical chip layout. The goal is to place the components in such a way that interconnect delay is minimized and there is sufficient space for routing. Placement also considers constraints such as power, clock, and signal integrity.

**Clock Tree Synthesis**: Clock tree synthesis (CTS) is the process of generating an optimized clock distribution network. The objective is to deliver the clock signal to all sequential elements (e.g., flip-flops) with minimal skew and good clock shape. CTS ensures that the clock signal reaches each part of the chip efficiently.

**Routing**: Routing is the process of implementing the interconnections between the placed components using metal layers. It involves determining the optimal paths for the interconnect wires, considering factors like timing, signal integrity, and congestion. Routing aims to minimize delay and ensure correct signal propagation.

**Sign-off**: The sign-off stage involves performing various physical verification checks to ensure the design is ready for fabrication. This includes Design Rule Check (DRC), which verifies that the layout adheres to manufacturing rules, Layout versus Schematic (LVS) check, which compares the layout with the netlist, and Static Timing Analysis (STA), which verifies that timing constraints are met.

After successfully completing the sign-off stage, the final GDS file is generated. The GDS file contains the detailed layout information, including the geometric shapes and positions of the components, interconnects, and other design elements. This file is then used for the fabrication of the integrated circuit.

The RTL-to-GDS flow is a complex and iterative process that requires expertise in various areas of physical design and verification. It involves multiple iterations and optimizations to achieve a well-optimized and manufacturable chip layout.

## iii. Introduction to OpenLane.    
OpenLane comes with Apache version 2.0 license.   
The Main goal of OpenLane is Clean GDS2 without human intervention.   
It has 2 modes of operation.   
I) Autonomous : Where we just push a button post configuring the flow and we'll get final GDS2 file.   
II) Interactive : Here we runs commands step by step.     
It also has the feature called Design Space Exploration where it finds the best set of flow configuration.   

## iv. OpenLane detailed ASIC design flow.   

OpenLane comes with several abc scripts. In RTL synthesis, it translates RTL to logic circuit. This circuit can be optimized and mapped to standard library cell using abc.    
For DFT, it used opensource project called **FAULT**.   
For Physical Implementation we'll use OpenRoad.   
We need to check LEC using yosys.    
During physical synthesis, we've **Fake antenna diodes Insertion script**.
For STA, we use OpenSTA for timing analysis. 


# 4. OpenSource EDA Tools.

## i. OpenLane Directory structure.

OpenLane is not justa tool, its a flow comprising of many opensource EDA tools like yosys, openSTA, openRoad, etc.    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/14e1cce8-fd98-4e27-89fa-b9b01e91a315)


Here, inside PDK's we'll be using SkyWater pdk. In that we have Libs.ref and libs.tech.    
Libs.ref : contains tech related libraries.   
libs.tech: contains files that are specific to the tool.   


## ii. Design Preparation steps:   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/48b9cc47-6f16-40d1-8eb6-570d0cfd73d4)     


We use **Interactive** method in our case to do step by step process.    
we also include the packages.    


![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/5b3478a9-6b57-4e6c-876b-d7e2f447a214)


Inside picorv32a, we have several files.   
**src :** It contains RTL(.v) file + (.sdc) file.      
**Config.tcl**  bypasses any config. that has already done into a plane.  Inside it, it has clocl period, clock net, clock port, etc.   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/5967b79c-b6cc-4009-873d-adb6cb744ca7)


**Other Skywater files** : they can override the clock period and other defined things in config.     


![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/19ccd3ad-914c-42e9-a78e-a84ad0376666)



Here we're perparing the design by **Merging LEFs**. It will merge both "Cell and tech lefs."   


## iii. Review files after design preparation and run synthesis.   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/237adc3d-4269-4f38-a80f-0755c6b5b160)



Going into the folders : runs --> Date Folder --> tmp.    
Inside "tmp", we'll se merged.lef file is created which contains the info about levels, wires, vias, etc.   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/842e36f1-5a9e-4a70-84a2-7f18d62f90bf)


    
 Now we'll run the synthesis by command : "% run_synthesis".
    
## iv. Steps to characterize sysnthesis results.    

After synthesis is completed.   

We'll try to find the FLOP area. 

Flop Area = (No. of DFF's) / Total number of cells. 

![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/64eb0164-8957-4a12-a9d0-030956429ffa)

So, Flop Area = 1613/14876 = 0.10842 i.e. 10.8%.    
We can also check the above in synthesis report at the location : runs --> date --> reports --> synthesis --> "yosys_2.stat.rpt"

# 5. Chip Floorplanning and Considerations.    

## i. Utilization Factor and Aspect Ratio.   
Die : Consists of core and is small semiconductor material specimen on which the fundamental circuit is fabricated.    
Core : Core is the section of the chip where the fundamental logic of design sits.     

Utilization Factor = (Area occupied by netlist)/(Total Area of Core)      
Aspect Ratio = Height / Weight      


## ii. Concept of pre-placed cells.          
Advantage of seperate cells is that it increases the reusability.     
There are many block like these used for ex. Mux, FF, etc to be pre-placed.
This pre-placement has to be done before placement. 
Placement of these blocks will be fixed.  
Once they are fixed, they can't be moved.       

## iii. De-coupling Capacitors           
Due to long wire interconnect from power supply, therer will be resistance and inductance drop so the voltage that will be reaching to the other end will be less than Vdd.       
If that voltage decreases and goes below the Noise Margin Level, then it'll be in Undefined region and that'll be a problem for us.           
So we add de-couplinh high valued capacitor that will charge itself and discharge when needed.    
It is closely attached to critical blocks so no interconnect delay.        
 
 
## iv. Power Planning      
 It's not feasible to supply power to all the places from single source due to RLC effects in interconnect.  
 It may also lead to phenomenon like Voltage Droop and Ground bounce.     
 So we go for grid based power planning.     


## V. Pin-Placement :        
The above circuit has to be implemneted and we need to do pin placement for this circuit.     
Blocks in gery are pre-placed.        
The circuit pin placement is done in above image.   
Currently it is done in random type. Later we'll place the block according to our need.    
The clock port is bigger in size as it has to drive all the sequential elements at the whole chip.      


## VI. Steps to run floorplan using openLane.     
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/52c5dfa5-96c8-44d5-a3aa-2898aa866a0e)     
In Floorplan.tcl, the information about Metal layer is defined and along with that th IO_mode setting = 1. It means the IO ports are equidistant.    
Use the command : "run_floorplan"   

## VII. Review floorplan adn steps to view floorplan.     
Inside the floorplan results : 
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/097a08c4-7ebd-4725-8214-d229ce326a9e)      
we'l get to see the Die Area.    
In the format : (lower left x      lower left y)   (upper right x    upper right y).       
Now use the below command to open magic to see floorplan : 
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/8016b2fa-8099-4254-a3a0-52c312b8537b)    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/577eb40c-5d9a-4ba9-8916-e43e4ba148cf)   
Few commands to navigate magic :      
'S' = select.    
'Z' = zoom.     
'V' = Centre align.     
'left and rigt mouse click' = make a box.    

Now to find metal layer info. :   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/b8d37a1e-65b2-4e12-b069-156bd43ead8b)    
Use "what" after selecting the port to see the metal layer.    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/7aa56d88-f96d-4bc4-a314-61ffa4a6f355)     
Similarly for vertical ports :    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/57204608-0cfd-4329-9ec7-a103bd773d00)      
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/cc222520-c7e0-4991-8705-4bf7cb48cd34)     
Standard cells are diagonally equidistant from each other.    
They are kept in lower left.     
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/cbc1306e-dca1-4585-8aa5-ecb2ddae3077)     

# 6. Library Binding and Placement.     
## I. Netlist binding and initial place design.     
 In library alll the info. related to the cells be it gates and their delay, all is present.    
 There are various sizes are also available which helps in defining their drive strength. The more the size, lower the resistance, more the driving capability.   

 Coming to the placement of our earlier cirtcuit.  
 we've to place our netlist on the floorplan.    
 We also have to use some optimization techniques to adjust our placement and improve the performance.    
 
## II. Optimize placement using estimated wirelength and capacitance.        
 Now the circuit has been placed according to rows of standard cell.    
 Now we need to optimize the placement.    
 **Signal Integrity :** Here we use repeaters/buffers yo repliocate the original signal and send again basiclly we replenish its original value which was dropped. 
 By adding Repeaters, the Area will increase though.    
 
## III. Final placement optimization.   
 Here is the final optimized placement with buffers inserted.    
 
## IV. Need for library characterization and Congestion aware placement using Replace.     
 Library characterization is important for EDA tools as its gate properties and requirements should be known and identified by EDA tools.    
 
 After Floorplan, we have to go for placement.   
 We're doing congestion related placement and ignoring timing conbstraint for now.   
 For global placement, the main objective is to reduce wirelength.    
 So in openLane we use HPWL - half parameter wire length.    
 use command "run_placement".   
 ![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/47e45e68-3d9c-4ce3-972b-624d2aeacd99)    
 We'll see the overflow vbalue is converging, means our placement is going right.    
 Opening placement using the command :    
 ![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/4926892c-9c2b-4a20-a17e-267b8de090d8)     
 ![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/7c5b12fa-3220-433f-9da4-0642c4bb9fe0)      
 
 
 
# 7. Cell design and characterization flow.     
## I. inputs for cell design flow.    
 Inputs for cell design are :    
 process design kits (PDKs).   
 DRC and LVS rules.   
 Spice models.   
 Library and user-defined specs.    
 
## II. Layout design steps.  
First fore layout we go for EULER's path where it traces the path only once.    
We create seperate for both nmos network and pmos network.     
Then we create Stick Diagram.   
Then finally we create Layout.   

## III. Typical Characterization Flow.    
Characterization will help us to get our timing , noise and power informantion.    
First we'll take the buffer description.    
Then we extract the spice netlist.   
Recognize behaviour of buffer.    
Read sub circuits of inverter.    
Attach necessary power source.    
Apply stimulus as Vin.    
Provide necessary output capacitance.    
Transient DC simulation.       
Then we feed the characteristics to GUNA software to generate timing, noise and power characterization.   

# 8. General timimg characerization parameters.        
Rise time = 20% of the output value reached from bottom to reach 20% of the output value from top. 
Fall time = 20% of the output value from top to reach 20% of the output value from bottom. 
propogation delay : difference between output and input delay at 50%.    

Choosing a  threshold otherwise, the delay might be negative.   

# 9. Labs for CMOS inverter using ngspice.   
## I. IO placer revision.    
Earlier we saw that the ports were equidistant from each other as IO_mode is set to 1.  
Now we'll try to change that.   
use command : "set :: env(FP_IO_MODE) 2"     
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/487ad069-3a5a-407d-bf2b-c32783552afa)     
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/946c0303-b0cc-4bde-ab1e-2d4ac43bb08d)     
As seen above all the IO ports are assigned to lower left corner.    

## II. SPICE deck creation for CMOS inverter.   
  

The syntax for spice decl is Name Drain Gate Source Substrate Type Width Length.    
For capacitive load/ Supply Voltage : Name Connection_from Connection_to Load_Value.

Simulation commands : dc/transient Vin From_V To_V Steps.   

**Switching Threshold:** 

Both The waveform shape is same even though the width is different, this shows the **robustness** of CMOS inverter.    
Both have different Width ---> Vm is shifted.     
Vm is switching Thresold, where both NMOS & PMOS are in saturation region.   

# 10. Inception of layout 16-mask CMOS fabrication process.     

**Create Active Region :** Select substrate. usually P-silicon substrate.   
**Create active region for transistors :**     40nm SiO2 --> 80nm Si3N4 --> 1um layer of photoresist.   We use lithography here.  Mask protects photoresist.    
Then remove mask, Si3N4 and photoresist. Then put in oxidation furnace for LOCOS. Now we get our active regions.   
**Create N-well and Pwell Formation :** Use Mask --> Boron --> breaking oxidizing layer forms P-well.    
Similarly do for N-well using phosphorus. Now put in high furnace that will drive_in phosphorus & boron substrate to make clear wells.   
**Formation of gate terminal :** Using low energy boron, create p type in p-well and similarly using arsenic do for n-well. Now break and regrow the oxidization layer as it might have been damaged. Further use different types of exposition so that gate should be low resistive.       
**Lightly doped drain(LDD) :** We use specific doping order here because of 1. Hot electron efect and 2. Short channel effect. Again using more low energy create wells.  
Finally using plasma anisotropic etching, protect centre oxide layer from etching.    
**Source & Drain formation:** Using thing oxide layer and masks, create the drain and source using arsenic and boron with low energy. Then using Annealing process, push n+ and p+ into more substrate.   
**Local interconnect formation :** Remove thin oxide and using **Sputtering** deposit titanium on wafer surface. Then again heated and we'll get low resistive interconnect TiSi2. TiB is only used for local communication. Now with RCA Cleaning, we'll get clean YiN local interconnects. 
**Higher Metal Layer formation :**  first deposit 1um of SiO2 wit P and B as glass and deposit it on the wafer to make planar and protect the surface from higher temperature.   
Then drill contact layers same way we do using mask. Further, Deposit blanket tungsten(W) layer deposition.   
Again use CMP for planarizing the surface. Then deposit Al layer. Keep doing this unril finally we reach the below image. 


# 11.Labs for layout & SKY130 tech file labs :    

## I. steps to git clone vsdstdcell design.    

We use below command for github clone :    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/afe370dd-9d29-45a7-bd0a-e8b6011264cf)   

Next --> Copy the magic's tech file of sky130A : 
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/df659ceb-a0cd-4ead-9f4d-f61818761e7b)     

Opening Command :   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/98adb9e4-4e1e-431c-8593-84c95c2500e3)    

![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/4b684c80-125c-468b-93e4-9454ab2c6539)    
Cheking the region by selecting :   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/a23a0cba-90a6-4a8d-8be2-55cef52cead3)    

To select a connection : Press S twice, hovering the mouse on that region.    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/f6c1ffdd-26c3-4dcd-bf1a-3706e235b674)    

Deleting some of the area to create DRC violations :    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/be067860-b5ca-4fcf-8a6e-56ed128e8834)    

To identify the region of DRC error : goto DRC FIND NEXT ERROR.    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/b882de65-6de8-4933-b8ba-f247464c36fe)    

In tkcon, it;ll describe the error :    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/fe9f36e1-55ec-4201-96ef-c337f8866f11)    

Now extract the file including parasitics :   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/e5880f23-bcd9-46a3-8e13-ad9e76432caf)   

open the extracted file using vim :    
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/31878358-8718-4ee9-8aca-6b99a91f78ce)    

## II. create final SPICE deck using sky130 tech:    

Now make changes in extracted spice file :   
![image](https://github.com/YashpalRathod14/Physical-Design/assets/135158197/0956b3ff-f037-4072-b8c3-dc3f27fa6ddb)    

use command : ```ngspice sky130_inv.spice``` to open that file in ngspice.     
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/47d736bd-a763-4b02-9db2-8b59e7213a80)     

Now to plot we can use : ```plot y vs time a```.     
Below is the plot of our inverter.   
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/06dd8bf1-4c9a-40e3-9a6c-8c90548ff3e8)     

Now we'll zoom in and try to find the rise time of inverter.    
On clicking on the required waveform at that value we'll get t he value printed in terminal.    
For rise time, we choose 20% of Vdd to 80% of Vdd.
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/aaec27be-698c-4160-9a4e-d65c8727e016)    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/34eba1ee-15e2-428e-959a-86b7b3d2cd03)     

Using above 2, we can calculate the rise time of our inverter.    

Now well find propogation delay.   
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/98f33351-7561-491a-9d95-795f01f69763)
propogation delay will be difference between output and input at 50% of Vdd.    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/105a8dab-6118-4ca5-b9ee-6220150ad6bc)    

## III. Magic Tool and DRC rules.    

First we'll get to know about the documentation of magic at : ```http://opencircuitdesign.com/magic```     
Navigating the site, we'll get 2 useful directories : Using Magic and Technology Files.   
```Tech File``` : It has layer types, colours, patterns, electrical connectivity, DRC rules, extraction rules for generating netlist, rules for LEF and DEF etc.   
```Using Magic``` It is sort of tutorial documentation for guiding us how to use magic and command shortcuts.   
```Design Rules``` and ```peripheral rules``` they tells us about specific violations for all the layers.    

Steps to download Magic  :    
we use ```wget``` called web get as non-interactive downloader command.    
Use ```wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz``` in our home directory to download it.    
Now we have to extract that file using ```tar```.    
use command ```tar xfz drc_tests.tgz``` to extract the files.    

![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/725c648f-0ff8-4560-8c3b-b1b6dc273dc2)    

Now to open magic in high resolution : ```magic -d XR```   

## IV. Introduction to Magic   
   

Magic Tablet: It refers to the software tool called Magic, which is used for integrated circuit layout design.    

```Derived Layers and Via```: The Magic tablet utilizes various derived layers, and one of them is the "via" layer. A via represents an area filled with contact cuts, which are used for connecting different metal layers.    

Feedback Entry and Feed Clear: The view you see on the screen is a feedback entry. You can dismiss it by using the command ```feed clear```    

Snap Internal Command: The ```snap int``` command is used to make the cursor move along the edges of the grid.    

Steps to import metalfile : ```File --> Open --> metal. met3.mg```.

using command ```drc why``` we can check the reason behind the DRC violation in selected area.    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/339e7d7f-65bc-4a9d-a0d7-9c7c6c46e986)   

Metal 3.4 DRC Rules: These are design rules for metal layers in integrated circuits. According to the rules, Via2 (a type of via) must be enclosed by Metal3 layer by at least 0.065u (unit of length). 

Drawing M3 Contact: Using the mouse pointer, you can hover over the M3 contact icon on the side toolbar and draw a large area of M3 contact on the layout.    

Command ```cif see VIA2```: With the cursor boxed around the M3 area, you can type the command "cif see VIA2" This command creates contact cuts in the shape of black squares. These contact cuts don't physically exist on the drawn layout but represent a mask layer called VIA2, which will be included in the GDS (Graphic Data System) file. Further details about VIA2 can be found in the CIF (Caltech Intermediate Format) output section of the tech file.
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/f632a288-ce23-400c-b585-dc408d12432f)     


Spacing Rules: There are specific spacing rules mentioned, such as the spacing between Via2 and Metal 3 being 0.065u. These rules ensure that the contacts are placed correctly and meet the design requirements.    

Measuring Distance: By putting the cursor between the contact cut and the drawn via edge and using the "tkcon" command, you can obtain the distance between them. This allows you to verify that the tool ```automatically places``` the contact cuts with the appropriate spacing, ensuring there are no Design Rule Check (DRC) errors. The distance should not be smaller than the specified spacing value.    

![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/cbc4ae22-703a-4a82-b0b2-1263a89c68d4)     

## V. fix poly.9 error in DRC.    

First load the file ```poly.mag```   

According to the documentation the constraint is  :    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/753b7b8f-79c0-45be-8c65-44190513cd70)     

So we check the distance :    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/a0bb52cf-ebfe-4325-abef-3760e0c5e65b)    
It seems that there is no error.     

Now To create a rule defining spacing between polyres (poly resistor) and poly (poly silicon) : 
Open tech file : ```sky130A.tech```   
 If there is no existing rule defining the spacing between polyres and poly, add a new rule in the tech file. Specify the desired spacing value for the rule, which represents the minimum distance that must be maintained between polyres and poly.     
 ![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/2cfa8f9e-9648-4e0d-a4bf-abcac3bb6f3f)    
 ![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/b1a90260-bee0-421f-8da9-b5db7f9ba229)     
 
 Using ```tech load sky130A.tech``` & ```drc check```.    
 
 ![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/19d54e5b-9882-464c-ad93-d60bda7dbac9)     
 
It's possible that after resolving the spacing violation between polyres and poly, violations between poly and tap/diff (tapping or diffusion layers) may arise. To address these violations, you will need to provide additional spacing rules for tap/diff in the tech file.   
Modify thw tech file again :     
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/da463509-0f38-4692-9316-7c6d20497591)     

Again using ```tech load sky130A.tech``` & ```drc check``` the error is resolved.    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/22af8793-a210-4146-90a6-04804cb47db0)     

# 12.  Pre-layout Timing Analysis.     

## I. Convert grid info. to track info.     

During the placement phase in the design of integrated circuits, it is not necessary to have the entire detailed information of the individual cells within the circuit. Instead, only certain key information is required, such as the PR (placement and routing) boundary, I/O ports, and power and ground rails of the cell. This information is typically defined in a specific file format called the Library Exchange Format (LEF).    

The main objective during placement is to extract the necessary information from the cell, usually represented in a layout file format known as the Magic (MAG) file, and integrate it into the overall design flow of the circuit.    

Before delving further into the topic, it is essential to understand the concept of tracks in the context of integrated circuit design. In this context, a track refers to a line or path on which metal layers are drawn for routing signals. Tracks are used to define the height of the standard cell, ensuring proper alignment and spacing of circuit components.    

When implementing a custom standard cell, there are specific guidelines that need to be followed. For instance, the I/O ports of the standard cell must align with the intersection points of the horizontal and vertical tracks. This ensures that the input and output connections of the cell are properly aligned with the routing resources.    

Furthermore, the width and height of the standard cell need to be specified in a way that adheres to certain design rules. Specifically, the width and height should be odd multiples of the horizontal and vertical track pitch, respectively. The track pitch refers to the spacing between adjacent tracks, and following this guideline helps ensure proper alignment and compatibility with the overall routing grid.    

By extracting the necessary information from the LEF file, which contains details about the PR boundary, I/O ports, and power and ground rails, designers can integrate the standard cell into their design flow, ensuring that it aligns with the track-based routing resources and meets the required design guidelines. 

First open the layout of inverter.   
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/5f005dd1-bd17-4ba2-aff9-a51a108f6090)     

Now we'll check track info. on ```vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd$ less tracks.info```      
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/27fb7db0-ef26-46df-9296-7a88ac30165e)     

Now press `G` to activate the grid.     
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/1033f315-f083-4bcc-bad9-27b66cec2deb)     

The I/O ports are in `li` layer so we'll check if they intersect at the grid.     
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/23a34bae-c9b0-4615-8785-ed72426cf970)    

The below image shows the intersection of metal layer at I/O ports.    
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/e0d53b3e-f688-4d89-b45b-f316e48ce0ca)    

## II. Convert magic layout to std. cell design.     

A cell with ports is called a macro cell. Ports are defined as pins within the macro. The objective is to extract information from a MAG file to generate a corresponding LEF file. The MAG file contains layout details, including the pins. By analyzing the MAG file, information such as dimensions and pin locations can be extracted. This information is then formatted into the LEF file, which represents the macro cell in a standardized format. Generating the LEF file ensures compatibility and proper integration of the macro cell into the circuit design.    

Here for inverter, in magic layout ```EDIT --> TEXT```.    
It'll create a dialog box(text helper), now using `S` multiple times choose input port - A, output port - Y, VDD and GND.     

As soon as we select, few things will be auto-filled ike label name, Rotation, etc.    
Make necessary changes as below :   
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/32cbda78-9670-4bf5-8592-1265149b8998)
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/d9e3eb1b-4b13-40db-a6c1-45fe549089a0)
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/34fa61b4-4376-4df7-b334-7ab78faa0bb4)
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/0572dca1-3f9a-40c9-b800-21355178ab54)   

Now as the parameter are set, we'll extract the .lef file.    

## III. Custom cell naming.    

Now extract the above using the command `save sky130_yash.mag`.    
This will extract the file     
![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/a76a9212-6ac7-42e8-8d5f-735624688267)    

To open that use : ```magic -T sky130A.tech sky130_yash.mag```    

Further extract the lef file by `lef write` in tkcon.    

![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/5161c6ed-6a04-49a9-a082-4bd96965460f)    

Opening the .lef file we'll see the pins are created because when we set a layer to port, a PIN is formed.   
Also the details of ports are in the order we gave to port as 0 being the highest order.    

![image](https://github.com/YashpalRathod14/Physical_Design_Updated/assets/135158197/d44a85dd-103d-4155-824c-d23d56af8ec1)    


