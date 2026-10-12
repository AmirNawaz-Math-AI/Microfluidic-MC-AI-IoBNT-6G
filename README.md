# Microfluidic-MC-AI-IoBNT-6G
AI-Assisted Encoding Decoding for Microfluidic Molecular Communication - CPiFHSS-LLM - TU Dresden Project
-----------------------------------
Post-MPhil Research Work for Research Associate Position - TU Dresden

Applicant: Amir Nawaz
Position Applied: Research Associate - TU Dresden DFG Project Ref 10001-1003638856-S
Research Group Leader: Dr. Pit Hofmann
Date: 11 Oct 2026
GitHub: https://github.com/AmirNawaz-Math-AI/Microfluidic-MC-AI-IoBNT-6G

### What is this project?

We send text message Hello Dresden through microfluidic channel. Text is converted to bits and sent through fluid. Due to diffusion in fluid, errors happen. This project corrects those errors using AI.

This work is based on TU Dresden DFG Project by Dr. Pit Hofmann.

### Parameters used

Diffusion coefficient D is 0.1 taken from Hofmann paper Figure 3. Paper says range 0.08 to 0.12 so average 0.1 is taken. Adjusted for SPU lab conditions.

Flow velocity v is 0.5 taken from Hofmann Figure 2.

Channel distances tested are 1mm to 5mm. 10mm was also tried but error was too high 60 percent.

### Error model

Raw error BER increases with distance from 15 percent to 44 percent. This matches Hofmann Figure 4 non-Gaussian noise model.

Simple LLM correction reduces error by 70 percent.

Proposed CPiFHSS-LLM correction reduces error by 75 to 80 percent. This is my contribution based on Complex Picture Fuzzy Hypersoft Set theory with complex phase term for fluid delay.

### Results

At 1mm distance, error improves from 15 percent to 2.8 percent.
At 5mm distance, error improves from 44 percent to about 12 percent.
Average improvement is 75 percent plus.

### Files uploaded

Main code file for simulation.
Results file generated after running code.
Chart image showing BER vs WER improvement.

### Future work for Research Associate role

Integration with IoBNT and 6G and FPGA implementation for microfluidic chip and real lab testing.

### Contact
Amir Nawaz
Applicant for TU Dresden Research Associate application Ref 10001-1003638856-S
