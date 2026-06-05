# Consistency Models

## Overview

A consistency model defines what a system guarantees about the values a read can return when data is replicated or accessed concurrently. The model determines whether a read always reflects the most recent write, or whether it may return an older value for some time. Distributed systems make this an explicit decision because replication and partitioning create a window in which copies disagree.

---

## The consistency spectrum

| Model | Guarantee | Consequence |
|---|---|---|
| Strong (linearizable) | Every read returns the most recent committed write | Requires coordination across replicas — higher latency, reduced availability under partition |
| Causal | Reads respect cause-and-effect order; related writes are seen in order | Concurrent, unrelated writes may be seen in different orders |
| Eventual | Replicas converge to the same value once writes stop | A read may return stale data until convergence |

Strong consistency behaves as if there were a single copy of the data. Eventual consistency allows replicas to diverge temporarily and converge later. Causal consistency sits between the two: it preserves the order of operations that depend on one another without paying the full cost of global coordination.

---

## CAP and PACELC

The CAP theorem states that when a network partition (P) occurs, a distributed system can preserve either consistency (C) or availability (A), but not both.

- A **CP** system rejects requests it cannot serve consistently during a partition: it stays consistent and loses availability.
- An **AP** system keeps serving during a partition: it stays available and may return stale data.

When there is no partition, the trade-off does not apply. PACELC extends CAP to that case: Else (no partition), a system trades Latency against Consistency. Strong consistency costs latency even on a healthy network, because reads and writes coordinate across replicas.

---

## Client-centric guarantees

Beyond global models, systems offer guarantees scoped to a single client's view. These are weaker than strong consistency but cover the cases users notice:

- **Read-your-writes:** a client always sees its own prior writes — a user sees the comment they just posted.
- **Monotonic reads:** a client never sees data move backwards in time across successive reads.
- **Consistent prefix:** a client sees writes in an order that could have occurred, never a later write without an earlier one it depends on.

A common implementation routes a client's reads to the replica that served its writes, or pins the client to the leader for a short window after a write.

---

## Where the choice appears

Eventual consistency is not an abstract concept; it shows up wherever a system holds more than one copy of data:

- Read replicas lag the leader, so reads from a replica are eventually consistent — see [Data Architecture](data-architecture.md#replication).
- A saga leaves the system in intermediate states until all steps complete, producing eventual consistency across services — see [Distributed Transactions](distributed-transactions.md#saga).
- A cache holds a copy that can be staler than its source — see [Caching](caching.md).

---

## Key trade-offs

| Decision | Benefit | Cost |
|---|---|---|
| Strong consistency | Reads are always current; simplest to reason about | Coordination latency; unavailable under partition (CP) |
| Eventual consistency | High availability, low latency, partition-tolerant | Application must tolerate stale reads and conflicts |
| Causal consistency | Preserves meaningful order without global coordination | More complex to implement and track |
| Client-centric guarantees | Cover user-visible cases cheaply | A per-client view, not a global invariant |

---

## Common pitfalls

- **Assuming strong consistency by default.** Many distributed stores are eventually consistent unless configured otherwise; the guarantee has to be checked, not assumed.
- **Ignoring replication lag in the UI.** A user who does not immediately see their own action assumes it failed and retries.
- **Treating "eventual" as "soon".** Convergence time is unbounded under load or partition; code cannot rely on a tight window.
- **Concurrent conflicting writes with no resolution policy.** Multi-leader and leaderless setups need an explicit strategy — last-write-wins, CRDTs, or application-level merge.
