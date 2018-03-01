## Simple benchmark

This is just calculating factorial of 12345678.

Results
* Reference interpreter: 25 s
* ocaml-offchain: 5.3 s
* wasmi: 4.8 s
* binaryen: 3.3 s
* wabt: 2.2 s
* native C: 0.03 s factorial 1234567890 2.6 s
* node.js JIT: 0.09 s factorial 1234567890 2.6 s

## Gas metering

To implement the metering in a way that it doesn't make everything 10x slower, I propose the following:
* at the beginning of each function, add gas usage of all instructions in the function
* at the beginning of each loop iteration, add gas usage of all instructions inside the loop
* loops can be ignored when calculating the gas usage of a function

## Critical path

Let's have a coarse measure of number of steps: Each function call is a step, and each loop iteration is a step.
Perhaps also exiting from a function could be this kind of step.
Prover and verifier have to find the first step for which they disagree.
The _critical path_ for a step in execution include the function calls in the stack needed for the step, and for each loop that is in the stack, the loop iteration.

For example:
```
function asd() {
   return 123;
}
function bsd() {
   return 123+asd();
}
function main() {
   for (int i = 0; i < 10; i++) bsd();
}
```

We have steps
1. main
2. main, for.0
3. main, for.0, bsd
4. main, for.0, bsd, asd
5. main, for.0, bsd
6. main, for.0
7. main, for.1
8. main, for.1, bsd
9. main, for.1, bsd, asd
10. main, for.1, bsd
11. main, for.1
12. ...

The critical path for step 9 is
 * function call `main` at step 1
 * for loop 2nd iteration at step 7
 * function call `bsd` at step 8
 * function call `asd` at step 9. We call this last part of the critical path the _critical block_.

The idea is that to construct the stack, we only need to handle the critical path, and the critical path should be a small part of the execution (at least in normal programs). 

## Constructing the stack

For each function or loop body in the critical path, we have two versions: one is normal, and other is for the critical path, and this one will construct the stack needed for merkleization. There are probably some ways to optimize this if needed, because it is possible to determine which statements can generate an element on stack.

Memory and globals should not be problems for generation.

## Instructions, subinstructions and phases

The critical block will have for each instruction a possibility to generate the intermediate state.
Some of the instructions are more complex and need to have several subinstructions, like
* Function call (local initialization)
* br_table
* Check dynamic call

Finally the instructions can be divided into phases, these are designed so that each phase will only need one merkle proof.

Notes:
* Easy to have a pointer to original WASM instruction

