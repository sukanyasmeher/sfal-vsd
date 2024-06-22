<details>
  <summary> Day 1 - Inception of EDA and PDK </summary>

  # Day 1 - Inception of EDA and PDK

  <details>
    <summary> Theory </summary>


## From Software Applications to Hardware 

- Application software (Apps) enters into system software which converts the apps into binary language to be understood by hardware.
- Major components of system software are operating system, compiler, and assembler.
  
<img width="811" alt="1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/d4ed9a15-c832-425f-8912-366cfc5ee863">

<img width="818" alt="2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2d06cb1b-4fff-4c71-b708-92ea05ce945a">

## SOC Design using OpenLane

### What is Process Design Kit (PDK)?
A Process Design Kit (PDK) is an essential set of documents and data files used in the design of integrated circuits (ICs). It is provided by semiconductor foundries to IC designers to ensure that their designs are manufacturable using the foundry's process technology. Here are the key components and aspects of a PDK:

1. **Design Rules**: Detailed guidelines for the physical layout of the IC. These rules ensure that the design can be reliably manufactured and meet the desired performance criteria.

2. **Device Models**: Mathematical models that describe the behavior of the transistors and other components fabricated with the specific process technology. These models are crucial for accurate circuit simulations.

3. **Technology Files**: Information about the layers and materials used in the process, such as metal layers, dielectric materials, and doping concentrations.

4. **Standard Cell Libraries**: Pre-designed and pre-characterized logic gates, flip-flops, and other fundamental building blocks. These cells are optimized for the specific process technology and are used to speed up the design process.

5. **Parameter Files**: Data for setting up simulation tools and ensuring that the simulations reflect the real-world performance of the fabricated IC.

6. **Design Verification Files**: Scripts and settings for design rule checking (DRC), layout versus schematic (LVS) checking, and other verification processes to ensure that the design meets all manufacturing requirements.

7. **Process-Specific Scripts and Tools**: Automated tools and scripts tailored for the specific process technology, which help streamline the design and verification process.

#### Importance of PDKs

- **Manufacturability**: Ensures that the designed ICs can be reliably fabricated.
- **Performance Optimization**: Helps designers optimize their designs for performance, power, and area.
- **Efficiency**: Speeds up the design process by providing pre-characterized components and automated tools.
- **Accuracy**: Improves the accuracy of simulations, leading to better predictions of the final IC performance.

#### Usage in the Design Flow

PDKs are used throughout the IC design flow, from initial schematic capture and simulation through physical layout and verification. They are essential for both custom IC designs and designs using standard cells.

### Simplified RTL-to-GDS

<img width="1150" alt="3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b5b84f28-f3f0-492d-9d15-248fd5005be8">

<img width="1145" alt="4" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b5b5fad4-aa6c-47f0-86bd-f58adc3eab60">

<img width="1133" alt="5" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9000684b-1c0b-492a-9f43-b34d26c7dcf1">

<img width="1133" alt="6" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/96915a87-3a80-4780-8262-d73963f4f005">

<img width="1131" alt="7" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/43391ede-6889-44f3-a227-4f56970d836d">

<img width="1129" alt="8" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/21edccc1-e419-461a-a063-ce35518f8d10">

<img width="1132" alt="9" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/711df054-dce3-4279-bbfc-2e5b6ec62467">

<img width="1130" alt="10" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/eed690fe-9de9-45fe-903f-89f5b5388c44">

<img width="1133" alt="11" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c33b4c48-5e6d-4a2d-b46b-fbf362e622c1">

<img width="1132" alt="12" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2a04ebf9-5209-42f6-bf6d-1b5348e3a323">

### OpenLane ASIC Flow

<img width="1132" alt="13" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6fa1e1a1-58e4-4fd4-ba6f-82eaf4fce854">

</details>

<details>
  <summary> Lab - Download VirtualBox in MAC OS </summary>
  
</details>

<details>
  <summary> Lab - Introduction to Open Source EDA Tool - OpenLane </summary>

## Introduction to Open Source EDA Tools  - OpenLane

Refer the link for more information - https://github.com/The-OpenROAD-Project/OpenLane
  
