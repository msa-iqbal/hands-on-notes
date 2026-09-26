# Performance

Practical, structured notes for understanding, diagnosing, measuring, and optimizing **database performance**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Query Performance|Query execution, slow queries, bottlenecks, filtering, joins, and query optimization|
|02|Query Plans|Query execution plans, operators, cost estimation, scan methods, joins, and plan analysis|
|03|EXPLAIN|`EXPLAIN`, execution plans, query costs, estimated rows, and plan interpretation|
|04|Connection Pooling|Database connections, connection pools, pool sizing, timeouts, reuse, and connection management|
|05|Lock Contention|Lock contention, blocked queries, waiting transactions, concurrency bottlenecks, and diagnosis|
|06|Memory|Database memory, buffers, caches, working memory, memory pressure, and configuration|
|07|Disk I/O|Disk I/O, storage performance, read/write operations, I/O bottlenecks, and monitoring|
|08|Performance Tuning|Performance profiling, bottleneck identification, configuration tuning, benchmarking, and optimization strategies|

## Structure

The notes progress from **query performance → query plans and `EXPLAIN` → connection pooling → lock contention → memory and disk I/O → performance tuning**.

The early topics focus on understanding how queries execute and identifying inefficient operations, while later topics cover database resource usage, concurrency bottlenecks, configuration, measurement, and systematic optimization.

Each topic is separated into focused notes for easier learning, reference, and expansion.

## Focus

- Database query performance
- Slow query analysis
- Query execution plans
- `EXPLAIN` and plan interpretation
- Query cost and cardinality
- Index and scan behavior
- Connection pooling
- Connection limits and pool sizing
- Lock contention
- Blocked queries and transactions
- Database memory and caching
- Disk I/O and storage performance
- Performance bottleneck analysis
- Database configuration
- Benchmarking and profiling
- Performance monitoring
- Systematic performance tuning

## Goal

> Understand database performance systematically, identify bottlenecks using evidence and execution plans, and apply practical optimization techniques to build fast and efficient database systems.
