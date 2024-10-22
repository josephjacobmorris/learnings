# Eventual Consistency

## Introduction
Eventual consistency refers to a system state where all components reach consistency over time, even though they may be temporarily inconsistent. This approach is critical in distributed systems and helps achieve scalability.

Here are four common patterns to implement eventual consistency:

1. **Event-Based Eventual Consistency**: Services emit events when their state changes, and other services listen and update accordingly. This enables scalability and loose coupling but introduces delays before all systems reflect the latest state.

2. **Background Sync Eventual Consistency**: A background process periodically synchronizes data between systems, allowing for performant updates but slower eventual consistency since the sync only happens at intervals.

3. **Saga-Based Eventual Consistency**: Distributed transactions are broken into smaller steps, with each service handling its own transaction. If an error occurs, compensating transactions roll back changes, ensuring eventual consistency.

4. **CQRS-Based Eventual Consistency**: Separates read and write models. The write model is updated immediately, while the read model is synchronized asynchronously, leading to eventual consistency between the two.

## References
* chatgpt 
* https://chatgpt.com/share/6717ea59-223c-800c-8d7a-2b56216df6df
* https://newsletter.systemdesigncodex.com/p/eventual-consistency-is-tricky?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true