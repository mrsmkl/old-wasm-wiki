## Notation

System state `S`

## What data the events should include?

I think it is simplest if the events include as much data as is needed for generating a reply.
But it will be cheaper to include less data, so we could have a partial function `query` that will return more data.

There is the following requirement: for each event `e` and states `S1` and `S2`, `query(S1, e) == query(S2, e)`.
Otherwise there could be inconsistent data.

## Generated transactions

For each event, one transaction should be generated. 

## Failure

The events might become lost, so there needs to be a way for the nodes to poll the state of the system. So there basically should be a way to find the event it should be replying to. Perhaps the main contract will store it or each actor will have their own contract that is updated from the main contract.