1. The libraries are located in `/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.ref`. We will be working with `sky130_fd_sc_hd` library.
   - `sky130_fd` is skywater foundry library
   - `sc` stands for standard cell
   - `hd` stands for high density
2. Change the directory to `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane`.
3. set alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u
4. Invoke OpenLane using `docker` command
5. Run OpenLane in interactive mode using the command `./flow.tcl -interactive`. Without `-interactive` it will run the complete flow. But at this stage, we want to do step-by-step.
   
The screenshot after invoking is shown below

![14](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/efbb9fea-0829-42d4-bdf4-300426efdda8)

6. Command for input required package for openlane flow is
```
package require openlane 0.9
```
7. The designs are already located in the folder `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs`. We will run the design `picorv32a`. The settings are present in config.tcl. However, the precedence Opnelane takes is openlane setting < config.tcl < sky130A_sky130_fd_sc_hd_config.tcl. Command to prepare the file system and design setup is
```
prep -design picorv32a
```
A new directory called `runs` is created inside `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a`. It has all the necessary files and folder required for synthesis.  

The screenshot is shown below
![15](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/b65ebf6a-3903-4302-bcbb-15494810972f)

8. The command to run the synthesis is
   ```
   run_synthesis
   ```
   After successful synthesis, the output is shown below.
   
![16](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/73d4c00b-72d5-46bf-8330-073a184316db)

9. Next task is to find the ***flop ratio*** which is ratio of number of D flip flops and the total number of standard cells which is 1613/14876=0.10843.
 ![17](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9bd1c07a-4af6-49e6-980e-959ef7192dec)

10. Synthesis reports are present in `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-06_00-09/reports/synthesis`. The reports are
```
    total 1736
-rw-r--r-- 1 vsduser vsduser   1216 Jun 18 06:18 1-yosys_pre.stat
-rw-r--r-- 1 vsduser vsduser    866 Jun 18 06:18 1-yosys_dff.stat
-rw-r--r-- 1 vsduser vsduser  20479 Jun 18 06:19 1-yosys_4.chk.rpt
-rw-r--r-- 1 vsduser vsduser   2674 Jun 18 06:19 1-yosys_4.stat.rpt
-rw-r--r-- 1 vsduser vsduser     12 Jun 18 06:19 2-opensta_tns.rpt
-rw-r--r-- 1 vsduser vsduser     11 Jun 18 06:19 2-opensta_wns.rpt
-rw-r--r-- 1 vsduser vsduser 816771 Jun 18 06:19 2-opensta.timing.rpt
-rw-r--r-- 1 vsduser vsduser  17763 Jun 18 06:19 2-opensta.min_max.rpt
-rw-r--r-- 1 vsduser vsduser 816771 Jun 18 06:19 2-opensta.rpt
-rw-r--r-- 1 vsduser vsduser  74793 Jun 18 06:19 2-opensta.slew.rpt
```
  
</details>

</details>

<details> 
  <summary> Day 2 - Good floorplan vs bad floorplan and introduction to library cells </summary>

# Day 2 - Good floorplan vs bad floorplan and introduction to library cells

<details>
  <summary> 1 - Chip Floorplan Considerations </summary>
  
  # 1 - Chip Floorplan Considerations
  
<details>
  <summary> Theory </summary>

Following steps are considered for chip floorplan
  - Define the height and width of the core and die
  - Define the location of pre-placed cells
  - Surround the pre-placed cells with de-coupling capacitor
  - Power planning
  - Pin placement and logical cell placement blockage
    
## Utilization Factor and Aspect Ratio

Steps to define the height and width of the core and die 
- Define the netlist which is the connectivity between all the components
- Convert the symbols of gates into the physical dimension
- Find out the dimensions of standard cells (not wires as of now). Let's assume the rough dimensions of standard cells as 1unit X 1unit. Thus area is 1 sq. unit.
- With the help of this information, next, we can calculate the area occupied by the netlist on a silicon wafer. The total are occupied by this netlist will be no. of cells X 1sq. unit.
- Utilization Ration = Area occupied by netlist/Total area of the core
- Aspect Ratio = Height/ Width

