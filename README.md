Copyright Felix van Oost 2015.
This documentation describes Open Hardware and is licensed under the CERN OHL v1.2. You may redistribute and modify this documentation under the terms of the [CERN OHL v1.2](http://ohwr.org/cernohl). This documentation is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN OHL v1.2 for applicable conditions.

# Cell-Protection-Board
A PCB to link the cell voltage taps of 8 parallel 6S batteries and protect them using fuses.

This project is developed by the [Electric Car Club](http://ubcelectriccar.com/) at the University of British Columbia, whose current goal is to break the world record for the fastest 1/4 mile in a street-legal electric car. The car, named Elektra, is a modified 1963 Volkswagen Beetle.

![Image of Cell Protection Board](https://raw.githubusercontent.com/FelixVanOost/Cell-Protection-Board/84640f141ac1f19269a229b15f1320e1e7a3f6a7/Photos%20%26%20Renderings/Cell%20Protection%20Board%201.JPG)
*Cell Protection Board - Rev. A*

----------
Context
----------

Phase I of Elektras' battery pack consists of 30 [Turnigy 22.2V 6S 5Ah](http://www.hobbyking.com/hobbyking/store/__38515__Turnigy_Heavy_Duty_Series_5000mAh_6S_60C_Lipo_Pack.html) lithium-ion polymer batteries (originally designed for use in R/C airplanes) arranged in 5 series modules of 6 parallel batteries each. This results in a total nominal pack voltage of 111V with a peak discharge current of around 2,400A. Phase II (a future upgrade) will increase the number of parallel batteries in each module from 6 to 8 and the number of modules from 5 to 8 (resulting in a pack capable of around 3,000A peak discharge current at a nominal 177V).

The battery management system (BMS) incorporated into the car monitors the voltage of the individual cells within each battery in the pack and is capable of balancing cells with one another to maintain optimal battery health over time. This is accomplished through the connection of cell voltage taps, which are wired by the battery manufacturer between the cells in every battery. Each of the 6S batteries features 7 cell taps for monitoring and balancing purposes, which are seperate from the positive and negative leads of the battery and do not provide traction power to the car.

Since the 6 batteries in each module (Phase I) are connected together in parallel through copper bussbars, their cell voltage taps will also need to be paralleled together. In this way, the BMS can treat cell 1 from each of the 6 batteries as one large cell, followed by cell 2 and so on. The BMS will therefore only see 6 virtual cells arranged in series, regardless of how many batteries are actually connected together in parallel. One important consideration when connecting cell taps in parallel is that they provide an opportunity for excess current to flow between cells in the event that a cell in one battery fails short. This can cause cascading failures in the adjacent cells connected in that parallel group, which would cause significamt damage to the battery pack. To prevent such a failure from occuring, fuses can be placed along the cell taps to prevent excess current from flowing if a cell fails short.

Cell voltage taps play a key component in maintaining battery health over time, as they enable the individual cells to be balanced with one another periodically to accomodate for minuscule variations in chemistry and cell construction. Cell balancing is handled by the BMS, which dissipates excess energy in cells with higher voltage to bring them down to the same voltage as the other cells within the battery (this is done towards the end of the charging phase). The [Orion BMS](http://www.orionbms.com/) used in Elekta is capable of sustaining a maximum cell balancing current of around 200mA. Excess current flowing through the cell taps (as in the case where a cell fails short) will damage the BMS, however, so the inputs to the unit should also be fused for protection.

The Orion BMS is also capable of monitoring temperature data from up to 80 thermistors, thereby enabling it to take appropriate action if any battery exceeds a certain temperature threshold. In Elektra, a thermistor is fixed to each Turnigy battery within the overall pack (30 for Phase I).

----------
Functionality
----------

The Cell Protection Board performs four main functions:

- Connects the cell taps of 8 Turnigy 6S batteries in parallel. The 8 slots enable the board to accomodate the future Phase II battery pack, although only 6 slots will be populated during Phase I.
- Places fuses in series with each cell tap to protect adjacent parallel cells in the event that one cell in the group fails short. The fuses are designed to limit current in each cell tap to 3A, which will prevent the cells from overdischarging and avoid fusing or burning the 22AWG cell tap wires.
- Places PTC fuses in series with each cell tap input to the BMS. These are specified to trip at 250mA (50% above the rated Orion BMS balancing current), which should adequately protect the BMS without interfering with its normal operation.
- Outputs the 8 thermistor connections (one for each of the 8 batteries) to a single connector and links the grounds of all the thermistors together. This significantly reduces the amount of wiring needed between the battery pack and the BMS, making both installation and troubleshooting far simpler.

A Cell Protection Board is required for each series module in the battery pack (5 for Phase I). The board has been designed with space-efficiency in mind, so surface-mount fuses / PTCs and double-row connectors are used to minimise its total surface area. This also greatly reduces the cost of the boards and components, but comes at the expense of replaceability (a blown fuse will need to be desoldered and another one resoldered in its place). All surface-mount components are 3216 (metric) size, which provides a good balance between space-efficiency and ease of replacement. Nonetheless, the fastest and most efficient way to assemble the board is by reflow soldering, which produces consistent and excellent quality joints (a solder paste stencil file is included in the Gerber Files folder). Molex Micro-Grid connectors are used throughout the design due to their high current rating, compact footprint, and sturdy locking mechanism.

The Cell Protection Boards are designed to be modular - they can be easily modified to accomodate more or fewer batteries with a varying number of cells. Their functionality is also quite generic, enabling them to be used with almost any BMS solution or lithium-ion cell chemistry. Mounting holes are placed throughout the board to allow vertical stacking using standoffs.
