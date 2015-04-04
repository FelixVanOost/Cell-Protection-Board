Copyright Felix van Oost 2015.
This documentation describes Open Hardware and is licensed under the CERN OHL v1.2. You may redistribute and modify this documentation under the terms of the [CERN OHL v1.2](http://ohwr.org/cernohl). This documentation is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE. Please see the CERN OHL v1.2 for applicable conditions.

# Cell-Protection-Board
A PCB to link the cell voltage taps of eight parallel 6S batteries and protect them using fuses.

This project is developed by the [Electric Car Club](http://ubcelectriccar.com/) at the University of British Columbia, whose current goal is to break the world record for the fastest 1/4 mile in a street-legal electric car. The car, named Elektra, is a 1963 Volkswagen Beetle.

----------
Context
----------

Phase I of Elektras' battery pack consists of 30 [Turnigy 22.2V 6S 5Ah](http://www.hobbyking.com/hobbyking/store/__38515__Turnigy_Heavy_Duty_Series_5000mAh_6S_60C_Lipo_Pack.html) lithium-ion polymer batteries (originally designed for use in R/C airplanes) arranged in 5 series modules of 6 parallel batteries each. This produces a total nominal pack voltage of 111V with a peak discharge rate of around 2,400A. Phase II will increase the number of parallel batteries in each module from 6 to 8 and will increase the number of modules from 5 to 8 (resulting in a pack capable of around 3,000A peak discharge current at a nominal 177V).

The battery management system (BMS) incorporated into the car monitors the voltage of the individual cells within each battery in the pack and is capable of balancing cells with one another to maintain optimal battery health over time. This is accomplished through the connection of cell voltage taps, which are wired by the battery manufacturer between each cell in every battery. Each of the 6S batteries features 7 cell taps for monitoring and balancing purposes, which are seperate from the positive and negative leads of the battery and do not provide traction power to the car.

Since the 6 batteries in each module (Phase I) are connected together in parallel through copper bussbars, their cell voltage taps will also need to be paralleled together. In this way, the BMS can treat cell 1 of each of the 6 batteries as one large cell, followed by cell 2 and so on. The BMS will therefore only see 6 virtual cells arranged in series, regardless of how many batteries are actually connected together in parallel. One important consideration when connecting cell taps in parallel is that they provide an opportunity for excess current to flow between cells in the event that a cell in one battery fails short. This can cause cascading failures in the adjacent cells connected in that parallel group, which would cause significamt damage to the battery pack. To prevent such a failure from occuring, fuses can be placed along the cell taps to prevent excess current from flowing if a cell fails short.

Cell voltage taps play a key component in maintaing battery health over time, as they enable the individual cells to be balanced with one another periodically to accomodate for minuscule variations in chemistry and cell construction. Cell balancing is handled by the BMS, which burns off excess energy in cells with higher voltage to bring them down to the same voltage as the other cells within the battery (this is done towards the end of the charging phase). The [Orion BMS](http://www.orionbms.com/) used in Elekta is capable of sustaining a maximum cell balancing current of around 250mA. Excess current flowing through the cell taps (as in the case where a cell fails short) can damage the BMS, however, so the inputs to the unit should also be fused for protection.
