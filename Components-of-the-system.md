
What part of the system each repo implements
 * truebit-contracts: the incentive layer, implements contracts for posting tasks to the system, deposits, jackpot.
 * webasm-solidity: Judging contracts and verification game. Also includes the test node, it listen to events from a simplified version of TrueBit contract, and starts solvers and verifiers. Can also be used to post a task. 
 * ocaml-offchain: Offchain interpreter, can be used to run tasks and generate inputs needed for verification game.
 * wasm-preprocessor: the offchain interpreter and judges use a simplified bytecode instead of WASM files, this program can be used to define a TrueBit task that converts a WASM file to the internal format.
 * emscripten-module-wrapper: after a C or Rust project has been compiled to WASM using emscripten, this wrapper can be used to link our runtime into this WASM file.

