# Database

Practical, step-by-step **database recipes** for backing up, restoring, and migrating PostgreSQL, MySQL, MongoDB, and SQLite databases, along with general database migration procedures.

## Contents

|#|Topic|What You'll Learn|
|---|---|---|
|01|Backup PostgreSQL|Create PostgreSQL backups, choose backup formats, include required database objects, verify backup files, and manage backup storage|
|02|Restore PostgreSQL|Restore PostgreSQL databases from backups, handle existing databases, verify restored data, and troubleshoot restore issues|
|03|Migrate PostgreSQL|Migrate PostgreSQL databases between servers or environments, transfer schemas and data, preserve required objects, and verify migration results|
|04|Backup MySQL|Create MySQL backups, export databases and tables, handle required options, verify dump files, and manage backup storage|
|05|Restore MySQL|Restore MySQL databases from backup files, handle existing data, verify restored objects, and troubleshoot restore issues|
|06|Backup MongoDB|Create MongoDB backups, export database data, preserve collections and metadata where applicable, verify backups, and manage backup storage|
|07|Restore MongoDB|Restore MongoDB databases and collections from backups, handle existing data, verify restored data, and troubleshoot restore issues|
|08|Backup SQLite|Back up SQLite databases safely, handle active database files, verify backup integrity, and manage backup copies|
|09|Migrate Database|Migrate databases between servers, environments, or database systems, transfer schemas and data, handle compatibility concerns, and verify migration results|

## Structure

The recipes progress from **database backup → database restoration → PostgreSQL migration → MySQL backup and restoration → MongoDB backup and restoration → SQLite backup → general database migration**.

The first sections focus on PostgreSQL, covering the complete backup, restore, and migration lifecycle. These recipes provide repeatable procedures for protecting PostgreSQL data and moving databases between environments.

The MySQL and MongoDB sections then apply similar operational workflows to other commonly used database systems. Each database has dedicated backup and restore procedures so that system-specific commands, formats, and considerations remain clear.

The SQLite recipe focuses on safe database backup and integrity verification for file-based databases. Unlike server-based databases, SQLite requires particular attention to active database files and consistent backup procedures.

The final migration recipe covers database migration as a broader operational procedure, including moving data between environments or systems, addressing compatibility considerations, and verifying the migrated database.

Each recipe follows an **action-oriented workflow** with commands, configuration, expected results, verification steps, troubleshooting guidance, and important safety considerations.

## Focus

- PostgreSQL backup and restoration
- PostgreSQL database migration
- MySQL backup and restoration
- MongoDB backup and restoration
- SQLite database backup
- Cross-environment database migration
- Backup verification and data integrity
- Database recovery and migration troubleshooting

## Goal

> Build a practical database recipe knowledge base that makes backup, restoration, migration, recovery, verification, and database-maintenance procedures easy to follow, troubleshoot, reproduce, and maintain.
