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

  **Pads:** Sends signal in and out of the chip.  
  **Core:** Where the digital logic sits (ex. mux, gates, etc.).  
  **Die:** Size of the entire chip. It is basically manufactured on silicon wafer.  
  
## ii. Introduction to RISC V


 Basically the high level language code is converted to assembly level and then to Machinbe Code.   
 The above is implemented on cpu core, in our case RISC V and then it is fabricated into layout.   
 
## iii. From Software applications to Hardware. 
 
 
Here the entire flow is shown from application to hardware.      
**System Software :**  It handles the translation from application to hardware.  
      It has OS, Comnpiler and Assembler.
      Compiler translated C, C++, etc. high level program to Instruction Set of the cpu architecture (ex. x86, RISC V, ARM, etc.)    
      Assembler translated this ISA to machine code which further fed to RTL of our hardware.     
      That RTL hardware code is synthesized and after physical design of the synthesized netlist, hardware fabrication is carried out.    
      
# 3. SoC Design and OpenLane    
## i. Introduction to components of open source digital ASIC design.
 
 
  There are 3 main things we need for open source ASIC design.    
  RTL IP's, EDA tools, and PDKs.    
  
  
 we already avail open source EDA tools and RTL IPs from above mentioned sources.    
  **PDK (Process Development Kit) :** Its basically the interface between the FAB and the designers. It contains filkes to model a fabrication process for EDA tools used to design our IC. 
  It contains :    
  Process Design Rules : DRC, LVS, PEX...     
  Device Models.   
  Digital Standard cell libraries.   
  I/O libraries and so on.   
  
 PDKs contains NDA, so it made difficult to fabricate for masses. But in june 30, 2020 : Google and SkyWater released OpenSource PDK for 130nm processor. 
 
 So now we have Open source PDKs too to make full open source ASIC design.
 

## ii. RTL2GDS flow.   

**Synthesis :** Converts RTL to circuit out of components from standard cell library.    
**Floor Planning & Power Planning :** Objective is to plan whole chip's silicon area.   
**Placement :** Components are placed together in order to reduce interconnect delay and also to get successful routingh afterwards.    
**Clock Tree Synthesis :** To deliver clock to aLL Sequential elements with minimum skew and in good shape.   
**Routing :** Implementation of interconbnect using metal layers.    
**Sign Off :** Physical Verifications like DRC, LVS check along with STA.    

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

use command : "ngspice sky130_inv.spice" to open that file in ngspice.    








