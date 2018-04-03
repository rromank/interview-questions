## Transactions
### Transaction propagation

Defines how transactions relate to each other. Options:
* `MANDATORY` - Supports a current transaction, throw an exception if none exists.
* `NESTED` - Execute within a nested transaction if a current transaction exists, behave like `PROPAGATION_REQUIRED` else.
* `NEVER` - Execute non-transactionally, throw an exception if a transaction exists.
* `NOT_SUPPORTED` - Execute non-transactionally, suspend the current transaction if one exists.
* `REQUIRED` - Support a current transaction, create a new one if none exists. (Default)
* `REQUIRES_NEW` - Create a new transaction, and suspend the current transaction if one exists.
* `SUPPORTS` - Supports a current transaction, execute non-transactionally if none exists.
