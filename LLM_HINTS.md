
---

# 📄 LLM_HINTS.md

```markdown
# LLM Hints for Double-Entry Accounting Service

This document helps Large Language Models (LLMs) understand and correctly interact with the system.

## Core Rules (Do Not Violate)

1. **Every transaction must balance**
   - Total debits == total credits
   - Reject or fix any unbalanced transaction

2. **No implicit money creation**
   - All value must come from an existing account

3. **Entries are atomic**
   - A transaction is valid only if all entries are valid

4. **Immutability is preferred**
   - Do not modify past transactions unless explicitly allowed
   - Prefer reversal transactions

## Mental Model

Think of the system as:

> A graph of value movements between accounts, not a set of balances.

Balances are derived, not stored as truth.

## Common Patterns

### Transfer Between Accounts

- Debit destination
- Credit source

### Expense

- Debit expense account
- Credit cash/bank

### Income

- Debit cash/bank
- Credit income

### Investment Purchase

- Debit asset (investment)
- Credit cash

### Liability Increase

- Debit cash
- Credit liability

## Common Mistakes (Avoid These)

- ❌ Creating a transaction with only one entry
- ❌ Forgetting to balance amounts
- ❌ Mixing debit/credit semantics incorrectly
- ❌ Treating balances as mutable state
- ❌ Using negative numbers instead of direction fields

## API Interaction Guidelines

When generating transactions:

- Always include:
  - description
  - timestamp (if required)
  - full entry list

- Ensure:
  - consistent currency
  - valid account IDs
  - correct direction labels

## Validation Checklist

Before submitting a transaction:

- [ ] Do debits equal credits?
- [ ] Are all accounts valid?
- [ ] Are amounts positive?
- [ ] Are directions correct?
- [ ] Is the business logic consistent?

## Advanced Considerations

### Multi-Currency

- Do not mix currencies in a single transaction unless FX logic is explicitly defined

### Idempotency

- Use unique transaction IDs when retrying operations

### Time

- Transactions are append-only
- Historical queries must respect timestamps

## Suggested Reasoning Strategy for LLMs

1. Identify the business event
2. Determine affected accounts
3. Assign debit/credit directions
4. Ensure balance
5. Output structured transaction

## Example Reasoning

Event: "User deposits 500 EUR into account"

- Cash increases → Debit cash
- Equity increases → Credit equity

Result:
- Debit: Cash 500
- Credit: Equity 500

## Tone & Behavior Expectations

- Be precise, not creative
- Prefer correctness over convenience
- Reject invalid financial logic instead of guessing

## Final Rule

If the transaction does not balance, it is wrong.
No exceptions. Not even "just this once."
