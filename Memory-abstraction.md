The basic difference between off-chain and on-chain interpreters is that off-chain interpreter has easy access to the machine state, and the on-chain interpreter will only have access to the parts of the memory that was sent to blockchain. We say that the interpreter is stuck if there is not enough data to run it forward.

Here is a simplified memory model for off-chain interpreter:
```
contract Onchain {
  uint pc;
  uint reg1;
  uint opcode;
  uint[] code;
  uint[] memory;
  function getPc() returns (uint) {
     return pc;
  }
  function setPc(uint newval) {
     pc = newval;
  }
  function getMemory(uint pos) returns (uint) {
     return memory[pos];
  }
  function setMemory(uint pos, uint newval) {
     memory[pos] = newval;
  }
}
```

The memory model for on-chain interpreter is as follows:
```
contract Onchain {
  bytes32 state; // Initially only the state is known
  uint pc; // Solver can post these other variables to make the interpreter be able to run forward
  uint reg1;
  uint opcode;
  bytes32 code_root;
  bytes32 memory_root;
  bytes32[] merkle_proof;
  function getPc() returns (uint) {
     // If the hash is not correct, we do not yet know what the pc is in the state
     require(sha3(pc, reg1, opcode, code_root, memory_root) == state);
     return pc;
  }
  function setPc(uint newval) {
     require(sha3(pc, reg1, opcode, code_root, memory_root) == state);
     pc = newval;
     // State has changed, generate new hash
     state = sha3(pc, reg1, opcode, code_root, memory_root);
  }
  // getRoot, getLeaf and setLeaf are defined in instruction.sol
  function getMemory(uint pos) returns (uint) {
     require(sha3(pc, reg1, opcode, code_root, memory_root) == state);
     // If the merkle proof is not correct, we do not know what is the memory value in position pos
     require(getRoot(merkle_proof, pos) == memory_root);
     return uint(getLeaf(merkle_proof, pos));
  }
  function setMemory(uint pos, uint newval) {
     require(sha3(pc, reg1, opcode, code_root, memory_root) == state);
     require(getRoot(merkle_proof, pos) == memory_root);
     setLeaf(merkle_proof, pos, bytes32(newval));
     // First generate new memory root, then new state
     memory_root = getLeaf(merkle_proof, pos);
     state = sha3(pc, reg1, opcode, code_root, memory_root);
  }
}
```

The rest of the interpreter is the same for on-chain and off-chain interpreters. We just have to make sure that each step or phase is small enough so that it won't be stuck. Another possibility would be to extend the on-chain memory model so that more data can be posted.

Reference: https://github.com/chriseth/scrypt-interactive/
