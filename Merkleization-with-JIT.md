## Gas metering

To implement the metering in a way that it doesn't make everything 10x slower, I propose the following:
* at the beginning of each function, add gas usage of all instructions in the function
* at the beginning of each loop iteration, add gas usage of all instructions inside the loop
* loops can be ignored when calculating the gas usage of a function

## Critical path



