---
layout: post
title: "Review for Exam2: Pipelining (N4) and Instructioon Level Parallelism (N5)"
markdown: kramdown
date: 2021-04-04
---
##### Pipelining (N4)
a)	Example Pipeline: what IF, ID, EX, MEM, WB do  
Instruction fetch (IF)  
Instruction Decode (ID)  
Execute (EX)  
MEM (Reading/Writing Memory)  
WB (Status update in regs file)  
<img  class="img-content" alt="Zhimin Sun" width="500"  src="/assets/img/Pipeline.png">  
b)  Pipeline speedup calculation
Speedup = (execution time unpipelined)/(execution time pipelined)  
     	=(IC ×CPI<sub>unpipelined</sub>×CT)/(IC×CPI<sub>pipelined</sub>×CT)  
        =CPI<sub>unpipelined</sub>/CPI<sub>pipelined</sub>   
        =CPI<sub>unpipelined</sub>/(CPI<sub>unpipelined</sub>/N+latching)    
Now, more generally speaking, when we have N stage pipeline, speedup = N. In the reality, this is not 100 percent feasible, it works in our simplified approach where we assume that time for latching is 0.  

c)	CPI = 1 + stalls, where stalls is average stall cycles per instruction

d)	How Program Characteristics + Pipeline Hazards => Stalls  
Pipeline Hazards + program characteristics leads to stalls.  
Pipeline Hazards: May lead to a pipeline stall (some instructions have to be made in place and cannot make any further progress for some number of block cycles)
* Data hazard (RAW, WAR, WAW): Dependencies instructions pipelining overlapped execution.
* Control hazard: A branch may change PC but not until later
* Structural hazard: Not enough hardware resources for combination of instructions

e)	Structural hazards: what they are, how to avoid them  
Structural hazard: not enough hardware resources for combination of instructions.  
For example: we now have a unified I/D cache, then in some pipeline, instructions may need to access the cache at the same time, then one instruction needs to wait until the other instruction has accessed the cache.

f)	Data Hazards & Solutions
* Data dependencies (pure, anti-, output dependencies) vs. Data hazards (RAW, WAR, WAW) - Know difference between these!  
|    | Data Dependecies | Data Hazard |  
|----|:----:|:----:|  
|    |Pure/Flow Dependencies|RAW (read after write) |  
|    |Anti- Dependencies    |WAR (write after read) |  
|    |Output Dependencies   |WAW (write after write)|  
|Dependencies | Dependencies are program behavior | Hardware |  

