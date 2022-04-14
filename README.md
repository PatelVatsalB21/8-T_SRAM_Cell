# 8-T SRAM Cell
This repository presents the design of 8-T SRAM Cell implemented using eSim and SKY130 PDK on 130nm CMOS Technology.

## Table of Contents
- [Abstract](#abstract)
- [8-T SRAM Cell Working](#8-t-sram-cell-working)
- [Schematic](#schematic)
- [Simulation Results](#simulation-results)
- [Observations](#observations)
- [Limitations](#limitations)
- [References](#references)
- [Author](#author)

## Abstract
SRAM or Static Random Access Memory is a form of semiconductor memory widely used in electronics, microprocessors and general computing applications. The main concern for SRAM cell design is stability. The stability of memory is affected by the aspect ratio of MOSFET and operating conditions. Memory stability aims to operate reliably and correctly. Stability in the SRAM cell is measured by Static Noise Margin (SNM). SNM is the minimum voltage noise that can flip the state of SRAM. While reading the stored data from SRAM, the stored value should not change.

## 8-T SRAM Cell Working
In this design, two voltage sources VS1 and VS2 connected to the outputs of the bit and bit bar line, respectively. Two NMOS transistors VT1 and VT2 are connected with inputs of bit and bit bar line directly to switch ON and switch OFF the power supply source during writing “0” and write “1”
operations, respectively. These power supply sources reduce the voltage swing at the 'out' node when a
write operation is being performed. 

**A. Write '0' operation**

During the write '0' operation, the bit line goes low and the bit bar line goes high. So, the transistor VT2 goes ON and the transistor VT1 goes OFF. Thus the voltage source VS2 forces to decrease the voltage swing at the output of the bit bar line. 

**B. Write '1' operation** 

Similarly, when we perform the write '1' operation, transistor VT1 is ON and transistor VT2 goes to the OFF condition, so the voltage source VS1 decreases the voltage swing at the bit line output. Due to the decrease in voltage swing, dynamic power dissipation is almost constant even if we increase the frequency of the SRAM cell. In this SRAM model, voltage sources VSI and VS2 decrease the voltage swing during switching activity. With the increase in frequency, the switching will also increase, but the voltage source decreases its voltage swing simultaneously at the output. So at a higher frequency, the dynamic power dissipation is almost constant. These two voltage sources also provide extra voltage during the write operations on the bit line, bit bar line and word line. This extra voltage will provide a better noise margin on the bit line and word line during write operations.

## Schematic

<img src="https://github.com/PatelVatsalB21/8-T_SRAM_Cell/blob/main/Images/schematic.jpg"/>

## Simulation Results

**20ns Combined Signals**

<img src="https://github.com/PatelVatsalB21/8-T_SRAM_Cell/blob/main/Images/1.jpg"/>

**100ns Combined Signals**

<img src="https://github.com/PatelVatsalB21/8-T_SRAM_Cell/blob/main/Images/2.jpg"/>

**20ns Output Signal**

<img src="https://github.com/PatelVatsalB21/8-T_SRAM_Cell/blob/main/Images/4.jpg"/>

**100ns Output Signal**

<img src="https://github.com/PatelVatsalB21/8-T_SRAM_Cell/blob/main/Images/3.jpg"/>

## Observations
- Operating Frequency of the SRAM Cell is **100 MHz**.
- Operating Voltage of the SRAM Cell is **1.8V**.
- Write 1 voltage is **1.79V**.
- Write 0 voltage is **0.02V**.

## References
[Write stability analysis of 8-T novel SRAM cell for high speed application](https://ieeexplore.ieee.org/document/6514460)

## Author
- [Vatsal Patel](https://github.com/patelvatsalb21), Bachelor of Technology in Electronics and Communication Engineering, VGEC

