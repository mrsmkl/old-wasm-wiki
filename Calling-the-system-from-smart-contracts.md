## Adding a task

When contracts on the blockchain need more computational power, they can use TrueBit to create computational tasks using method `addWithFile` in the TrueBit contract. There are three parameters:
* `init`: the Merkle root of the initialized code. Code initialization is described here:  https://github.com/TrueBitFoundation/ocaml-offchain/wiki/Initializing-and-preprocessing-WebAssembly
* `code_file`: this is the IPFS hash of the file that contains the task code.
* `input_file`: this is the id of a file stored in the blockchain. This file is the input to the task.

## Adding input files

The input file has to be registered with method `createFileWithContents(string name, uint nonce, bytes32[] arr, uint sz)`.
The parameters are as follows:
* `name`: name of the file.
* `nonce`: used to generate a unique identifier. It is just `keccak256(msg.sender, nonce)`.
* `arr`: contents of the file. Currently each element in the array will become one byte.
* `sz`: this will be the size in bytes, once the Merkle trees have been optimized.

## Solving the task

The method `createFileWithContents` will also calculate the Merkle root of the file. The solver will then have to post an initial state, where one of the files in the IO block has the same Merkle root as this file (method `ensureInputFile` at https://github.com/TrueBitFoundation/webasm-solidity/blob/master/tasks.sol). The checks for file size and name are not yet implemented.

After the solver has computed the task, it will have to upload the output file into the blockhain using `createFileWithContents`. It will also have to submit proof that this file is in the IO block of the final state in computation (done at method `finalize` at https://github.com/TrueBitFoundation/webasm-solidity/blob/master/tasks.sol).

## Callback

When the result of the task is accepted, a callback is called from the task giver contract. The contents of the output file can be given as an argument to the callback.

A sample smart contract can be found here:
https://github.com/TrueBitFoundation/webasm-solidity/blob/master/testUser.sol

