## Transactions
### Transaction propagation

Defines how transactions relate to each other.
* `MANDATORY` - Supports a current transaction, throw an exception if none exists.
* `NESTED` - Execute within a nested transaction if a current transaction exists, behave like `PROPAGATION_REQUIRED` else.
* `NEVER` - Execute non-transactionally, throw an exception if a transaction exists.
* `NOT_SUPPORTED` - Execute non-transactionally, suspend the current transaction if one exists.
* `REQUIRED` - Support a current transaction, create a new one if none exists. (Default)
* `REQUIRES_NEW` - Create a new transaction, and suspend the current transaction if one exists.
* `SUPPORTS` - Supports a current transaction, execute non-transactionally if none exists.

---

### Transaction isolation

Defines the data contract between transaction.
* `READ_UNCOMMITED` - A constant indicating that dirty reads, non-repeatable reads and phantom reads can occur.
* `READ_COMMITED` - A constant indicating that dirty reads are prevented; non-repeatable reads and phantom reads can occur.
* `REPEATABLE_READ` - A constant indicating that dirty reads and non-repeatable reads are prevented; phantom reads can occur.
* `SERIALIZABLE` - A constant indicating that dirty reads, non-repeatable reads and phantom reads are prevented.

Example when a dirty read can occur
```
 thread 1     thread 2
    |            |
 write(x)        |
    |            |
    |         read(x)
    |            |
 rollback        |
    v            v 
              value (x) is now dirty (incorrect)
```

---

### Method visibility and @Transactional
When using proxies, you should apply the `@Transactional` annotation only to methods with public visibility. If you do annotate protected, private or package-visible methods with the `@Transactional` annotation, no error is raised, but the annotated method does not exhibit the configured transactional settings. Consider the use of AspectJ (see below) if you need to annotate non-public methods.
