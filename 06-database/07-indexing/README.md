# Indexing

Practical, structured notes for understanding, designing, maintaining, and optimizing **database indexes**.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Index Basics|What indexes are, how they work, index structures, benefits, and trade-offs|
|02|B-Tree|B-Tree indexes, ordering, range queries, equality searches, and common use cases|
|03|Hash Index|Hash indexes, equality lookups, limitations, and practical use cases|
|04|Composite Index|Multi-column indexes, column order, prefix matching, and query patterns|
|05|Covering Index|Covering indexes, index-only scans, included columns, and reducing table access|
|06|Partial Index|Partial indexes, conditional indexing, predicates, and targeted optimization|
|07|Full-Text Index|Full-text indexing, text search, tokenization, and search-oriented queries|
|08|Index Selectivity|Selectivity, cardinality, filtering effectiveness, and choosing useful indexes|
|09|Index Maintenance|Index size, fragmentation, rebuilding, monitoring, and maintenance strategies|
|10|Index Performance|Query performance, index usage, execution plans, benchmarking, and optimization|

## Structure

The notes progress from **index fundamentals → common index structures → composite and specialized indexes → index selectivity and maintenance → index performance optimization**.

The early topics explain how indexes work and when different index types are useful, while later topics focus on designing effective indexes, maintaining them, and evaluating their impact on query performance.

Each topic is separated into focused notes for easier learning, reference, and expansion.

## Focus

- Database index fundamentals
- Index structures and data access
- B-Tree indexes
- Hash indexes
- Composite indexes
- Covering and index-only access
- Partial indexes
- Full-text indexes
- Index selectivity and cardinality
- Index column ordering
- Index maintenance
- Index monitorin
- Query execution plans
- Index usage and effectiveness
- Database query optimization
- Index performance trade-offs

## Goal

> Understand how database indexes work, design effective indexes for real-world query patterns, maintain them properly, and use execution plans to optimize database performance.