<img width="1201" alt="1" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/41a290f0-da05-4dfd-a6d3-bc9bfb8f81c7">

<img width="1175" alt="2" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/04b8fadc-c5c7-4735-b685-85afcac71706">

## Concept of pre-placed cells

Steps to define the location of pre-placed cells 
- Define the combinational operation in terms of gates
- Cut them into parts, in this case may be 2 parts or 2 separate blocks. Each block will be implemented separately.
- Extend the IO pins and black box the two blocks
- Separate the black boxes as separate IPs or modules. The advantage of this is these IPs or modules can be used multiple times in the chip as required. This is the concept of re-used modules. Similarly there are other IPs available such as memory, clock gating cell, comparator, mux.
- The arrangement of these IPs in a chip is called ***Floorplanning***.
- These IPs/blocks have user-defined locations, hence are placed in chip before automated P&R and are called ***pre-placed cells***.
- Automated P&R tools place the remaining logical cells in the design onto the chip.

<img width="1168" alt="3" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c6a7ba53-56cb-4d73-9c6b-62a3f1cecc8c">

<img width="1152" alt="4" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/3451e244-a469-418f-ba8a-1f7f2f6fc682">

<img width="1129" alt="5" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2dd000f4-fdbf-4f1a-a481-f35b01b0d459">


## De-coupling capacitor

Decoupling capacitors, also known as bypass capacitors, are critical components in VLSI (Very-Large-Scale Integration) design for several reasons:

- Power Supply Stabilization: VLSI circuits can draw significant and rapidly changing amounts of current, especially during switching operations. These sudden changes can cause fluctuations in the power supply voltage. Decoupling capacitors help stabilize the voltage by providing or absorbing current as needed, ensuring a steady supply to the circuit.

- Noise Reduction: High-speed switching in VLSI circuits generates noise, which can propagate through the power supply lines and affect the performance of other parts of the chip. Decoupling capacitors filter out this high-frequency noise, reducing its impact on sensitive components.

- Signal Integrity: Variations in the power supply can lead to signal integrity issues, causing errors in data transmission and processing. Decoupling capacitors maintain a consistent voltage level, helping to preserve the integrity of signals within the chip.

- Transient Response Improvement: When a circuit suddenly switches states, the demand for current can spike. Without decoupling capacitors, the inductance and resistance in the power delivery network can prevent the power supply from responding quickly enough, leading to voltage dips. Decoupling capacitors provide the necessary current during these transitions, improving the transient response.

- Prevention of Ground Bounce and Supply Droop: Ground bounce occurs when multiple outputs switch simultaneously, causing a temporary rise in ground voltage. Similarly, supply droop happens when the supply voltage drops due to a sudden increase in current demand. Decoupling capacitors mitigate these effects by providing a local reservoir of charge.

- Reduction of Electromagnetic Interference (EMI): Switching noise can radiate as electromagnetic interference, affecting nearby circuits and systems. Decoupling capacitors help in suppressing this noise, reducing EMI.

  <img width="948" alt="6" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e3553809-72a1-4a45-812d-44464929489e">

## Power Planning

Disadvantage of single power supply is shown below

<img width="1113" alt="10" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4534bd64-c36d-469d-b1e2-2b89c282af42">

<img width="1169" alt="7" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c5b2753b-da61-49f5-9c5e-77539ed5ba13">

<img width="1135" alt="8" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4908e38f-f418-4d49-9d3f-0f8a5a3751dc">

<img width="1118" alt="9" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f04a3c2e-fdd9-4fdc-b893-b2f1183aa104">

Solution is to have a power and ground supply mesh so that circuit could tap from the nearest source

<img width="1094" alt="11" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/5a3e8427-fb17-466d-9e9c-0d7786d5edbf">

<img width="1169" alt="12" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9d16f26c-cd1b-40ad-a4bb-62b6a9fe5515">

## Pin Placement and Logical Cell Placement Blockage

Let's consider the design as shown below.

<img width="1135" alt="13" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/09d878a6-e0c6-45af-9cf1-32452859bc01">

