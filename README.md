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
### DAY 3 -Library Cell Design using Magic and ngspice

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
  
## SYNTHESIS

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

**Power Planning** : Power planning is done on a global level and it is very important for digital circuits which are prone to voltage droop and ground bounce.Whenever all the capacitors discharge to the same ground tap point,there will be a bump in the ground tap point.This is called ground bounce.Similarly if the voltage reduction takes place when the supply voltage is tapped from the same point,its called voltage droop.Instead of a single power supply,if there are multiple power supplies,these issues can be avoided.

**Pin Placement** :The placement of input/output ports vary from designer to designer.Ordering of these ports are completely random and this depends on the placement of the cells.Ports can be placed close to the cells which make use of them.The design shoud be properly known to arrange the pins. In many cases, optimal pin placement will be accompanied with less buffering and therefore less power consumption.

**Logical Cell Placement Block** :Logical Cell Placement blocks are placed to differentiate between the core area and the I/O area.Cells should not be placed in the area where the pins are placed.

## FLOORPLANNING 

To run floorplanning the following command is used

    run_floorplan
  
  ![run_floorplan](https://user-images.githubusercontent.com/86793947/124254811-136cf200-db47-11eb-9412-22501833abe9.PNG)
 
 The floorplan is run according to the configurations provided in the config.tcl file.
   
   ![floordone](https://user-images.githubusercontent.com/86793947/124255099-6a72c700-db47-11eb-9b6f-fd42dfbfc4ac.PNG)

The obtained floorplan can also be viewed in magic using the command
    
    magic -T /home/Desktop/username/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef/ def read picorv32a.floorplan.def &

![image](https://user-images.githubusercontent.com/86793947/124256635-27195800-db49-11eb-8913-005042a44f62.png)

View after executing floorplan in magic

![floorplan](https://user-images.githubusercontent.com/86793947/124258780-69dc2f80-db4b-11eb-8679-2c7eb79d3b8a.PNG)

When zooming into the floorplan,we could see the standared cells and the input /output ports

![closer look at standard cells after placement](https://user-images.githubusercontent.com/86793947/124258875-87a99480-db4b-11eb-9236-ccf6d46b43cc.PNG)


## PLACEMENT

Placement is done in two stages :


1.Global placement

2.Local placement
 
Global placement doesn't focus on legalizing the cells.They can be overlapped or even placed outside the boundary but in local placement the randomly placed cells are legalized and the placement is optimized.

To run placement use the following command
   
    run_placement
    
   ![run_placement](https://user-images.githubusercontent.com/86793947/124258692-516c1500-db4b-11eb-8b82-75e34bd8756f.PNG)
   
After running placement it looks like the following

   ![placement report](https://user-images.githubusercontent.com/86793947/124259037-b293e880-db4b-11eb-82c8-d7a2710b38aa.PNG)

This can also be viewed in magic using a command similar to that of the one used to view the floorplan

   ![placementmagic](https://user-images.githubusercontent.com/86793947/124259306-fedf2880-db4b-11eb-9a65-0eb7856e8200.PNG)


## DAY-3 -Library Cell Design Using Magic and ngspice
 
### SPICE DECK 

Deck wrappers are required to be generated around model files for standard cell simulation.

1.Connectvity of Components

2.Component Values

3.Identification of nodes

To plot the output waveform of the spice deck  ngspice is used. The steps followed to run the spice deck is ngspice is:

Source the .cir spice deck file  using the command `source spicedeck.cir`

`run`command is used to run the spice file

`setplot` allows to view plots possible in the simulations specified in the spice deck

Select the simulation by entering the  name of the simulation in the terminal

`display` command is used to see nodes for plotting

 ### 16 MASK CMOS PROCESS
 
 1.**Selecting a substrate** : a base where the whole chip is being fabricated.P type silicon substrate is mostly preferred.
 
 2.**Creating active regions for transistors** : The PMOS and NMOS are placed in the small pockets called the active region.These pockets should be isolated so they dont          interfere with each other.The pockets are created using photoithography
 
 3.**N well and P well formartion** :N well will be used for pmos fabrication while p well is used for nmos fabrication.
 
 4.**Gate formation** : The doping contentration and the oxide capacitance are two important parameters to control the Threshold voltage.Threshold voltage decides the              functioning of the gate
 
 5.**Formation of Lightly Doped Drain (LDD)** : A doping profile is achieved to avoid hot electron and short channel effects.
 
 6.**Source and Drain formation** : Source and drain are formed. 
 
 7.**Contacts & local Interconnect Creation** :Removing the screen oxide and opens up the source drain and gate region to build the contacts.TiN layer is used for local            communication.
  
 8.**Higher Level metal layer formation** : Deposition on upper layers of metal and contact holes are drilled.

### CMOS INVERTER ngspice SIMULATIONS

The inverter cell will take time to build from scratch so the .mag file can be downloaded from github.From the .mag file the post layout simulation can be done in ngspice and post characterizing the cell.This is plugged into a picorv32 design and the observations are noted.

The github repository to clone the cell is as follows

    git clone https://github.com/nickson-jose/vsdstdcelldesign.git
    
 ![image](https://user-images.githubusercontent.com/86793947/124231103-7a7dad00-db2d-11eb-8c48-39803db4606d.png)

The tech file for the magic will be present in the pdk directory and it can be copied to the vsdstdcelldesign directory

![image](https://user-images.githubusercontent.com/86793947/124232004-aa798000-db2e-11eb-9627-6dbf320e518e.png)

To view the layout ,the following command is used 

    magic -T sky130A.tech sky130_inv.mag &

 ![image](https://user-images.githubusercontent.com/86793947/124232270-12c86180-db2f-11eb-81ec-b715ce90e2f5.png)
 
 The layout of the inverter appears in the magic and looks as follows
 
 ![image](https://user-images.githubusercontent.com/86793947/124232428-460af080-db2f-11eb-9579-1bd33d9835f5.png)
 
 The color palletes present in the magic tools helps us to figure out the layers in the layout.From the layout ,it can be seen that the gate of the both transisitors are connected to the input and the drain of both the transistors to the output,the source of nmos to ground and the source of pmos to vdd.This basically sinifies the structure of an inverter.
 
 ![image](https://user-images.githubusercontent.com/86793947/124268076-af522a00-db56-11eb-8111-36f6b83b2f1d.png)

The top right corner specifies the name of the alyer for each of the color selected.

To get to know the particular device formation,select an area and type `what` in the tkcon window.It gives the information regarding the device 

![image](https://user-images.githubusercontent.com/86793947/124268481-2edff900-db57-11eb-8127-167827692a84.png)

To check the connections ,press S twice while placing the cursor near the required connection.The white highlighted region confirms the connection.

![image](https://user-images.githubusercontent.com/86793947/124268753-8e3e0900-db57-11eb-80e3-28e9d0d089d2.png)

**CHECKING OF DRC ERRORS**

When there are DRC errors it is denoted by white dotted lines and are also specified in the DRC tab above the window.

![image](https://user-images.githubusercontent.com/86793947/124270110-618af100-db59-11eb-9d37-917743da1a2d.png)

**EXTRACTING SPICE NETLIST**

To extract the layout on spice open the tkcon window and use the command `extract all`

![image](https://user-images.githubusercontent.com/86793947/124270446-d78f5800-db59-11eb-8941-fec7d609eb58.png)

Now when we check the vsdstdcelldesign directory a new file of .ext extension appears which is the extracted file.

![image](https://user-images.githubusercontent.com/86793947/124270674-263cf200-db5a-11eb-88ce-cf585ccc29c4.png)

This ext file is used to create the spice file to use with the ngspice tool.

The parasitic capacitances have been removed and the file is extraced to spice through the following commands

![image](https://user-images.githubusercontent.com/86793947/124271094-a95e4800-db5a-11eb-8415-718c15ffdc45.png)

This creates a .spice file in the vsdstdcelldesign directory

![image](https://user-images.githubusercontent.com/86793947/124271194-c98e0700-db5a-11eb-914f-eef7f632d229.png)

The spice file contains the following parameters

![image](https://user-images.githubusercontent.com/86793947/124271323-f6dab500-db5a-11eb-9595-e59ed363959a.png)

Here we have edited the netlist to run transient analysis as follows 

![image](https://user-images.githubusercontent.com/86793947/124312707-61a4e400-db8d-11eb-8ba1-a792c67958d9.png)

To invoke ngspice use the command
    
    ngspice sky130Ainv.spice
    
  ![image](https://user-images.githubusercontent.com/86793947/124314520-29eb6b80-db90-11eb-9d5a-25f9b48416a2.png)

  
  The output of the spice window properly depicts the functionality of an inverter
  
  ![image](https://user-images.githubusercontent.com/86793947/124314620-4edfde80-db90-11eb-996b-374ec131ee05.png)

Four timing parameters are calculated.They are:

Rise transition delay = Time taken for the output signal to reach from 20%  to 80% of max value.

Fall transition delat = Time taken for the output signal to reach from 80% to 20% of max value.

Cell rise delay = Time difference between 50% of rising output and 50% of falling output

Cell fall delay = Time difference between 50% of falling output and 50% of rising output

Eg:

Rise transition delay:

20% of max value 

![image](https://user-images.githubusercontent.com/86793947/124315196-38865280-db91-11eb-9f4f-d821308ac2c4.png)

80% of max value

![image](https://user-images.githubusercontent.com/86793947/124315594-c6623d80-db91-11eb-91dd-ce5177680b92.png)

Rise time =80%-20%

## DAY 4 - Timing Analysis and Clock Tree Synthesis

## EXTRACTING LEF FILES

The lef files are extracted and plugged into the picorv32a design and the challenges are observed

Technology LEF - Contains layer information, via information, and restricted DRC rules
Cell LEF - Abstract information of standard cells

There are two conditions

1.The ports should be on the intersection on the horizontal and vertcial track.

2.The width of the standard cell should be in odd multiples of x pitch in that layer.

Tracks are used in the routing stage.Tracks can be laid over the routes.The track information is present in the pdk files as tracks.info file

![image](https://user-images.githubusercontent.com/86793947/124344456-de68aa00-dbef-11eb-9042-83aa5aa1b01e.png)

For an li1 layer the horizontal offset is 0.23 and has a pitch of 0.46 for the X pitch.Similarly the track information for Y is also present.This is present for every metal layer.The ports should be on the intersection on the horizontal and vertcial track.

To verify if the ports of the standard cell A and Y are actually a part of the intersection ,we use the grid option,by pressing g on the layout window so that the grids get activated.

![image](https://user-images.githubusercontent.com/86793947/124344580-b0379a00-dbf0-11eb-9706-7676e97b94da.png)

We can also make grids from the information provided in the tracks.info file as

![image](https://user-images.githubusercontent.com/86793947/124344652-35bb4a00-dbf1-11eb-8444-f5df1186bb19.png)

Now the grid sizes have been changed according to the track definition

![image](https://user-images.githubusercontent.com/86793947/124344699-774bf500-dbf1-11eb-9bb8-9baf54bbd26f.png)

Here we can observe that the input and the output ports have both the horizontal and the vertical track intersecting so that the routes can reach from input to outputs.

![image](https://user-images.githubusercontent.com/86793947/124344745-c3973500-dbf1-11eb-88ac-86532739ac60.png)

The second condition is that the  width of the standard cell should be in odd multiples of x pitch in that layer

In this we see that there are 2 complete grids and half on each side of the boundary which makes it 3,odd multiple.

![image](https://user-images.githubusercontent.com/86793947/124344814-47512180-dbf2-11eb-8dfa-754e0493c57b.png)

When we extract the lef files ,the ports are considered as pins.

   To extract the lef file use the `lef write` command in the tkcon window
   
   ![image](https://user-images.githubusercontent.com/86793947/124345048-ef1b1f00-dbf3-11eb-898f-400e65ef33c7.png)

So this has created a lef life in the same name as the standard cell design
 
   ![image](https://user-images.githubusercontent.com/86793947/124345079-1ffb5400-dbf4-11eb-9af4-31b4a1efaf94.png)
   
A closer look at the lef file illustrates that the ports have been assigned as pins and all the changes we made in magic has been reflected in here.

    ![image](https://user-images.githubusercontent.com/86793947/124345142-a57f0400-dbf4-11eb-87bd-e5e0e9d7aaee.png)
    
 Before plugging them into the picorv32a design ,the files are being copied into the src folder under designs.
 
 ![image](https://user-images.githubusercontent.com/86793947/124345274-8c2a8780-dbf5-11eb-93ab-237e7d5fdc11.png)

 Now the first step into mapping the standard cells is the synthesis.The abc step maps the netlist to the library cells.,so a library is required which has the cell definition.This is present in the libs folder under vsdstdcelldesign
 
 ![image](https://user-images.githubusercontent.com/86793947/124345393-2ee30600-dbf6-11eb-9aff-3ed9f75148ea.png)

The library files are also moved to the src folder

![image](https://user-images.githubusercontent.com/86793947/124345583-64d4ba00-dbf7-11eb-8956-4c698f5bea70.png)

The next step is to modify the config.tcl file

![image](https://user-images.githubusercontent.com/86793947/124346001-e88fa600-dbf9-11eb-8473-5c9f4f737ab2.png)

Now the docker is invoked and the openlane flow is performed.

Overwrite previous run to include new configuration switches by using 

![image](https://user-images.githubusercontent.com/86793947/124362289-a42ff480-dc51-11eb-9d45-224660e69d0d.png)

To include the extra lef cells add the following comments

    set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
    add_lefs -src $lefs 
    
 ![image](https://user-images.githubusercontent.com/86793947/124362566-53210000-dc53-11eb-9ada-f2eef877ef31.png)
 
 ## SLACK VIOLATIONS FIXES
 
 Negative slack is not ideal to any design,so the slack values will have to be optimized and bought up to a positive value.Hence some changes could be made in the configuration directory
 
 ![image](https://user-images.githubusercontent.com/86793947/124363148-3e466b80-dc57-11eb-9fd6-9277efc257f8.png)

The implemented changes are as follows and the synthesis is run again

![image](https://user-images.githubusercontent.com/86793947/124363573-a5651f80-dc59-11eb-9628-b037fd4dda32.png)

The slack time has reduced to zero 

![image](https://user-images.githubusercontent.com/86793947/124363610-e3624380-dc59-11eb-853c-d820518f3646.png)





    
    



 
    










### TIMING MODELING USING DELAY TABLES







  
  

    










 

      
  
  






