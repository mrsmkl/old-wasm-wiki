## Basics

The system has state `S`. There are transactions (set `TR`) that can modify the system state. Assume there are two actors, the node and the opponent with the sets of transactions `TR_n \union TR_o = TR`. These transactions can generate new system state `process(S, tr)`

The node has internal state. The goal of the node is to keep its internal state consistent with the system state, represented by predicate `consistent(S)`. The system and the predicate have to be designed so that the opponent cannot put it to inconsistent state
```
consistent(S) /\ tr \in TR_o => consistent(process(S, tr))
```

For example in the verification game:
* For the solver, the state will be consistent if each step hash stored in `S` is correct according to the solver
* For the verifier, each step hash stored in `S` is correct if it is before `idx1` and incorrect if it is after `idx2`.

## What data the events should include?

I think it is simplest if the events include as much data as is needed for generating a reply.
But it will be cheaper to include less data, so we could have a partial function `query` that will return more data.

There is the following requirement: for each event `e` and states `S1` and `S2`,
```
query(S1, e) == query(S2, e)
```
Otherwise there could be inconsistent data.

## Generated transactions

For each event, one transaction should be generated: `generate(ev) \in TR_n`.
The generated event must keep the state consistent:
```
consistent(S) => consistent(process(S, generate(ev)))
```

## Practical example

Assume that the verifier queries the hash of a state with event `query(i)`. The internal state `S` includes the pointer `i`. Then the solver would post transaction `reply(hash)`, and the state would become such that step `i` has hash `hash`. Now if the block with event `query(i)` is cancelled, and the new block has event `query(j)` such that `i <> j`, then the transaction `reply(hash)` could be replayed and the new system state would have `hash` at position `j`. This state is inconsistent with the internal state of the solver.

## Missing events or transactions

The events or transactions might become lost, so there needs to be a way for the nodes to poll the state of the system. So there basically should be a way to find the event it should be replying to. Perhaps the main contract will store it or each actor will have their own contract that is updated from the main contract.
