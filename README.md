# 8-T SRAM Cell
This repository presents the design of 8-T SRAM Cell implemented using eSim and SKY130 PDK on 130nm CMOS Technology.

## Table of Contents
- [Abstract](#abstract)
- [8-T SRAM Cell Working](#8-t-sram-cell-working)
- [Schematic](#schematic)
- [Netlist](#netlist)
- [Simulation Results](#simulation-results)
- [Observations](#observations)
- [Limitations](#limitations)
- [Steps to run this project](#steps-to-run-this-project)
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

## Netlist
### SKY130 Netlist
```
* c:\users\vatsal\8t_sram.cir
.lib "sky130_fd_pr/models/sky130.lib.spice" tt 


xm5 gnd writtenbit net-_m3-pad2_ gnd sky130_fd_pr__nfet_01v8
xm3 net-_m3-pad1_ net-_m3-pad2_ writtenbit net-_m3-pad1_ sky130_fd_pr__pfet_01v8
xm7 net-_m3-pad2_ writeenable bitbar gnd sky130_fd_pr__nfet_01v8
xm8 net-_m3-pad2_ bitbar net-_m8-pad3_ gnd sky130_fd_pr__nfet_01v8
xm4 gnd net-_m3-pad2_ writtenbit gnd sky130_fd_pr__nfet_01v8
xm1 writtenbit bitline net-_m1-pad3_ gnd sky130_fd_pr__nfet_01v8
xm2 writtenbit writeenable bitline gnd sky130_fd_pr__nfet_01v8
xm6 net-_m3-pad1_ writtenbit net-_m3-pad2_ net-_m3-pad1_ sky130_fd_pr__pfet_01v8
v5  net-_m8-pad3_ gnd 1.8
v2  writeenable gnd 1.8
v4  net-_m3-pad1_ gnd 1.8
v3  net-_m1-pad3_ gnd 1.8
* u1  writtenbit plot_v1
* u3  writeenable plot_v1
* u2  bitline plot_v1
* u4  bitbar plot_v1
v1  bitline gnd pulse(0v 1.8v 0u 0u 0u 10n 20n)
v6  bitbar gnd pulse(1.8v 0v 0u 0u 0u 10n 20n)
.tran 250e-12 20e-09 0e-06

* Control Statements 
.control
run
print allv > plot_data_v.txt
print alli > plot_data_i.txt
plot v(writtenbit)
plot v(writeenable)
plot v(bitline)
plot v(bitbar)
.endc
.end
```

### ESim Netlist
```
* c:\users\vatsal\esim-workspace\8t_sram\8t_sram.cir

.include PMOS-180nm.lib
.include NMOS-180nm.lib
m5 gnd writtenbit net-_m3-pad2_ gnd CMOSN W=100u L=100u M=1
m3 net-_m3-pad1_ net-_m3-pad2_ writtenbit net-_m3-pad1_ CMOSP W=100u L=100u M=1
m7 net-_m3-pad2_ writeenable bitbar gnd CMOSN W=100u L=100u M=1
m8 net-_m3-pad2_ bitbar net-_m8-pad3_ gnd CMOSN W=100u L=100u M=1
m4 gnd net-_m3-pad2_ writtenbit gnd CMOSN W=100u L=100u M=1
m1 writtenbit bitline net-_m1-pad3_ gnd CMOSN W=100u L=100u M=1
m2 writtenbit writeenable bitline gnd CMOSN W=100u L=100u M=1
m6 net-_m3-pad1_ writtenbit net-_m3-pad2_ net-_m3-pad1_ CMOSP W=100u L=100u M=1
v5  net-_m8-pad3_ gnd 1.8
v2  writeenable gnd 1.8
v4  net-_m3-pad1_ gnd 1.8
v3  net-_m1-pad3_ gnd 1.8
* u1  writtenbit plot_v1
* u3  writeenable plot_v1
* u2  bitline plot_v1
* u4  bitbar plot_v1
v1  bitline gnd pulse(0 1.8 0 0 0 10n 20n)
v6  bitbar gnd pulse(1.8 0 0 0 0 10n 20n)
.tran 250e-12 20e-09 0e-06

* Control Statements 
.control
run
print allv > plot_data_v.txt
print alli > plot_data_i.txt
plot v(writtenbit)
plot v(writeenable)
plot v(bitline)
plot v(bitbar)
.endc
.end
```

## Observations
- Operating Frequency of the SRAM Cell is **100 MHz**.
- Operating Voltage of the SRAM Cell is **1.8V**.
- Write 1 voltage is **1.79V**.
- Write 0 voltage is **0.02V**.

## Limitations
- Frequency of the SRAM Cell cannot be increased much higher as it results it more dynamic power dissipation, uneven duty cycle and low write voltages.
- Area of the circuit cannot be reduced further.

## Steps to run this project
1. Open a new terminal
2. Clone this project using the following command:</br>
```git clone https://github.com/PatelVatsalB21/8-T_SRAM_Cell.git ```</br>
3. Change directory:</br>
```cd Project Files```</br>
4. Run ngspice:</br>
```ngspice SKY130_8T_SRAM.cir```</br>
5. To run the project in eSim:

  - Run eSim</br>
  - Load the project</br>
  - Open eeSchema</br>


## References
[Write stability analysis of 8-T novel SRAM cell for high speed application](https://ieeexplore.ieee.org/document/6514460)

## Author
- [Vatsal Patel](https://github.com/patelvatsalb21), Bachelor of Technology in Electronics and Communication Engineering, VGEC

