## Uploading data to blockchain

Using storage: 20000/32 = 625 gas per byte

Benchmarks for uploading data to blockchain as contract code:
 * 1Kb: 328340 gas -> 320 gas per byte
 * 2Kb: 602970 gas -> 294 gas per byte
 * 4Kb: 1152242 gas -> 281 gas per byte
 * 10Kb: 2800154 gas -> 273 gas per byte
 * 20Kb: 5546994 gas -> 270 gas per byte

Of this, the transaction overhead is about 70 gas per byte.

Updating data: 5000/32 = 156 gas per byte. So for volatile data, using storage is cheaper.

## Storj, Filecoin, other similar services

Can these help with data availability, or can the task giver still control the availability?

## Problems

The first problem is that task giver posts a task hash, but doesn't give the data to the solver. It is hard to determine which one is wrong.

If the task giver controls access to the data needed for solving the task, he can perhaps find out the identities of the verifiers and bribe them.

Who will pay for the distribution of the data?

## Some ideas

The peers could vote whether the data is available or not. But perhaps there is no incentive to vote correctly. Then the result would be that the enemies of the task giver vote against and the friends vote for him.

If an actor is suspicious that data is not available, 

