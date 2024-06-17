<details>
  <summary> Day 1 - Inception of EDA and PDK </summary>

  # Day 1 - Inception of EDA and PDK

  <details>
    <summary> Theory </summary>


## From Software Applications to Hardware 

- Application software (Apps) enters into system software which converts the apps into binary language to be understood by hardware.
- Major components of system software are operating system, compiler and assembler.
  
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
  <summary> Lab </summary>

  ## Open Source EDA Tools Introduction
  
1. Change the directory to `/home/vsduser/Desktop/work/tools/openlane_working_dir/openlane`.
2. The libraries are located in `/home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.ref`. We will be working with `sky130_fd_sc_hd` library.
   - `sky130_fd` is skywater foundry library
   - `sc` stands for standard cell
   - `hd` stands for high density
3. 


  
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

Following are the considerations for chip floorplan
  - Define the height and width of the core and die
  - Define the location of pre-placed cells
  - Surround the pre-placed cells with de-coupling capacitor
  
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


</details>

<details>
  <summary> Lab </summary>
</details>
  
</details>

</details>
