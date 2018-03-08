## General performance

There are two modes for the interpreter:
1. Generate outputs from inputs
2. Generate intermediate states or Merkle roots

For the first case, it should be straightforward to use JIT. But if the performance difference between JIT and interpreter is too high, the feasibility of the system becomes endangered. For example JIT might perform a task in a minute, but the interpreter might take an hour. Idea: perhaps the system could pay to the task giver if there is a delay?

## Gas metering

Metering by instruction makes JIT much slower. This can be optimized though, the current approach at https://github.com/ewasm/wasm-metering seems to be OK.

## Easy merkleization

There are three concerns here:
1. It must be easy enough to convert the state into a merkle tree (note that the tree cannot have arbitrary depth)
2. Generating proofs for atomic transitions
3. Handling initialization of the code (jump tables, memory, etc.)

The best solution would be to have all this implemented using instrumentation, see https://github.com/TrueBitFoundation/webasm-solidity/wiki/Merkleization-with-JIT ... not sure yet if this is possible.
