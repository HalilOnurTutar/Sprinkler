# PLC-Based Sprinkler Control System (KOM3772 Experiment)

## Overview

This project implements a control system for a garden sprinkler using a Programmable Logic Controller (PLC). The system runs three distinct, sequential irrigation programs with adjustable repetition counts. It features a custom Function Block (`My_sprinkler`) to handle the timed energizing and stopping cycles for each program phase.

This project was developed as part of the Industrial Automation Systems Lab I (KOM3772) coursework at the Department of Control and Automation Engineering, Yıldız Technical University.

## System Functionality

The system operates a single sprinkler through three sequential programs:

1.  **Program 1:** Energizes the sprinkler for 2 seconds, then stops for 1 second. This cycle repeats 'X' times.
2.  **Program 2:** Energizes the sprinkler for 3 seconds, then stops for 1 second. This cycle repeats 'Y' times.
3.  **Program 3:** Energizes the sprinkler for 2 seconds, then stops for 3 seconds. This cycle repeats 'Z' times.

An operator can adjust the repetition counts (X, Y, Z) before starting the sequence:
* **Button 1 (DI 1):** Selects the variable to adjust (Cycles through X -> Y -> Z -> X...).
* **Button 2 (DI 2):** Increments the currently selected variable (X, Y, or Z) by one.
* **Button 3 (DI 3):** Resets all variables (X, Y, Z) to their initial value of 1.

Once the variables are set, the operator can press the **Start Button (DI 4)** to begin the entire irrigation sequence (Program 1 -> Program 2 -> Program 3).

A **Process Lamp (DO 1)** indicates when the system is actively running any part of the sequence. It turns off once Program 3 completes its repetitions. The **Sprinkler (DO 2)** is activated according to the current program's timing.

## Key Component: `My_sprinkler` Function Block

A reusable Function Block named `My_sprinkler` was created to manage the core timing and repetition logic for each program phase.

* **Inputs:**
    * `Start` (BOOL): Initiates the block's operation for one program phase.
    * `Energizing_Period` (TIME): Duration for the sprinkler to be ON.
    * `Stopping_Period` (TIME): Duration for the sprinkler to be OFF between energizing cycles.
    * `Repeat_number` (INT): Number of times the ON/OFF cycle should repeat (corresponds to X, Y, or Z).
* **Outputs:**
    * `Done` (BOOL): Becomes TRUE when the block has completed all its repetitions.
    * `Busy` (BOOL): Is TRUE while the block is actively running (not yet Done).
    * `Sprinkler` (BOOL): The output signal to activate the physical sprinkler valve.

Three instances of this block (`My_sprinkler_4`, `My_sprinkler_5`, `My_sprinkler_6` in the main logic) are likely used, one for each program phase, configured with the respective timing and repetition inputs.

## Software and Hardware

* **Software:** Developed using **Phoenix Contact PC Worx version 6.30.3146**.
* **Hardware:** Designed for a generic PLC system capable of handling digital inputs and outputs.
    * **Inputs:** 4 Digital Inputs (Buttons)
    * **Outputs:** 3 Digital Outputs (Lamp, Sprinkler,Done)

## How to Use (Simulated/Physical)

1.  **Load Project:** Load the project onto a compatible PLC using the appropriate Phoenix Contact software.
2.  **Set Repetitions:** Use Button 1, Button 2, and Button 3 to set the desired values for X, Y, and Z (number of repeats for Program 1, 2, and 3 respectively).
3.  **Start Sequence:** Press the Start Button (DI 4).
4.  **Monitor:** Observe the Process Lamp (DO 1) and Sprinkler (DO 2) outputs as the sequence progresses through the three programs. The system stops and the lamp turns off after Program 3 completes.

## Repository Contents

* **`Sprinkler.pdf`**: This PDF file contains exported views of the project logic, including the main program (Main POU) and the `mysprinkler` Function Block, likely including Ladder Logic/FBD diagrams and variable lists.
* **`README.md`**: This file, providing an overview of the project.


## Author

* Halil Onur Tutar ```
