# Advanced-Physical-Design-Workshop-Using-OpenLANE-SKY130
![image](https://user-images.githubusercontent.com/86793947/124155404-14087880-dab4-11eb-8824-a9484566fc18.png)
## TABLE OF CONTENTS
### ABOUT THIS WORKSHOP
   A 5 day workshop focusing on both theoritical and practical concepts of the RTL to GDSII flow with the help of opensource tool OpenLANE .The tool used the Google Skywater 130nm PDK files for the implementation.
### DAY 1-Introduction to Open Source EDA'S,openLANE and SKY130
```
1.How to talk to computers
2.Interfacing Software applications to Hardware
3.Digital ASIC Design
4.OpenLANE ASIC Flow
```
### DAY 2
### DAY 3
### DAY 4
### DAY 5

## DAY 1-Introduction to Open Source EDA'S,openLANE and SKY130
### HOW TO TALK TO COMPUTERS
  Integrated circuit (IC) is an assembly of electronic components fabricated as a single unit, in which miniaturized active devices (e.g., transistors and diodes) and passive devices (e.g., capacitors and resistors) are present.The most important component on a integrated circuit is the chip which usually is present in the center of any pacakage.The core of the particular chip is where the digital logic resides.The packages contains several Foundry IPs,macros etc.,
      ![image](https://user-images.githubusercontent.com/86793947/124161468-115d5180-dabb-11eb-91c6-889fcfb8ea60.png)
      
### INTERFACING SOFTWARE APPLICATIONS TO HARDWARE
  To run a software application on a hardware,we need a system software to smoothly translate the higher level software language to machine understandable code.
  
  Components of a System Software
  1. Operating System
  2. Compiler
  3. Assembler
  
  ![image](https://user-images.githubusercontent.com/86793947/124162125-cabc2700-dabb-11eb-8fe9-be632ca8f83b.png)

The Operating System handles the input/output operations and it also allocates memory.It takes the high level applications and converts them into assembly language program.

The compiler converts the codes which was written in a programming language to hardware instructions and saves them as an .exe file.Output of the compiler is hardware dependent as the instructions should be compatible to the hardware.

Assembler converts the compiler instructions to binary language  which can be understood by the hardware.

### DIGITAL ASIC DESIGN
  To design ASIC,the following components should always be present
  
  1.RTL IP - eg: Opencores,Librecores
  
  2.EDA TOOLS - eg: Qflow,OpenROAD,OpenLANE
  
  3.PDK DATA -eg: Google+Skywater 

  With the growth of opensource design websites the availability of these components are hugely increased.

**The general ASIC design flow is as follows :**

  1.**Synthesis** : 
  
  The given RTL circuit  is converted to a circuit of components from the standard cell libraries and a netist is produced.
  
  2.**Floor planning and power planning** :
  
  The silicon area is planned and a robust power distribution is created.The floorplanning can be divided into:
  
  Chip Floorplanning: The chip is partitioned between different system building blocks
  
  Macro Floorplanning:The macro dimensions and the pin locations are provided along with the routing tracks.
  
  Power planning deals with the power distribution of the chip.A chip is powered by several Vdd and Gnd pins.The power pads are connected through rings and horizontal and vertical power straps.Typically the power distribution uses the upper metal layers as they are thicker than lower metal layers.
  
  3.**Placement**:
  
  Places the gate level netlist cells on the virtual rows.The connected cells are placed closer in order to reduce the interconnect delays and to achieve optimized routing.
  
  4.**Clock Tree Synthesis**
  
  Creates a clock distribution network where the distribution resembles a tree,the clock source being the parent node and all the components functioning with the clocks acting like leaf nodes.
  
  5.**Routing**
  
  Given a placement and a fixed number of metal layers ,it is required to find a valid pattern of horizontal and vertical layers to connect the nets.The router uses all the available metal layers and also the vias mentioned in the PDKS
  
![image](https://user-images.githubusercontent.com/86793947/124164271-1f60a180-dabe-11eb-8cda-98043b019425.png)
 
The final steps includes Physical verification which contains Design Rule Check,Layout vs Schematic and the Timing Verifications checks.

### OpenLANE ASIC FLOW

There are various steps OpenLANE ASIC flow . The flow starts with feeding the RTL design and ends with layout in GSII format. The PDKs are being introduced to support the flow of the total design . OpenLANE is dependent on several opensource tools such as Yosys,abc,Klayout,magic etc.,

The complete flow of the openLANE tool is as follows

![image](https://user-images.githubusercontent.com/86793947/124167009-2210c600-dac1-11eb-845b-527c4d3e2d0d.png)

Synthesis
    
      Yosys - RTL synthesis is done using Yosys
      abc - abc mapping is done to map the specific technology to the standard cells as mentioned in the PDK. Performs technology mappin to standard cells described in the PDK.
      OpenSTA - Performs static timing analysis on the obtained netlist
      Fault – It is an optional DFT tool.This tool contains provisions for scan-chain insertion , test patterns compaction and Fault coverage

Floorplan
     
      Init_fp - Defines the core area which is the initial step in floor planning
      Ioplacer - Places the input and output ports
      RePLace - Global placement is done
      OpenDP - Detailed placement is performed for the globally placed components
      Resizer - optional optimizations for the design
      
     
 CTS
  
      TritonCTS - Clock distribution network is being synthesized
  
Routing

      FastRoute - Performs global routing  for the detailed router to make it more specific
      SPEF-Extractor - Performs SPEF extraction 
  
GDSII file Generation

      Magic - Takes out the final GDSII layout file 
     
 ## INVOKING OpenLANE
  
  OpenLane can be used in both automated and interactive flow.Here the interactive flow is being explained
  
  **Command to invoke the interactive flow**
  
    ./flow.tcl interactive 
     
  ![image](https://user-images.githubusercontent.com/86793947/124169281-b0864700-dac3-11eb-8452-7abc39d4910e.png)

The OpenLANE shows that it is running using the interactive flow.The next step is to import the packages .For importing the packages use the command

    package require openlane 0.9
    
  ![importing package](https://user-images.githubusercontent.com/86793947/124169501-ef1c0180-dac3-11eb-9e88-b57b5de97cee.PNG)

Now that the packages are imported the design can be prepared to run.

 **Preparing the design**
 
 OpenLANE has a huge variety of predefined designs under the designs directory .They can be accessed as follows:
 
 ![image](https://user-images.githubusercontent.com/86793947/124169807-491cc700-dac4-11eb-9b03-a158d5c9acdb.png)

For this lab,the picorv32a design is considered.So this design should be prepared to run using the command

     prep -design <design_name>
     eg: prep -design picorv32a
       
  ![prepdesign](https://user-images.githubusercontent.com/86793947/124169990-81bca080-dac4-11eb-8ad9-4853716fa7e0.PNG)
  
**Synthesis**

To run synthesis the following command is used
    
    run_synthesis
 
 ![synthdone](https://user-images.githubusercontent.com/86793947/124170471-17f0c680-dac5-11eb-8d7b-755f437737b6.PNG)

 Synthesis starts running and the resulting netlist is obtained.The log of the synthesis is found under the general log file which is generated for every run under /designs/design_name/runs
 
 The netlist is available in the synthesis directory under the /designs/picorv32a/runs/01-07-08_35/results/synthesis
 
![image](https://user-images.githubusercontent.com/86793947/124171074-b54bfa80-dac5-11eb-96ad-1760c476eb14.png)

The generated netlist looks like the following

![netlist](https://user-images.githubusercontent.com/86793947/124171184-d9a7d700-dac5-11eb-80ed-881c462cafc0.PNG)


## DAY 2-Floorplanning and Introduction to Standard Cells

### CHIP FLOOR PLANNING CONSIDERATIONS

1.Definition of core and die width and height

2.Locations of preplaced cells

3.Placement of decoupled capacitors

4.Pin placement

5.Logical cell placement blockage

**Definition of core and die width and height**:  The core and die area depends on the dimensions of logic gates,hence the dimensions of the chip.The dimensions of the core and die is also dependent on the standard cell dimensions.The wires doesn't contribute to these dimensions.

**Utilization factor** :Utilization factor is defined as the area occupied by the netlist defined circuit in the core.Itt can be calculated as

     Utilizaion factor = Area occupied by the netlist
                        --------------------------------
                         Total area of the core
                         
**Aspect ratio** :  Aspect ratio can specify the shape of the chip. An aspect ratio of 1 discribes the chip as a square else it is a rectagle.Aspect ratio can be calcuated by

    Aspect ratio = Height
                  --------
                   Width

**Preplaced cells** : A huge circuit can be granularized into smaller circuits and can be black-boxed.These IP's can be instatiated many times but one time implementation is enough.

**Decoupling Capacitors** : Decoupling capacitors are placed local to preplaced cells during Floorplanning.When a particular toggle happens in a circuit,these decoupling capacitors will send current to the circuit and there will be no voltage drop.Whenever there is no switching ,the capacitor refreshes itself by charging to its maximum extent.

**Power Planning ** : **Power planning during is very important to lower noise in digital circuits.This will resolve issues like ground bounce and voltage droop Coupling capacitance is formed between interconnect wires and the substrate which needs to be charged or discharged to represent either logic 1 or logic 0. When a transition occurs on a net, charge associated with coupling capacitors may be dumped to ground. If there are not enough ground taps charge will accumulate at the tap and the ground line will act like a large resistor, raising the ground voltage and lowering our noise margin. To bypass this problem a robust PDN with many power strap taps are needed to lower the resistance associated with the PD**





      
  
  