Steps for pin placement and logical cell placement blockage
- The pins are placed based on where the cells are placed. Goal is to keep the pins closer.
- Clock input ports are bigger in size than the data ports. As the clock ports continuously sends signal to all the flips flops, it needs a least resistance path for clocks. And bigger the size, the lower the resistance.
- Similarly for clock output ports as we need clock signals to move out o fthe chip as fast as possible because the clock is driven continuously.
- We need to make sure that the automated P&R doesn't place any cell in the pin placement area. For this, we place the logical cell placement blockage.
- floorplan is ready for placement and routing step.

The screenshot shows how the design looks after pin placement and logical cell placement blockage.

<img width="1196" alt="14" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/729955ce-555b-4213-91c1-a77de7b67614">

<img width="1203" alt="15" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/98cfb6c8-1e33-4657-a716-51ac46208e4f">


</details>

<details>
  <summary> Lab - Floorplan using OpenLane </summary>
  
# Lab - Floorplan using OpenLane

Steps for the floorplan
1. The variables or switches for the OpenLane design flow are mentioned `README.md` inside the directory `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/configuration/floorplan.tcl` as shown below.
 
   ![1](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ea0c3e2f-d491-4189-bcfb-a4b41b4117fa)

2. The parameters for floorplan are set in `floorplan.tcl` in the same directory as highlighted below.
   
   ![2](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/8451a90a-e6fe-467f-a410-36db31379ab9)

3. However, the precedence Opnelane takes is openlane setting < config.tcl < sky130A_sky130_fd_sc_hd_config.tcl. A screenshot of config.tcl and sky130A_sky130_fd_sc_hd_config.tcl is shown below.
   
  ***config.tcl***
  ![7](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/49de2ce8-7581-4c06-b5c7-30cdcd69b3a9)


***sky130A_sky130_fd_sc_hd_config.tcl***   
![4](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/911b689a-60a0-4521-91e6-6cccbd291c88)

4. The command to run floorplan is `run_floorplan`and the successful completion is shown below.
   
   ![3](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/429a6363-9711-4e78-ba63-7c01e37922a5)

5. Check the floorplan by opening `picorv32a.floorplan.def` inside the directory `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-06_00-09/results/floorplan`. Screenshot of picorv32a.floorplan.def is shown below.
   
   ![5](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/baf99883-95b8-474d-8bcc-6a211dc944c3)

6. Calculate the die area  
   1um = 1000 unit distance
   
   Die width = 660685/1000 = 660.685um
   
   Die height = 671405/1000 = 671.405um
   
   Area = width x height = 660.685 x 671.405 = 443587.212 um<sup>2<sup>
   
7. Next we load the generated floorplan.def in Magic tool and exploring it. Change directory to folder containing floorplan.def and then load floorplan.def in magic tool with the command
```
cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/18-06_00-09/results/floorplan
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
The screenshot shows the layout of floorplan in Magic.
![6](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0126182f-715b-4d91-bf07-491b5072621c)

8. Next we review the floorplan layout in Magic.
- Input and output pins are placed almost equidistant
  
  ![8](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/8a326ece-5692-4059-a024-61b213475534)

- Identify the metal of the pin by placing the cursor and type 's'. Then `tkcon` window type 'what` which shows the metal layer of the pin or port as shown below.
    ![9](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/923a61d9-ee6e-417b-a5d1-798d23b883c9)
  
- From the above figure, you can see that the decap cell locations are at the end of the row as they are set as endcap in config.tcl.
  
- Tap cells are equidistant from the diagonal tap cells as shown below. Tap cells connect the substrate (or wells) to a fixed potential, typically the power supply (VDD) or ground (VSS). This is necessary to prevent floating substrate or well potentials, which can lead to latch-up conditions or leakage currents.
  ![10](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/9016550b-5df6-4e69-95d3-1a406c63e766)

- The floorplan doesn't take into consideration the placement of standard cells. But standard cells are present here at the origin.
  ![11](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/05efd79c-d147-4c5c-8b1e-2f187419971e)

- If changes are needed for the design, it is always prefered to make it at `sky130A_sky130_fd_sc_hd_config.tcl` level.
  
