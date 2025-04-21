# ACID Principle

The ACID principle is a set of properties that ensure reliable processing of database transactions. These properties are crucial for maintaining data integrity in database systems, especially in the event of errors, power failures, or concurrent access by multiple users.

## Summary Table

| Property       | Description                                                          | Key Mechanisms                                           | Example                                                 |
|----------------|----------------------------------------------------------------------|----------------------------------------------------------|----------------------------------------------------------|
| **Atomicity**  | Ensures all operations in a transaction are completed or none        | Undo logs, transaction management systems               | Bank transfer: rollback if debit succeeds but credit fails |
| **Consistency**| Ensures DB transitions from one valid state to another               | DB constraints (e.g., primary keys), app-level checks    | E-commerce: prevent stock from going below zero         |
| **Isolation**  | Prevents interference between concurrent transactions                | Isolation levels: Read Uncommitted → Serializable        | Flight booking: only one user books the last seat       |
| **Durability** | Guarantees committed changes are permanent                           | WAL (Write-Ahead Logging), checkpoints, backups          | Online purchase: order persists after a system crash     |

## What is a Transaction?

A transaction is a sequence of one or more operations performed as a single logical unit of work. A transaction must adhere to the ACID properties to ensure consistency and reliability in a database.

## ACID Properties

### 1. Atomicity

Atomicity ensures that a transaction is treated as a single, indivisible unit. Either all operations in the transaction are completed successfully, or none of them are applied. This prevents partial updates to the database.

#### Key Mechanisms:
- Undo logs are often used to roll back incomplete transactions.
- Transaction management systems ensure atomicity by marking transactions as either "committed" or "aborted."

#### Example:
Consider a bank transfer where $100 is moved from Account A to Account B:
- Debit $100 from Account A.
- Credit $100 to Account B.

If the system crashes after debiting Account A but before crediting Account B, atomicity ensures that the transaction is rolled back, leaving both accounts unchanged.

---

### 2. Consistency

Consistency ensures that a transaction brings the database from one valid state to another. Any rules, constraints, or integrity conditions defined in the database must be preserved after the transaction is completed.

#### Key Mechanisms:
- Database constraints such as primary keys, foreign keys, and unique constraints help enforce consistency.
- Application-level validations also contribute to maintaining consistency.

#### Example:
In an e-commerce application:
- A product's stock level must never go below zero.
- If a customer places an order, the stock is reduced only if sufficient inventory exists.

If a transaction violates this rule, it will be aborted to maintain consistency.

---

### 3. Isolation

Isolation ensures that concurrent transactions do not interfere with each other. The intermediate state of a transaction is not visible to other transactions until it is committed.

#### Isolation Levels:
- **Read Uncommitted**: Transactions can read uncommitted changes from other transactions (least strict).
- **Read Committed**: Transactions can only read committed changes.
- **Repeatable Read**: Ensures that data read during a transaction cannot change.
- **Serializable**: Ensures complete isolation by serializing transactions (most strict).

#### Example:
Two users are attempting to book the last seat on a flight:
- User A and User B both try to reserve the seat simultaneously.
- Isolation ensures that only one transaction succeeds, and the other is either retried or aborted.

This prevents conflicts and ensures accurate results.

---

### 4. Durability

Durability ensures that once a transaction is committed, its changes are permanent, even in the event of a system crash. The database system uses mechanisms like write-ahead logs or backups to guarantee durability.

#### Key Mechanisms:
- Write-ahead logging (WAL) ensures that changes are written to a log before being applied to the database.
- Checkpoints and backups are used to recover committed transactions after a crash.

#### Example:
After a customer completes an online purchase:
- The order details are stored in the database.
- Even if the system crashes immediately after the confirmation, the order will still exist when the system is restored.

---

## Summary

The ACID properties work together to ensure the reliability and correctness of database transactions:
- **Atomicity**: All or nothing.
- **Consistency**: Valid state transitions.
- **Isolation**: No interference between transactions.
- **Durability**: Changes are permanent after commit.
