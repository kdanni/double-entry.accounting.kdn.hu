# double-entry.accounting.kdn.hu

Double-entry accounting service. Base building-block for financial planing and invesment management tools.

# Double-Entry Accounting Service

A modular, API-first double-entry accounting service designed as a foundational building block for financial planning and investment management tools.

## Overview

This service implements strict double-entry accounting principles to ensure consistency, traceability, and auditability of financial data.

It is not an end-user product. Instead, it acts as a **core ledger engine** that other applications (financial planners, portfolio managers, budgeting tools) can build on top of.

## Why This Exists

Most financial tools:
- reinvent ledger logic poorly
- break accounting invariants
- become impossible to audit

This service solves that by:
- enforcing double-entry rules at the lowest level
- providing a clean abstraction for financial systems
- separating accounting correctness from business logic

## Core Principles

- **Every transaction balances** (sum of debits = sum of credits)
- **Immutability-first design** (no silent mutations)
- **Auditability** (full history, traceable changes)
- **Composability** (usable as a backend service or library)

## Key Concepts

### Account
Represents a ledger account (e.g., Cash, Revenue, Assets).

Attributes:
- `id`
- `name`
- `type` (asset, liability, equity, income, expense)
- `currency`

### Transaction
A collection of entries that must balance.

Attributes:
- `id`
- `timestamp`
- `description`
- `entries[]`

### Entry
Represents a debit or credit to an account.

Attributes:
- `account_id`
- `amount`
- `direction` (debit | credit)

### Ledger
The complete set of accounts and transactions.

## Features

- Enforced double-entry constraints
- Multi-currency support (optional FX layer)
- Idempotent transaction handling
- Time-based queries (historical snapshots)
- Extensible metadata for integrations
- API-first design (REST / GraphQL / event-driven)

## Example

```json
{
  "transaction": {
    "description": "Buy stocks",
    "entries": [
      { "account_id": "cash", "amount": 1000, "direction": "credit" },
      { "account_id": "investments", "amount": 1000, "direction": "debit" }
    ]
  }
}
```

## Use Cases
- Personal finance apps
- Investment portfolio trackers
- Robo-advisors
- Budgeting systems
- Accounting backends for fintech

## Architecture (Suggested)
- Core ledger engine (pure logic)
- Persistence layer (PostgreSQL or event store)
- API layer
- Optional event streaming (Kafka / queues)

## Non-Goals
- UI / dashboards
- Tax calculation
- Regulatory compliance logic (left to higher layers)
- Future Extensions
- FX conversion engine
- Reporting layer (P&L, balance sheet)
- Smart categorization
- Rule-based automation
