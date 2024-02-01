# Transactions

## Isolation Problems

- Lost Updates: Two concurrent transactions simultaneously updates the same information in the database.
    - Scenario:
        - First transaction reads the value
        - Second transaction starts shortly after and reads the same value.
        - The first transaction changes and writes the updated value, and the second transaction overwrites that value
          with its own update.
    - Problem: the update of the first transaction is lost, being overwritten by the second transaction. The last commit
      wins.
- Dirty reads: Transaction 1 reads changes made by transaction 2, and it hasn't being committed yet.
    - Problem: The changes made by transaction 1 may later be rolled back, and invalid data will have been read by
      transaction 2
- Unrepeatable reads: A transaction reads a data twice and reads different states each time.
- Phantom reads: A transaction executes a query twice, and the second result includes data that wasn't visible in the
  first result because something was added by another transaction, or it includes fewer data because something was
  deleted.

## ANSI isolation levels and the problems they address

| **Isolation Level** | **Phantom Reads** | **Unrepeatable reads** | **Dirty reads** | **Lost update** |
|---------------------|-------------------|------------------------|-----------------|-----------------|
| READ_UNCOMMITTED    | -                 | -                      | -               | +               |
| READ_COMMITTED      | -                 | -                      | +               | +               |
| REPEATABLE_READ     | -                 | +                      | +               | +               |
| SERIALIZABLE        | +                 | +                      | +               | +               |

**Notes**:

- With increased levels of isolation come higher costs and serious degradations of performance and scalability
- Which isolation to choose?
    - For almost all scenarios, eliminate **READ_UNCOMMITTED**. Use only for debugging purposes.
    - Most applications do not need **SERIALIZABLE**. Phantom reads are usually not that problematic, and this isolation
      level tends to scale poorly.
    - For almost all multiuser JPA application, **READ_COMMITTED** isolation for all database transactions is acceptable
      with enabled entity version.