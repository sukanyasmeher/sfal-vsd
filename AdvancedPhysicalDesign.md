<details>
  <summary> Day 1 - Inception of EDA and PDK </summary>

  # Day 1 - Inception of EDA and PDK

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

### Usage in the Design Flow

PDKs are used throughout the IC design flow, from initial schematic capture and simulation through physical layout and verification. They are essential for both custom IC designs and designs using standard cells.

  
</details>
