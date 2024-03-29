Here are some ways to handle floating point.

## Define deterministic floating point handling with judges

This means that the floating point ops are implemented in the same way as other operations. Nondeterminism in the standard 
is resolved by selecting one legal value for `NaN`.

Pros:
* All WASM files will work without modifications.

Cons:
* Need to implement floating point operations on-chain.
* Might be unsafe to use JIT.

## Correct all NaNs to a single value

This means that the WASM file will first be converted to another WASM file, where after each operation that might return
NaN, we check the result and if it is NaN, we change it to a standard value. Perhaps it is more efficiently to canonize 
NaN values when a floating point value is stored or reinterpreted as integer. (There is also a copysign operation).

Pros:
* Can run with JIT.

Cons:
* Need to implement floating point operations on-chain.
* Might be harder to check if posted WASM files are valid.

Perhaps this can be combined with the first solution so that before solvers run the code with a JIT, they convert the
WASM file to remove nondeterminism.

## Replace floating point operations with integer implementations

In the WASM file, each floating point operation is replaced with a call to software implementation of that operation.
Link to a software implementation of floating point operations: https://github.com/tianocore/edk2/tree/master/StdLib/LibC/Softfloat


Pros:
* Can run with JIT.
* No floating point needed on-chain.

Cons:
* Integer operations are slower than hardware implementation.
* Have to check that posted WASM files do not use floating point.