</details>
  
</details>

<details>
  <summary> 2 - Library Binding and Placement </summary>

  # 2 - Library Binding and Placement

  <details>
  <summary> Theory </summary>

  The following steps are considered for library binding and placement
  - Bind netlist with physical cells
  - Placement of the cells on the floorplan
  - Optimize placement using estimated wire length and capacitance
  
  ## Netlist binding and initial place design

Steps for netlist binding and initial place design
- Give the cells physical dimensions (width and height). The cells are present in a library which includes physical information, timing information such as delay information, and required condition of the cells. The library has different flavors of each cell.
- Place the netlist onto the floorplan. the netlsit contains connectivity information of the design.
- Placement makes sure that the pre-placed locations are not affected. There will be no cells placed in these locations.
- Place the FF cells closer to the IO pins and combinational cells close to FF. This way we can maintain the timing requirements. Sometimes the cells are abutted which is a good example of high frequency circuits.

<img width="1198" alt="16" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/97c2f68f-8841-4525-b686-1667d8835429">

<img width="1209" alt="17" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/33e076e1-9c1c-4680-8677-5178942d1cfa">

<img width="1205" alt="18" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/efd7fe1d-a74d-432b-8bc0-d516f44c7ec4">

## Optimize placement using estimated wire length and capacitance

Steps to optimize placement using estimated wire length and capacitance
- We estimate the wire length and capacitance and insert repeaters (buffers) based on that. If the wire length is longer, both the capacitance and resistance increase. This way signal integrity is maintained. This tradeoff with the area.
- No repeater is inserted if the wire length and capacitance are not large.
- Since there are no clocks yet, verify if the data path is correct considering the ideal clock such as setup timing analysis. Hold timing analysis is irrelevant without a clock.

  <img width="1206" alt="19" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/150f3124-0560-4706-96c4-cb02315aa5fa">

</details>

<details>
  <summary> Lab - Congestion aware placement using RePlAce </summary>

  ## Congestion-aware placement using RePlAce
  - Placement happens in two stages
      - Global placement - Main objective is reduce the wire length. In OpenLane we used half half-perimeter wire length (HPWL)
      - Detailed placement
  - Standard cells are placed in rows, abutted with each other and there should be no overlap. this is called legalization, which is important for timing point.
  - Global placement - We need to converge overflow. As the overflow value decreases, the design is converged.

1. Run the congestion aware placement using the command `run_placement` the result is shown below
   ![12](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1b96b9dd-9c0c-414f-83ab-7eeb83408a44)

2. Open generated placement.def in magic tool using the following commands
    ```
    cd /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/06-04_16-22/results/placement
    magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def & 
    ```
    The placement of cells in placement.def are shown below
    ![13](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1c38d4fe-1feb-42eb-b21b-ab9beb7a5301)

3. If we zoom in, we can see the standard cells are correctly places on the rows and not overlaping each other.
   ![14](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c327919d-4fb4-4be0-9d54-37b60554a7f7)

4. Power and ground are usually created during floorplan. But in OpenLane it is created during CTS. 

</details>

</details>

<details>
  <summary> 3 - Cell Design and Characterization Flow </summary>

  # 3 - Cell Design and Characterization Flow
  <details>
    <summary> Theory </summary>

  Cell design flow involves 3 steps
  - Inputs
  - Design steps
  - Outputs
    
  ## Inputs for cell design flow

  Inputs for cell design flow are 
  - Process design kit (PDK) including DRC and LVS rules
  - Spice models
  - Library and user-defined specs like cell height, width, supply voltage,metal layers, pin locations, drawn gate length etc

Cell height is defined by the separation between the power and ground rails.  
Cell width is defined by the drive strength of the cell.
  
<img width="1136" alt="20" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/85001ef0-6f8a-4aed-8f54-ee130bb23ac1">

<img width="1152" alt="21" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/881d06d2-2d8a-4e4b-bbe8-8f9b0076705b">

<img width="1155" alt="22" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f622f6c9-9603-40a9-8233-0f93ca2f2e04">

<img width="1154" alt="23" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/08aebbd2-a9a4-4043-93a3-8c3008c675c9">

