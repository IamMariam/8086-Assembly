# Microprocessors Software Project: Base Number Conversion and Mouse Control.

## Features
1. Base Number Conversion
Accepts a decimal number (signed or unsigned) entered by the user.
Converts and displays the number in the following formats:
- Binary
- Hexadecimal
- Octal

2. Mouse Interaction and UI Control
  Activates the mouse and reads the cursor position.
  Implements the following mouse interactions:
  Left Click: Changes the background color to Yellow.
  Right Click: Changes the background color to Blue.
  Double Left Click: Exits the program.

## Technologies Used
  8086 Assembly Language
  MASM/TASM (for assembling and debugging)

## How to Run
Assemble the code using an 8086 assembler (e.g., MASM or TASM).
Run the compiled program in an 8086 emulator or a real system supporting the 8086 architecture.
Follow the instructions in the program to:
Input a decimal number and view its base conversions.
Interact with the mouse to test UI controls.

## Project Breakdown
Base Conversion: The program uses arithmetic and bitwise operations to convert numbers into different bases.
Mouse Control: Mouse interrupts are used to capture clicks and manage screen colors.
Purpose

## This project demonstrates the use of:

Basic I/O operations in assembly language.
Mouse handling using hardware interrupts.
Conversion algorithms for multiple number systems.

## Notes
Ensure the program is executed in an environment that supports mouse interrupts and graphical changes.
Double left-click properly to safely exit the program.
