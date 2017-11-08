To be able to run larger computations, we will have to have some way to handle parallelism.
Here is one way to implement parallel computations.

First, add an instruction that runs several tasks in parallel. There has to be a block of memory that will be modified by this instruction. For each parallel tasks, that memory block includes a task specification, that is, task code and input files. 
This instruction is similar to WebAssembly module calling other modules in JavaScript.