## Design steps for cell design flow

Design steps for cell design flow are
- Circuit design
- Layout design
- Characterization

Circuit design involves 2 steps:
1) Implement the functionality itself using CMOS or other technology
2) Model the PMOS and NMOS in such a fashion to meet library requirements
   
Both of them are based on spice simulation. The output we get from circuit design is called ***circuit description language*** or CDL.

<img width="1143" alt="24" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0ea739e4-c582-472a-8d13-f87c3031288e">

Layout design involves the following steps:
1) Get the function implemented through MOS transistors or PMOS and NMOS connections
2) Get the NMOS and PMOS network graph
3) Obtain the Euler's path - path traced only once
4) Draw the stick diagram
5) Convert the stick diagram to layout adhering to layout rules (DRC) from the foundry
6) Extract the parasitics and characterize it in terms of timing

The output of layout design is GDSII, LEF and extracted spice netlist (.cir) which is after parasitic extraction.

 <img width="1161" alt="25" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f1f3d7f2-3555-41b3-af66-c8d9b23d6bfd">

Characterization flow involves the following steps:
1) Read in the models for PMOS and NMOS
2) Read the extracted spice netlist
3) Recognize the behavior of buffer (2 inverters in series)
4) Read the sub-circuit of inverter
5) Attach the necessary power sources
6) Apply the stimulus
7) Provide the necessary output capacitance or load
8) Provide the necessary simulation command (like .tran, .dc)

Characterization software is called ***GUNA*** which takes the input from step 1 to 8 in a configuration file. Characterization is further divided into timing characterization, power characterization, and noise characterization. 
The output of characterization is timing, noise, power .libs, function.

<img width="1184" alt="26" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/7511ebeb-22c5-4071-8742-4f083d036dad">

  </details>
  
</details>

<details>
  <summary> 4 - General Timing Characterization Parameters </summary>
  
# 4 - General Timing Characterization Parameters

<details>
  <summary> Theory </summary>

  ## Timing threshold definations
  Different timing threshold definations are 
  - slew_low_rise_thr -  calculate the slope or slew of the particular waveform at the lower side towards 0. Typical value is 20% of VDD.
  - slew_high_rise_thr - calculate the slope or slew of the particular waveform at the higher side towards VDD. Typical value is 80% of VDD.
  - slew_low_fall_thr - Typical value is 20%
  - slew_high_fall_thr - Typical value is 80%
  - in_rise_thr - Typical value is 50%
  - in_fall_thr - Typical value is 50%
  - out_rise_thr - Typical value is 50%
  - out_fall_thr - Typical value is 50%

    <img width="1151" alt="27" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/043ed329-5ec8-4c47-99c8-e53cdb145d9d">

    ## Propagation delay
    Propagation delay = time(out_x_thr)-time(in_x_thr)
    
    <img width="1154" alt="28" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/2550b95b-c2d3-4bac-8c6a-f276e6cda715">

    ## Transition time
    <img width="1127" alt="29" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/4a83b775-1140-4945-9c62-031215aa2ddf">

  
</details>
  
</details>

</details>

<details>
  <summary> Day 3 - Design library cell using Magic Layout and ngspice characterization </summary>

  # Day 3 - Design library cell using Magic Layout and ngspice characterization

<details> 
    <summary> 1- Labs for CMOS inverter ngspice simulations </summary>

  # 1- Labs for CMOS inverter ngspice simulations

<details>
  <summary> Theory </summary>

## CMOS Inverter SPICE deck
![1](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/f10dd884-43a6-4f5c-971e-94b6a6414b7f)

![2](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/e916c6d7-efc0-4e06-b88d-dc0301a25fe3)

![3](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/ed41ad48-fa61-456c-9a1f-ff2eb0982370)

![4](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/83c51aa1-4cf8-4492-8868-02c0ddc43a6c)

Parameters that define the robustness of CMOS inverter
- Switching threshold, Vm - It is a point at which Vin = Vout (or Vgs = Vds). It is the point where both NMOS and PMOS are ON or in saturation region leading to leakage current. The screenshots below shows the Vm for two different sizes of inverters.

