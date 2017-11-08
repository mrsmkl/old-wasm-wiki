To be able to run larger computations, we will have to have some way to handle parallelism.
Here is one way to implement parallel computations.

First, add an instruction that runs several tasks in parallel. There has to be a block of memory that will be modified by this instruction. For each parallel task, this memory block includes a task specification, that is, task code and input files. The intended semantics of the instruction is just to (synchronously) run the tasks and collect their output
into this special block.
This instruction is similar to WebAssembly module calling other modules in JavaScript.

Verifying this instruction would have two phases:
1. Find the parallel task where the error has happened. The prover and challenger start from the root hash of the special
block, and if they disagree, the prover will post the hashes of the subtrees. The challenger will then pick one of these,
and so on, until a task where there is a disagreement is found.
2. For the task that solver and verifier disagree on, we can now run another normal verification game.
There has to be some restriction on the level of recursion for these parallel instructions.

There is one possible advantage for implementing this even without using parallelism: If the off-chain interpreter
is too slow for running a task, the task can be split into a several pieces, each of them ran with JIT, and only one of
the pieces would have to be handled with the off-chain interpreter.
