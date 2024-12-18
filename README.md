## Table of Contents
***
1. [General Info](#general-info)
2. [Design Requirements](#design-requirements) 
   - [Boolean Logic](#boolean-logic)
   - [Flip Flops](#flip-flops)
   - [Power](#power)
   - [Silk Screen](#silk-screen)
   - [Dimensions](#dimensions)
3. [Detailed Schematic Info](#detailed-schematic-info)
   - [Input Power](#input-power)
      - [USB-C Connector](#usb-c-connector)
      - [5V to 3.3V Linear Regulator](#linear-regulator)
      - [On/Off Switch](#power-switch)
      - [Battery Considerations](#battery-considerations)
   - [Buttons](#buttons)
   - [LEDs](#leds)
   - [Logic Gates](#logic-gates)
      - [NOT Gate](#not-gate)
      - [AND Gate](#and-gate)
      - [OR Gate](#or-gate)
      - [NAND Gate](#nand-gate)
      - [NOR Gate](#nor-gate)
      - [XOR Gate](#xor-gate)
      - [XNOR Gate](#xnor-gate)
   - [Flip Flops and Latches](#flip-flops-and-latches)
      - [D Flip Flop](#d-flip-flop)
      - [JK Flip Flop](#jk-flip-flop)
      - [T Flip Flop](#t-flip-flop)
      - [SR Latch](#sr-latch)
   - [Adjustable Clock Generator](#adjustable-clock-generator)
   - [State Machine](#state-machine)
   - [Synchronous 3-Bit 0-5 Counter](#synchronous-3-bit-0-5-counter)
   - [Multiplexers and 16-Segment Display](#multiplexers-and-16-segment-display)
4. [KiCAD](#kicad)
   - [Schematic Design](#schematic-design)
   - [PCB Layout](#pcb-layout)
      - [Images](#images)
5. [Possible Improvements](#possible-improvements)
6. [Acknowledgements](#acknowledgements)

## General Info
***
This is a quick-turn promotional PCB made as an intern project for Gentex Corporation. It showcases digital logic concepts in an interactive format, and will be a useful reference for students studying digital electronics.

## Design Requirements
***
#### Boolean Logic
* The design shall include the following Boolean logic gates:
   - AND 
    - OR 
    - XOR 
    - NOR 
    - NAND 
    - NOT 
    - XNOR 
* The design shall electrically demonstrate each logic gate. 
* The design shall include a silk screen that captures all of the logic gate truth tables. 
* The design shall include 2 inputs for each logic gate. 
#### Flip Flops
* The design shall implement a minimum of one (1) flip-flop. 
* The design shall electrically demonstrate all implemented flip-flops. 
* The design shall include a silk screen that captures each implemented flip-flop truth table. 
#### Power
* The design shall operate on 3.0V power. 
* The design shall be powered from either a coin cell battery, or external power supply. 
#### Silk Screen
* The design shall implement the Gentex logo on the silk screen layer. 
* The design shall implement the designer's college on the silk screen layer. 
#### Dimensions
* The design shall be implemented on a 3”x5” PCB. 
## [Detailed Schematic Info](https://github.com/vandjac/Logic-PCB/files/12321933/Logic-PCB-Schematic.pdf)
***
### Input Power
***
According to the project requirements, the board had to operate at 3V. When choosing input power, that left us with a couple options. Either we could use power from a USB input, or we could use a coin cell battery. Each had their advantages, but we decided on using USB power only. Although the coin cell battery allows for the board to be portable, it didn’t have enough capacity or current sourcing capabilities to be worth the addition.

We did a power analysis on our design to determine the max current that would theoretically be drawn. With 17 LEDs pulling 5-10mA, a max of 8 segments of the 16-digit display being on at 10mA, and the linear regulator at 45mA, our max current draw was around 200-300mA.

#### USB-C Connector
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/fac413d8-90ed-4475-9481-2f84083173c6)

This USB-C connector is power only, which means it has fewer pins than a standard USB-C jack. This makes the pads bigger and easier to solder onto. The pins that are included are 2 VBUS, 2 GND, and 2 CC pins. The CC pins are used to regulate the current draw from the power source. In order to activate the power source, we have to add a 5.1k resistor to each CC pin. The USB-C should be able to supply 5V at up to 500mA, which should be enough for all functions on the board.

#### Linear Regulator
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/0c6cdb1f-0df8-4552-9353-6802618042cd)

The chosen linear regulator can convert 5V to 3.3V with a max output current of 400mA. Another consideration for the regulator is heat dissipation, which could be problematic at max output current. The calculation for determining the temperature of the part is as follows:

Rthja×voltage drop×current draw

With the SOT-223 package, the temperature comes out to (87)(1.7)(0.4)=59.16°C

With the DPAK package, the temperature comes out to (72.5)(1.7)(0.4)=49.3°C

The industry standard is that anything over 60°C is too hot to touch, so the DPAK package is more appropriate for this case. This package is larger than the SOT-223.

The circuit design was copied from the datasheet for the linear regulator. The datasheet provides different configurations depending on the desired output voltage. A .1u bypass capacitor is also added to the output to smooth out voltage spikes.

#### Power Switch
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/321562ea-7db3-4942-a291-9a9638e96d23)

The main consideration for the switch was how much current it could handle, since the entire power for the board was being routed through the switch. For the JS203011JAQN switch being used, the datasheet gives a contact rating of 0.3A@6VDC. Since we’re using 3V power, we should be able to get more than 0.3A through it, if needed.

#### Battery Considerations
![Battery](https://github.com/vandjac/Logic-PCB/assets/112498063/1bb644f6-d80b-4d52-8c8b-e09e0c4af3ad)



In the event that someone would want to add a battery to the board, we have kept the schematics for the battery in the design, but commented out. If a battery were to be added in conjunction with USB power, it would be necessary to ensure that the two power sources don’t fight. To do so, Schottky diodes are placed on the 3V outputs of the battery holder and the linear regulator. These diodes have a low voltage drop (0.3V) and block current from flowing back into the battery or regulator. 

In reality, the Schottky diode on the regulator wouldn’t be doing much. This is because the regulator outputs a constant 3.3V, while the battery will always be at 3V or less. Therefore, current would only try to flow from the regulator to the battery. However, it is good practice to have Schottky diodes placed on both sources.

### Buttons
***
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/e1341ef1-85ba-4026-b075-3e8bf363d91d)

Originally, we were planning on having individual buttons for each logic gate and flip flop. We quickly realized that would be a waste of space. Now, we have two buttons (labeled A and B) that control all the logic gates and flip flops on the board. 

Each button has a pulldown resistor and two capacitors. The pulldown is there to make sure the button line is held low when the button is not pressed. The 1u capacitor helps to debounce the button, and the .1u capacitor is a bypass capacitor to eliminate voltage spikes. 

As for the specific button chosen, we simply found one in a small 6x6mm package that Gentex had in stock.

### LEDs
***
Each LED is set up to run at a max of 10mA. The LEDs have a voltage drop of 2V. 3.3V is taken from the pins on the ICs or from VCC, depending on the circuit. Therefore, we are left with V=IR, where V=1.3, I=13mA, and R=100Ω. That can easily be changed to V=1.3, I=6.5mA, and R=200Ω with a change of resistors.

Because we have the LEDs sinking current from ICs, we had to ensure that each IC was capable of sinking 10mA of current. The ICs we chose have that capability.

### Logic Gates
***
Logic gates are fundamental building blocks of digital circuits and are used to perform logical operations. According to the project requirements, the common 7 logic gates: AND, OR, NOT, XOR, NOR, XNOR, NAND. This was implemented with the use of NPN, PNP, P-channel, N-channel CMOS transistors, LEDs, resistors, and buttons A & B.

#### NOT Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/413dc26d-6ead-47a2-91d5-774586f1adc1)

The NOT gate, also known as an inverter, is a logical component that takes a single input signal and generates an output signal that is the inverse of the input. Specifically, when the input signal is at a high state (1), the output signal is at a low state (0), and conversely, when the input is low (0), the output is high (1). To achieve this functionality, a circuit was designed in which the power flows through a resistor and an LED connected to the collector of a PNP transistor. Initially, when the board is powered on, the LED is lit, indicating a high (1) output as button A remains untouched, signifying a low (0) input. However, when button A is pressed, causing a high (1) input, power is directed through the base of the PNP transistor, resulting in the LED being turned off, thereby representing a low (0) output state.

Truth Table:

| Input | Output |
|:---------:|:---------:|
| 0 | 1 |
| 1 | 0 |

#### AND Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/84e4db1c-6655-483b-ad4f-7f6e7c6963c8)

The AND gate is a logic circuit that takes two or more input signals and generates a single output signal. The output signal is set to low (0) whenever any of the input signals are low (0). However, if all the input signals are high (1), the output will be high (1) as well. 

To implement this logic, the AND gate circuit utilizes two NPN transistors connected in series in the collector-emitter configuration with a resistor and an LED connected to the emitter. Initially, when the circuit is powered on, the LED remains off, indicating a low (0) output because neither button A nor B has been pressed, and no power flows through the transistors to activate the LED. Since the transistors are in series, pressing only one button (making it high) while the other remains low (0) continues to prevent power from reaching the LED, keeping the output low (0). However, when both buttons are pressed (making them high), power flows through both transistors, causing the LED to turn on/high (1) and indicating a high output.

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### OR Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/e98d617d-541e-4ea9-abe4-f58bfbd2135c)

The OR gate is a logic gate that takes two or more input signals and produces a single output signal. The output signal is set to high (1) when any of the input signals is high (1). However, if all input signals are low (0), the output will be low (0) as well.

To implement this logic, the OR gate circuit utilizes two NPN transistors connected in parallel, with their collectors and emitters linked together. Additionally, a resistor and an LED are connected to the emitters. When the circuit is powered on, the LED remains off, indicating a low (0) output state. This occurs because neither button A nor B has been pressed, and therefore no power is flowing through either transistor to activate the LED.

When only one of the input buttons is pressed, while the other remains unpressed, the power will still flow through the corresponding transistor to activate the LED, resulting in a high (1) output. This is due to the parallel configuration of the transistors, which allows the output to remain high (1) even when both buttons are pressed and both transistors are conducting.

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

#### NAND Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/486c85ad-8147-4d23-8cda-4bd7ad2da94d)

The NAND gate is a logical combination of an AND gate followed by a NOT gate (inverter). It accepts two or more input signals and produces an inverted output, which remains high (1) unless all the inputs are high (1).

To implement this logic, the NAND gate circuit utilizes two P-channel CMOS transistors connected in parallel, with their collectors and emitters linked together. Additionally, a resistor and an LED are connected to the emitters. There are also two N-channel CMOS transistors connected in series, with their collectors connected to the resistor and LED. Buttons A and B are respectively connected to the base of one P-channel and one N-channel transistor.

When the circuit is powered on, the LED will be initially on, representing a high (1) output. This occurs because neither button A nor B has been pressed, causing power to flow through both P-channel transistors and activating the LED. If only one of the input buttons is pressed, while the other remains unpressed, the power will still reach the LED, activating it, and resulting in a high (1) output. This is due to the parallel configuration of the P-channel transistors.

However, when both buttons are pressed, causing both P-channel transistors to turn off (low), the LED will be turned off (low) as well. This is because of the parallel configuration of the transistors, causing the output to turn off (low) in this scenario. The opposite behavior would occur for the N-channel transistors, but since the P-channel transistors are off (low), the LED remains off (low).

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 1 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### NOR Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/964539a5-ef98-4ffa-8053-5e44857d5d1d)

The NOR gate is a logical combination of an OR gate followed by a NOT gate (inverter). It accepts two or more input signals and produces an inverted output, which remains low (0) only when all inputs are high (1).

To implement this logic, the NOR gate circuit utilizes two P-channel CMOS transistors connected in series, in the collector-emitter configuration. Additionally, a resistor and an LED are connected to the emitter. There are also two N-channel CMOS transistors connected in parallel, with their collectors linked to the resistor and LED. Buttons A and B are respectively connected to the base of one P-channel and one N-channel transistor.

When the circuit is powered on, the LED will be initially on, representing a high (1) output. This happens because neither button A nor B has been pressed, allowing power to flow through both P-channel transistors, activating the LED. However, if only one of the input buttons is pressed, while the other remains unpressed, the power will not reach the LED, preventing it from activating. As a result, the output will be off/low (0). This behavior is due to the series configuration of the P-channel transistors.

Likewise, when both buttons are pressed, causing both N-channel transistors to turn off/low (0), the LED will be turned off/low (0) as well. This occurs because of the parallel configuration of the transistors, resulting in the output remaining off/low (0) in this situation. The opposite behavior would occur for the N-channel transistors, but since the P-channel transistor is off/low (0), the LED remains off/low (0).

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 0 |

#### XOR Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/f211c83e-812a-48e9-b6a4-45d827629204)

The XOR gate, a fundamental logical combination of basic gates, operates on two input signals and generates a singular output signal with a high (1) state when the inputs are dissimilar (one is high while the other is low). Conversely, if both inputs are identical (both high or both low), the output assumes a low (0) state. The implementation of this gate involved the utilization of a chip sourced from Diodes Inc.. The selection of this particular component was based on its ability to meet the criteria for current and voltage requirements, as well as its compact size and cost-effectiveness.

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### XNOR Gate
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/1b4ab5db-c9f2-49e7-a1a2-beb917b83df4)

The XNOR gate, a logical combination of an XOR gate followed by a NOT gate (inverter), serves to process two input signals and yield an output signal that assumes a high state (1) exclusively when the inputs are identical (both high or both low). Conversely, if the inputs are dissimilar, the output assumes a low state (0). The practical realization of this functionality involved the adoption of a chip sourced from the company Nexperia. This was selected for its ability to fulfill the necessary criteria concerning current and voltage requirements, as well as the chip’s ability to meet the criteria for current and voltage requirements, as well as its compact size and cost-effectiveness.

Truth Table:

| Input A | Input B | Output |
|:---------:|:---------:|:---------:|
| 0 | 0 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

### Flip Flops and Latches
***
Latches and flip-flops are fundamental building blocks in digital logic circuits used to store and synchronize data.

A latch is a simple digital storage element that can hold one bit of data. It is based on a bistable circuit, which means it has two stable states. Latches are level-sensitive and can be sensitive to glitches and timing issues. They are mainly used in asynchronous circuits and for temporary storage.

A flip-flop is a more advanced digital storage element that can also hold one bit of data. It is also based on a bistable circuit but operates with a clock signal, making it edge-triggered. Since flip-flops are edge-triggered they are more robust against glitches and can be easily synchronized in sequential circuits, making them the preferred choice for most synchronous digital systems.

There is a .1u bypass capacitor on each VCC pin to eliminate voltage spikes for each flip flop. 

#### D Flip Flop
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/11b35e56-6467-41bf-936d-cd9b7f2c31b7)

The D flip-flop has a data input (D), a clock input (usually called CLK or CP), and an output (Q). On the rising (or falling) edge of the clock signal, the D flip-flop stores the value present at the data input. The stored value is then available at the output (Q). D flip-flops are widely used for sequential circuits and in synchronous systems.

The D flip flop circuit has one button input (D) and a CLK input. It has inverted set and reset pins that are both tied to VCC to prevent them from activating. Both Q and !Q drive LEDs.

Truth Table:

| D | Q | Qt |
|:---------:|:---------:|:---------:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

#### JK Flip Flop
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/0d8c3958-39d0-4839-8774-688f8fe7f242)

The JK flip-flop has two data inputs: J and K, along with a clock input and an output. The behavior of the JK flip-flop is similar to the D flip-flop, but it has the additional feature of allowing the toggling of the output (Q) when both J and K inputs are active simultaneously.

The JK flip flop circuit has two button inputs (1J and 1K) and a CLK input. The IC being used is a dual JK flip flop, but only one of them is being used, so half of the pins aren’t being used. An inverted reset pin is tied to VCC to prevent it from activating. Both Q and !Q drive LEDs.

It is also a negative edge triggered flip flop, meaning it becomes active when the clock signal goes from high to low, and ignores the low- to-high transition. This means it will be activated on the button release instead of the press.

Truth Table:

| J| K | Qt | Q(t+1) |
|:---------:|:---------:|:---------:|:---------:|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 1 |
| 1 | 1 | 1 | 0 |

#### T Flip Flop
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/a534568d-8de1-4699-88bf-dc92bf8c4cb0)D

The T flip-flop has a single input (T) and a clock input (usually called CLK or CP), along with an output (Q). On each rising (or falling) edge of the clock signal, the T flip-flop toggles its output (Q) based on the value at the T input.

The implementation of the T flip flop uses the same chip as the JK flip flop. A T flip flop can be built from a JK flip flop by tying together the two inputs. The T flip flop is also on a different part of the board, because it is used in the counter (which is discussed more below). Since it is being used in the counter it also doesn’t use button inputs. To display the input, an LED is placed on the combined J&K input. Although it’s not displayed in this screenshot, both Q and !Q also drive LEDs.

Truth Table:

| T | Q | Qt |
|:---------:|:---------:|:---------:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

#### SR Latch
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/a8ae56bd-a585-451e-9971-9d5f49ecfd3a)

The SR latch has two inputs: Set (S) and Reset (R). When the Set input is asserted (usually logic high), the latch stores the value '1'. Conversely, when the Reset input is asserted (usually logic high), the latch stores the value '0'. If both inputs are active simultaneously, the behavior is unpredictable, and this condition should be avoided.

This is the only latch shown on the board. It is also implemented differently from the flip flops. Since latches are simpler, the SR could be built using two NOR gates instead of a single IC. This allows the structure and function of an SR latch to be better understood. In the SR latch configuration, each NOR gate has a button input. Each NOR gate output is tied to the second input of the other NOR gate. The outputs also drive LEDs.

Truth Table:

| S | R | Qt | Q(t+1) |
|:---------:|:---------:|:---------:|:---------:|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | X |
| 1 | 1 | 1 | X |

### Adjustable Clock Generator
***
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/174a7156-1697-4eab-aa45-f8567eb84b13)

It was necessary to add a timer to produce a clock signal that’s used by the flip flops. The timer circuit is mostly taken from its datasheet (the circuit for A-stable operation). The component values were derived from equations found on the datasheet. We chose RA to be 1kΩ and C to be 1u. That leaves RB as the value to change to affect the frequency of the clock. 

Period = 0.693(RA+2RB)C

Frequency = 1.44/((RA+2RB)C)

Using these equations we can find a potentiometer that can vary the frequency in a usable range. We settled on a 10Ω to 2MΩ potentiometer, which allows for a frequency sweep of .001s to 1.387s. 

The potentiometer was placed in series with another resistor (R32, 1kΩ) so it doesn’t effectively short when the potentiometer is at its lowest resistance.

Two extra capacitors were added as well. A 0.1uF is used for decoupling on power input to the chip. Another decoupling cap is placed from CONT to GND improves operation by reducing noise on the rising edge of the clock signal. This info is also found in the datasheet.

Finally, an LED is placed on the clock output so the frequency can be visualized.

### State Machine
***
The state machine is the most complex circuit on this board. It is intended to display the use of flip flops to switch between different states. The state machine being implemented here is a Moore state machine, which means its next state is only determined by its current state.

#### Synchronous 3-Bit 0-5 Counter
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/b1c76cd2-5208-4575-bece-abd172360f79)
![Sim](https://github.com/vandjac/Logic-PCB/assets/32146550/2f60fed3-4a9d-49d5-a8c6-77cf9f5cd16d)


To build a counter, first you need to create a state transition table. This table includes the current state, the corresponding next state, and whether that bit needs to toggle or not. 

This table is then used to create Karnaugh maps for each toggle bit. The Karnaugh maps are filled in with values from the toggle bit portion of the state transition table, in relation to the current state.

From the Karnaugh maps we got simplified expressions for each toggle bit, which will be used for each T flip flop. The T flip flops are implemented with the logic from the expressions. For example, if T1=!AC, then T flip flop 1 will accept an input from the inverse of the output of T flip flop 0 ANDed with the output of T flip flop 2. 

The chips being used all have decoupling capacitors, and the input and output of each T flip flop are visualized using LEDs.

#### Multiplexers and 16 Segment Display
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/da5f7f6b-4345-4ba2-b87a-13a45edd3354)

The output of the counter will need to be translated into letters in the word “GENTEX”. To do so, we used multiplexers. These muxes take the counter as their select bits, and then each of the inputs are tied to either 3V or GND, depending on whether a segment needs to be turned on or not.

Although there are 16 segments in the display, only 9 muxes are needed. This is because some of the segments always turn on and off at the same times in the letters of GENTEX, so a single mux can drive both segments.

In order to determine which mux inputs to tie high and low, we created a table. The table includes the counter input (with bits a, b, and c) and a column for each segment in the display. The table is then filled in with a 1 if the segment is on at that counter value, and 0 if it is not. Identical columns are colored as such. For example, segments A and B have a sequence of 110110. Therefore, the mux that drives A and B should have inputs 1, 2, 4, and 5 tied to 3V. Inputs 3 and 6 should be tied to GND. 

The outputs of each mux are capable of sourcing at least 10mA of current. A current limiting resistor is placed between each output and LED segment.

## KiCAD
***
#### [Schematic Design](https://github.com/vandjac/Logic-PCB/files/12321933/Logic-PCB-Schematic.pdf)

When designing the schematics to the circuits on the PCB, an important step was to find the components needed as well as assigning the footprints and values to them. This involved researching for parts and then finding the selected part online and importing it to KiCAD. This process was streamlined by using a tool called Samacsys KiCAD Libraries. Using this tool, it was possible to search the part needed, see if it had a footprint, datasheet, etc., and import it to the KiCAD library all with a couple clicks. There was only one footprint that was not found, the power USB-C, and so a handmade footprint was made using KiCAD based on the datasheet of the component. Boxes were also drawn around circuits that were in some way related as to clarify sections of the board because the point of the board is to explain how things work.

To tie the schematic to the QR code, and because this project is meant to be used as a teaching tool, a QR code was placed on the layout that links to the GitHub Repository. This not only provides an explanation for steps taken in the process of the making of the PCB, but also provides the schematic and other useful images and explanations.

#### [PCB Layout](#images)
0.15mm traces were used throughout the board. This was the chosen diameter because there is not a lot of current flowing through the board so wide traces are not as necessary. This also ensured that all the connections could be made as the 8-1 MUXs were small QFNs which would not be possible if the traces were much larger. All of the components were designed to be on the “top” side of the board to ensure it would lay flat, the only through hole component is the 16-seg display. As few vias as possible were used, however there were a lot around the MUXs due to the nature of the chosen chips.

Silkscreen was used heavily in the layout of the PCB. Artificial silkscreen “traces” were drawn from the buttons A & B as well as the clock to the outlines of their respective recipient symbols. These symbols were made by converting an image to a footprint, and then putting the footprint as the silkscreen layer. The same was done for the front and back of the board using the built-in KiCAD image converter tool. The logos were done in a similar fashion, except that they were converted to the mask layer instead of the silkscreen layer.

#### Images
***
#### KiCAD 3D view (Front)
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/6af5a9b0-1959-4d38-9557-de27337e6bf9)

#### KiCAD 3D view (Back)
![image](https://github.com/vandjac/Logic-PCB/assets/112498063/8ce8f2b0-a93a-4a7c-9e44-735fe6157a7e)

#### KiCAD Layout (Front)
![Logic_PCB_Layout_Front](https://github.com/vandjac/Logic-PCB/assets/112498063/872cb232-a996-44bb-8e81-4936c8487704)

#### KiCAD Layout (Back)
![Logic_PCB_Layout_Back](https://github.com/vandjac/Logic-PCB/assets/112498063/4d350d2a-441e-4b2e-8cc3-bf096f38c540)

#### KiCAD Routing (Front)
![Logic_PCB_Routing_Front](https://github.com/vandjac/Logic-PCB/assets/112498063/fa1784a8-3cb0-48cd-b7cc-edefc921f07b)

#### KiCAD Routing (Back)
![Logic_PCB_Routing_Back](https://github.com/vandjac/Logic-PCB/assets/112498063/62cb1fa2-5691-485e-aab1-5d8e6c63ac1f)

## Possible Improvements
***
- The switch currently being used is a right-angle switch, pointed in a direction on the board that makes it somewhat difficult to access. The switch should either be moved to the side of the board, or a vertical switch should be used.
- As discussed in the “Battery Considerations” section, a battery could be added to improve portability of the board.
- If a battery is added, thought could be put into whether making it rechargeable through USB-C or a solar panel would be worthwhile.
- It would entail a complete redesign of the board, but implementing everything on a small microcontroller could allow you to reduce the dimensions of the board or add many more features to the board in the same dimensions.

## Acknowledgements
***
Thanks to Tom Case and Nicholas Young for defining this project and doing a thorough design review with us.
