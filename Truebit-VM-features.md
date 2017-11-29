## Proof of concept

Features
 * Can execute WebAssembly except floating point
 * Simple file system
 * Support for native execution (JIT)

## Revision 2

New features:
 * Parallel computation
 * Floating point support
 * IPFS support

Perhaps we should follow WASM structure more carefully:
 * Split code segment into functions
 * Split stack into frames

Implementation options
 * Could be implemented in Rust, based on parity-wasm
 * Another idea that I want to test is translating the WASM file so that it will have explicit stack, perhaps JIT compilation will make this faster than interpreter.