* Pipeline forwarding (also called "register file bypass"")  
Instead of instruction only reading register value in the instruction decode stage, 
we have some mechanism of forwarding data from EX, MEM, WB to ID stage. 
Thus, the subsequent instructions can get updated value of registers. 
We propose that put a mux in the instruction decode stage, which chooses value either from register file or from EX/ MEM/ WB. 
Forwarding is also called register bypass (bypass #1 (from EX), bypass #2 (from MEM), bypass #3 (from WB)).

* When forwarding doesn't work: Load-use and delay slots  
"Load use" hazard: Assume the data cache hit is 1 cycle. We can see that in the LW instruction, R1 value is available 
after accessing MEM; while in the ADD instruction, R1 value is needed in the ID stage. Thus, there still needs stall.  
For example: LW R1, 0(R2)  
                ADD R4, R1, R5  
Feature the hazard – Load delay slot: Tell programmer load values are not available until after the next instruction.

g)	Control Hazards & Solutions 
* Issues in WHEN and WHERE
    * Delay slots, Squash slots w/likely bits
    Control hazard  
    "Featuring" the bug using Machine/Assembly language: A branch doesn’t take effect until 1 after the next instruction.  
    In computer architecture, delay slot is an instruction slot being executed without the effects of a preceding instruction.  
    For example, we fill a delay slot from branch target.  
    <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/DelaySlot.png">  
    If the branch is actually not taken, then we should not execute SUB; if the branch is taken, then it’s fine.  
    Although it looks like a good idea to fill the delay slot from the target of the branch, I now have to tell the hardware 
    whether or not I think I’m going to branch. So, I need to come up a way to inform the hardware whether or not SUB need to be squashed.  
    Squash slots with likely bits:  
    There are two kinds of branches:  
         BREQ-likely R1, H; BREQ-unlikely R1, G  
         * if likely bit = 1, squash slots when branch is not taken  
         * if likely bit = 0, squash slots when branch is taken  
    * Branch Target Buffer (BTB) & Hardware return address stack  
    Branch target buffer (BTB), instead of using tag comparison, I just took the PC. I take some bits from PC, for example 
    {12:4}, let's assume that my hash function gives me unique entry in this table.  
    There's a valid bit, if valid bit is 0, don’t branch; if valid bit is 1, use the target address.  
    <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/BranchTargetBuffer.png">  
    You can assume that the target accessed last time will be accessed again, but there is a problem in return. Where it needs to return, it depends the calling function.  
    Using return bit, if return = 1, use hardware return address stack instead of branch target buffer: on call – push, on return – pop.  
    <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/AddrReturnStack.png">  
* Issues in WHETHER
    * Software: Heuristics (eg, backward taken, forward not taken)  
    Methods to set likely bit  
        * Forward not taken, backward taken
        * Heuristic: Ball/Larus approach
        * Profiling compile -> test run -> recompile -> use
    * Hardware branch prediction
        * 1-bit (valid bit in BTB)  
        There's a valid bit in BTB, if valid bit is 0, don’t branch; if valid bit is 1, use the target address.
        * Smith 2-bit counter  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/SmithCounter.png">  
        Problem with Smith 2-bit counter: could have 100% misprediction.   
        However, some branches are correlated with other branches. If we could capture the correlation (global history register), then we can do something.
        * Gselect, Gshare  
        Gselect:
            GHR (Global history register): records the previous history (0-N, 1-T)  
            2-D array, Prediction [GHR][PC];  
            Update the counter using the actual behavior;  
            <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/GselectExample.png">
        Gshare: take a step further from Gselect  
            GHR XOR PC, 2-D array stored as a 1-D array  
            <img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/GShare.PNG">
        * Yeh/Patt  
         History Table, each entry is a shift register  
         Pattern Table, each entry is a 2-bit Smith Counter  
         <img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/Yeh-Patt.PNG">  
         <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/YehPattExample.png">
         PC->Find an entry in History Table -> Find an entry in Pattern Table  
        * Perceptron
        Not taken as -1, Taken as 1  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/Perceptron1.png">  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/Perceptron2.png">  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/PerceptronExample.png">

##### Instruction-Level Parallelism (N5)
a)	Limitations of pipelining – going beyond CPI = 1  
If we could design the pipeline really well, we could have low CT and CPI is approximately 1 (CPU Time=IC ×CPI×CT). 
However, can we achieve that CPI less than 1?  
CPI<1, which means IPC>1. For example, if we could do two instructions per cycle successfully, then we could get CPI≈0.5.  
Going back to the four levels parallelism, Gate-level parallelism, Thread-level parallelism, Instruction-level parallelism, 
Process-level parallelism. For the instruction-level parallelism, it's the same program with a single instruction stream and 
a single data stream, however, inside we’re figuring out how to execute instructions in parallel.  

b)	Architecture of superscalar processor: Fetch, Dispatch, Schedule, Execute (FUs), and State Update  
Example, CDC 6600, IBM 360/91 (later)  
                   -> Cray-1, super processor, supercomputer -> supercomputer with super processor, SUPERSCALAR; 
In general, this approach of doing more than one instruction in parallel in execution is called superscalar.  
General superscalar microarchitecture:  
<img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/SuperScalarArch.png">    
* Instruction fetch - Fetched
* Decode and Dispatch - Dispatched
* Schedule – Fired/Issued: after dispatch, we'll have a list of instructions which are ready either to execute or waiting 
for other instructions, so we need to schedule those instructions.
* Functional Units (FUs) – Completed: execute in a group of function units (units of executing functions)
* State update – Retired/Committed 

c)	Performance: retired IPC  
Retired IPC = min (Fetched IPC, Dispatched IPC, Issued (Fired) IPC, Completed IPC)

