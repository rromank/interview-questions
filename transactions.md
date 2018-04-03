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

---

### Default proxy mode and fully initialized proxy
In proxy mode (which is the default), only external method calls coming in through the proxy are intercepted. This means that self-invocation, in effect, a method within the target object calling another method of the target object, will not lead to an actual transaction at runtime even if the invoked method is marked with `@Transactional`. Also, the proxy must be fully initialized to provide the expected behaviour so you should not rely on this feature in your initialization code, i.e.` @PostConstruct`.

---

### @Transactional settings
The `@Transactional` annotation is metadata that specifies that an interface, class, or method must have transactional semantics; for example, "*start a brand new read-only transaction when this method is invoked, suspending any existing transaction*". The default `@Transactional` settings are as follows:

* Propagation setting is `PROPAGATION_REQUIRED`.
* Isolation level is `ISOLATION_DEFAULT`.
* Transaction is read/write.
* Transaction timeout defaults to the default timeout of the underlying transaction system, or to none if timeouts are not supported.
* Any `RuntimeExceptio`n triggers rollback, and any checked `Exception` does not.