![5](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/1ee941c6-3d6e-4ac5-b3ff-eab0234330ca)

![6](https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/99c413d9-3d48-4e5a-a90b-9df4bc2aa9c3)

</details>

<details>
  <summary> Lab </summary>

Steps to open the CMOS Inverter layout in Magic  
1. Change the directory to OpenLane directory  where the lab will be done `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane`.
2. Clone the repository https://github.com/nickson-jose/vsdstdcelldesign which has custom inverter design into the directory
   ```
   git clone https://github.com/nickson-jose/vsdstdcelldesign
   ```
3. Change into working repository directory with the command `cd vsdstdcelldesign`.
4. Copy magic tech file to the repo directory for easy access and which will be used to open the layout. The command is
   ```
   cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
   ```
 5. Command to open custom inverter layout in magic
    ```
    magic -T sky130A.tech sky130_inv.mag &
    ```
    The layout of CMOS inverter is shown below.
    <img width="1255" alt="7" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/fc261c69-1857-4978-a410-7e5ef7f8af87">

</details>
    
</details>

<details>
  <summary> 2 - Inception of Layout Â CMOS fabrication process </summary>

  # 2 - Inception of Layout Â CMOS fabrication process
<details>
  <summary> Theory </summary>
  
</details>

<details>
  <summary> Lab -  Introduction to Sky130 basic layers layout and LEF using inverter </summary>

  Steps to identify various layers in the CMOS inverter layout
  
  1. To identify NMOS, keep the mouse pointer around the n-diffusion layer (green) and press `s`. In `tkcon` type `what`. It outputs NMOS pointing to the device in the layout as shown below.
     <img width="1262" alt="8" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/64ac35ba-8460-4aab-8097-24aed040fc05">
     
  2. To identify PMOS, keep the mouse pointer around the p-device layer (red) and press `s`. In `tkcon` type `what`. It outputs PMOS pointing to the device in the layout as shown below.
     <img width="1260" alt="9" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/beea843c-efcc-445c-b1f0-303d440863e7">
     
  3. According to the definition, source of PMOS should be connected to VDD and source of NMOS should be connected to GND. For PMOS source connectivity check, keep the cursor on the source contact of PMOS and press `s` three times. The connection between PMOS source and VDD is highlighted as shown below.
     <img width="1257" alt="10" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/51eedaa9-9964-4136-a17b-018a4e1a63f3">

  4. For NMOS source connectivity check, keep the cursor on the source contact of NMOS and press `s` two times. The connection between NMOS source and GND is highlighted as shown below.
     <img width="1258" alt="11" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/c2a1f51f-1fb1-4c64-91c8-0320c70c49c0">
     
 The inverter from scratch is described in https://github.com/nickson-jose/vsdstdcelldesign
  
</details>

<details>
    <summary>  Lab - Steps to create standard cell layout and extract spice netlist </summary>
    

Steps to verify the DRC errors and extract the spice netlist

1. Magic Tool is an interactive DRC platform. Select the design by typing `s` and move your mouse to create a box. Press `Control+D` to delete the layer and you will see DRC error shown below. You can see DRC reports in `tkcon` window.
   
   <img width="1257" alt="12" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/0ffaef68-1a9f-4a4d-bfc9-8f1c1a59485b">

2. Next we extract the spice netlist. For this, type the following command in `tkcon` window to extract sky130_inv into sky130_inv.ext in present working directory. 
   ```
   extract all
   ```
   The sky130_inv.ext file is shown below
   <img width="1046" alt="13" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/6126b232-f2f5-4768-a265-250efc649563">
   
3.  Command to enable the parasitic extraction with resistors and capacitors before converting ext to spice. No new file is created.
    ```
    ext2spice cthresh 0 rthresh 0
    ```
4. Command to convert .ext file to .spice file
   ```
   ext2spice
   ```
   The sky130_inv.spice file is shown below
   <img width="1606" alt="14" src="https://github.com/sukanyasmeher/sfal-vsd/assets/166566124/37cdf165-d0d0-4abf-847f-b7a7e6aef067">


</details>

</details>
  
</details>


