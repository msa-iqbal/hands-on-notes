# Transactions

Practical, structured notes for understanding and managing **database transactions, concurrency, isolation, and locking**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Transaction Basics|Transactions, transaction boundaries, atomic operations, and transaction lifecycle|
|02|ACID|Atomicity, Consistency, Isolation, Durability, and transaction guarantees|
|03|Commit|`COMMIT`, making changes permanent, transaction completion, and commit behavior|
|04|Rollback|`ROLLBACK`, undoing changes, transaction failure, and recovery|
|05|Isolation Levels|Read Uncommitted, Read Committed, Repeatable Read, Serializable, and isolation anomalies|
|06|Locking|Locks, shared locks, exclusive locks, row-level locking, table-level locking, and lock management|
|07|Deadlocks|Deadlock conditions, detection, prevention, resolution, and retry strategies|
|08|Optimistic Locking|Version-based concurrency control, conflict detection, retries, and application-level locking|
|09|Pessimistic Locking|Explicit locking, lock acquisition, contention, and database-level concurrency control|

## Structure

The notes progress from **transaction fundamentals → ACID guarantees → commit and rollback → isolation levels → locking and deadlocks → optimistic and pessimistic concurrency control**.

The early topics establish how transactions work and guarantee data consistency, while later topics focus on concurrent access, locking strategies, conflict handling, and practical concurrency patterns.

Each topic is separated into focused notes for easier learning, reference, and expansion.

## Focus

- Database transaction fundamentals
- Transaction lifecycle and boundaries
- ACID properties
- Commit and rollback
- Transaction failure and recovery
- Isolation levels
- Read and write anomalies
- Shared and exclusive locks
- Row-level and table-level locking
- Lock contention
- Deadlocks and prevention
- Optimistic concurrency control
- Pessimistic concurrency control
- Conflict detection and retry strategies
- Concurrent database operations

## Goal

> Understand database transactions and concurrency control, maintain data consistency under concurrent workloads, and choose appropriate isolation and locking strategies for real-world applications.
