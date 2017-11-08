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
NaN, we check the result and if it is NaN, we change it to a standard value.

Pros:
* Can run with JIT.

Cons:
* Need to implement floating point operations on-chain.
* Might be harder to check if posted WASM files are valid.

## Replace floating point operations with integer implementations

Pros:
* Can run with JIT.
* No floating point needed on-chain.

Cons:
* Integer operations are slower than hardware implementation.
* Have to check that WASM files that are posted do not use floating point.