d) Dispatch and Scheduling  
* FICO algorithm (in-order issue)
    * FICO example execution  
    ```
    // [Dispatch/Scheduling Unit]
    For each Inst in Scheduling Queue do:
	    if(!Regs[Inst.Src1].Busy AND !Regs[Inst.Src2].Busy AND !Regs[Inst.Dest].Busy AND !Scoreboard[Inst.FU].Busy)
	    then //Fire Instruction
            Scoreboard[Inst.FU].Busy = True
		    Regs[Inst.Dest].Busy = True  
        Else
            exit loop // halt issuing for this cycle
    // [Execution unit at completion of Inst]
    Regs[Inst.Dest].Busy = False
    If(Inst.FU advances and first stage is free)
        Scoreboard[Inst.FU].Busy = False
    ```
    * how FICO issue enforces dependencies  
    Going back to the data dependencies, 1) Pure/Flow Dependence -> RAW, 2) Anti-dependence -> WAR 3) Output dependence->WAW  
    In FIFO, if we hit any dependency, we stop.  
    If we change the register number, then anti and output dependencies (aka False dependencies) can go away; however, the pure dependency cannot.  
    * what causes FICO to be too conservative and thus miss parallelism opportunities  
    FICO is safe but limiting.  
* FOCO algorithm (out-of-order issue)
    * Full Tomasulo algorithm
    ```
    // [Dispatch Unit]
    For all instructions I in DispQ do:
	    If(SchedQ is not full) then
		    Add I to first free slof of SchedQ (=”RS”)
		    Delete I from DispQ
		    RS.FU = I.FU
			RS.Dest = I.Dest
			for I = 0..1 do
				if (Regs[I.src[i]].Ready = True) then
					RS.Src[i].Value = Regs[I.src[i]].value
					RS.Src[i].ready = True
				else
					RS.Src[i].Tag = Regs[I.src[i]].Tag
					RS.Src[i].ready = False
			Regs[I.Dest].tag = unique tag id // tag (rename) the dest
			RS.DestRegTag = Regs[I.Dest].tag
			Regs[I.Dest].ready = False
		else exit loop // stop dispatching if scheduling queue is full
	 // [Scheduling Unit]
	for each RS = entry in SchedQueue do:
	    (a)	for I = 0..1 do:
                if (CDB.Tag = RS.Src[i].Tag) then //broadcast on CDB matches tag
	                RS.Src[i].Ready = True
	                RS.Src[i].Value = CDB.value
        (b)	if (RS.Src[0].Ready = True AND RS.Src[1].Ready = True)
                and Scoreboard[RS.FU].Busy = False //WAKEUP: if all registers are ready…
                Scoreboard[RS.FU] = True // reserve the FU and issue      
                Issue the instructions at RS on FU
     // [Execution unit, for function unit FU]
    if (FU pipeline advances and now first stage is free) then 
        Scoreboard[FU].Busy = False
     // [Execution unit, at completion of instruction I]
    if (CDB.Busy = False) then 
        CDB.Busy = True
        CDB.Tag = I.Tag
        CDB.Value = I.Value
        CBD.Reg = I.Reg
        if (I.FU is not pipelined) then
            Scoreboard[RS.FU] = False
        Delete I from Sched Queue
	 // [Register file]
	if (CDB.Busy = True and CDB.Tag = Regs[CDB.Tag]) then 
		Regs[CDB.Reg].Ready = True // Update the RF contents
		Regs[CDB.Reg].Value = CDB.Value
		CDB.Busy = False
     ```
    * FOCO example
    * The behaviors and usage of Dispatch, Scheduling, FUs, CDB, and register file
        * Instruction fetch phase will put the instructions into a queue where dispatch reads.
        * Dispatch queue is taking the instructions in the order of programmer originally intended and interpreting it and deciding whether or not to do renaming. Rename is done in the Dispatch part.
        * Schedule unit
        * Function unit
        * CDB: common data bus
        * Register file: register file is only updated with the dispatcher
    * RAT/Preg approach: how it operates  
    ```
    // [Dispatch unit]
    For all instructions I in DispQ do:
	    if (SchedQ is not full AND there are free Pregs) then
            Add I to first free slot of SchedQ (= “RS”)
            Delete I from DispQ
            RS.FU = I.FU
            for i = 0…1 do
                RS.SRC[i] = RAT[I.srci]
            RAT[I.Dest] = Find a free Preg # // rename the dest
            RS.Dest_Preg_index = RAT[I.Dest]
            Regs[RAT[I.Dest]].Ready = False
		else exit loop // stop dispatching if scheduling queue is full or there are no free Pregs
	
	// [Scheduling unit]
     for each RS = entry in SchedQueue do:
        If (Regs[RS.SRC[0]] = True AND Regs[RS.SRC[1]] = True AND Scoreboard[RS.FU].Busy = False) then
            Scoreboard[RS.FU].Busy = True // …reserve the FU and issue
            Issue the instruction at RS on FU
     [Execution unit, for function unit FU]
        (if FU pipeline advances and now first stage is free) then
            Scoreboard[FU].Busy = False
     [Execution unit, at completion of instruction I]
        if (CDB.Busy = False) then
            CDB.Busy = True
            CDB.Value = I.Value
            CDB.Preg = I.Dest_Preg
            if (I.FU is not pipelined) then
                Scoreboard[I.FU].Busy = False
            Delete I from Sched Queue
     [Register file]
        if (CDB.Busy = True) then
            Regs[CDB.Preg].Ready = True  // Update the RF contents
            Regs[CDB.Preg].Value = CDB.Value //Update the Preg
            CDB.Busy = False
    ```
