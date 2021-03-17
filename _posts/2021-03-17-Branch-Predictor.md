---
layout: post
title: "Branch Predictor"
markdown: kramdown
date: 2021-03-17
---

This blog is to record my project2 in Advanced/High Performance Computer Architecture.

##### Introduction
Stalls due to control flow hazards and the presence of instructions, such as, function calls, direct and in-direct jump, returns, etc. that modify
the control flow of a program, are detrimental to a pipelined processor's performance.To mitigate these stalls, various branch prediction techniques are used.
In this project, I implemented various branch predictors, a two level Global History Branch predictor (GShare), a two level Local History predictor (Yeh-Patt),
and finally a perceptron branch predictor. I models a Pattern table (Smith Counters), or perceptron table, and a global history register or history table depending on the 
predictor being simulated.

##### The GShare Predictor
The architecture of the GShare branch predictor
<img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/GShare.PNG">
* The Global History Register is a G bits wide shift register. The most recent branch is stored in the least-significant-bit of GHR, and a value of 1 denootes a taken branch. The initial value of the GHR is 0.
* The Pattern Table (PT) contains 2<sup>G</sup> smith counters. The Predictor XORs the GHR with bits [2+G-1:2] of the branch PC to index into the PHT.
* Each Smith Counter in the PHT is 2-bits wide and initialized to 0b01, the Weakly-NOT-TAKEN state.  
<b>In summary</b>,
* GHR <b>XOR</b> [2+G-1:2] of branch PC = index of PT, then using the index, we could find the prediction state in the PT.
* After getting the branch prediction,we should shift the Global History Registor and update the Smith Counter.
    * If actual branch behavior is TAKEN, the Smith Counter should be updated (00->01, 01->10, 10->11, 11->11)
    * If actual branch behavior is NOT TAKEN, the Smith Counter should be updated (11->10, 10->01, 01->00, 00->00)  


##### The Yeh-Patt Predictor
The architecture of the Yeh-Patt branch Predictor
<img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/Yeh-Patt.PNG">
* The History Table (HT) contains 2<sup>G</sup>, P-bit wide shift registers. The most recent branch is stored in the least-significant-bit of these
history register, and a value of 1 denotes a taken branch. All entries of the history table are set to 0 at the start of the simulation.
The predictor uses bits [2+G-1:2] of the branch address (PC) to index into the History Table. 
* The Pattern Table (PT) ccontains 2<sup>P</sup> smith counters. Each Smith Counter in the PT is 2-bits wide and initialized to 0b01, the Weakly-NOT-TAKEN state.
The value read from the History Table is used to index into the pattern table.  
<b>In summary</b>,
* [2+G-1:2] of the branch address = index into the History Table  
The value read from the History Table = index into the pattern table
* After getting the prediction from Pattern Table, we should update shift register in History Table and the Smith Counter in Pattern Table.

##### Perceptron Predictor
The architecture of the perceptron predictor
<img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/Perceptron.PNG">
The perceptron based branch predictor was one of the first forays of neutral networks (in their simplest form) in the world of branch prediction. Unlike Smith counter based predictors,
perceptron branch predictors are able to make use of long branch histories and despite being more complex than alternatives can be implemented within moderate bit-budgets.
* The Global History Register is G bits wide.
* The perceptron table can hold 2<sup>P</sup> perceptrons. Each perceptron has G+1 weights, which can have a value in the range [-(\theta+1),\theta], where $\theta=1.93 \times G + 14$.  
Using bits [(2+P-1):2] of branch address to index into perceptron table.  
<b>In summary</b>,  
y = w<sub>0</sub> \times x<sub>0</sub> + w<sub>1</sub> \times x<sub>1</sub> + ... + w<sub>G</sub> \times x<sub>G</sub>
If sign(y) != t or abs(s) <= theta, we should update each w<sub>i</sub> by w<sub>i</sub> = w<sub>i</sub> + t * x[i],
and then shift the global history register.

###### Reference  
Project2 in CS6290 Advanced/High performance computing architecture

