Assume we have a system that can be used to verified load data from IPFS to our tasks using the IPFS hashes.
For simplicity, we could have a function `getData(hash)` that returns an chunk of bytes when given an IPFS hash. There are two problems concerning data availability:
1. the task can just generate a hash of data that is only available for attacker
2. the task can generate a hash from data available to program, but when `getData` is called, the verifiers cannot know that (procedurally generate chunk)

One solution is that the IPFS data has to be registered. There are two possibilities
* the chunk or a chunk that contains the chunk has been registered as being available in the block chain