* State update
    * What the problem is: imprecise interrupts  
    No consistent state
    * Reorder buffer with bypass - Solution #1: Reorder buffer with Bypass (ROB w/bypass)     
        * Add comparators to search reorder buffer for src regs    
            * The search uses source register numbers of the dispatching instruction: replaces register file lookup in basic Tomasulo  
            * Search from the newest to the oldest register file  
            * If no match, get the value from the register file  
        * Advantage: No longer have to wait until retirement for destination register to be updated  
        * Register file only stores the values, no additional meta data  
        * Problem:
            * Comparators are expensive
            * ROB now similar to fully-associative cache: a cycle time stretcher
        * Algorithm
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/TomasuloROBBypass1.png">  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/TomasuloROBBypass2.png">      
    * Future file - Solution #2: ROB w/Future File   
    <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/ROBWFutureFile.png">  
    Changes vs. ROB w/bypass are:
        * Keep the original Tomasulo register file (called “messy”)
        * Completing instructions broadcast on CDB and update the messy RF as in original Tomasulo algorithm
        * Dispatch checks “messy” register file (future file) just like original Tomasulo algorithm.
        * Before exception handling, architectural register file is copied to messy register file
    * Using the ROB to free Pregs in RAT/Preg  
    <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/FreePreg.png">  
        * When instruction reaches head of ROB, copy Preg to corresponding Areg
        * On exception, change RAT to point to corresponding Aregs
    ```
    Free Pregs via ROB
    At Dispatch of I:
        Allocate slot at tail of ROB
        ROB[tail].Prev_Preg = RAT[I.Dest]
        Then rename I.Dest
            ROB[tail]. Preg = Preg# from free list
    On instruction retirement:
        Free ROB[head].Prev_Preg
    ```
    * Checkpoint repair - Solution #3: Checkpoint repair  
        <img  class="img-content" alt="Zhimin Sun" width="400"  src="/assets/img/CheckpointRepair.png"> 
        * CDB (Common data bus): normally write to the messy register file (Tomasulo register file)
        * Messy Register File
        * Backup (backup1): CDB selectively write into backup1 on the completion of instruction based on whether 
        the instruction is before the instruction boundary.
        * Another backup (backup2)
            * If all instructions before IB (instruction boundary) complete, copy Backup to Backup2, messy to Backup1, 
            set IB2 = IB1, and pick a new IB1.
            * If an exception occurs, copy Backup2 to messy and resume at IB2 for Backup2 after exception.         

###### Attach a beautiful photo I took yesterday afternoon :)  
<img  class="img-content" alt="Zhimin Sun" width="600"  src="/assets/img/04062021Sky.jpg"> 
        


