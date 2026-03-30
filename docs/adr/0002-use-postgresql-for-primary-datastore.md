# Use PostgreSQL for Primary Datastore

- Status: accepted
- Date: 2026-03-15

## Context and Problem Statement

We need to select a primary relational database for our application's transactional data. The system handles user accounts, orders, and inventory with complex queries and strict consistency requirements.

## Decision Drivers

- Strong consistency guarantees for financial transactions
- Mature ecosystem with proven operational tooling
- Support for JSON columns to handle semi-structured data alongside relational data
- Team familiarity and hiring pool availability

## Considered Options

- PostgreSQL
- MySQL 8
- CockroachDB

## Decision Outcome

Chosen option: "PostgreSQL", because it provides the best balance of ACID compliance, JSON support via `jsonb`, and extensibility (e.g., PostGIS for future geo features). The team has deep operational experience running PostgreSQL in production, and managed offerings (RDS, Cloud SQL) reduce operational burden.

### Consequences

- All services requiring relational storage will use PostgreSQL 16+
- Schema migrations will be managed via Flyway
- Connection pooling will use PgBouncer in transaction mode
- CockroachDB remains a future option if we need horizontal write scaling
