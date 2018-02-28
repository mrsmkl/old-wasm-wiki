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
