## Transaction
- A transaction in redis consists of a block of commands.
- `MULTI`, `EXEC`, `DISCARD` and `WATCH` are the foundation of trnasactions in redis.
- All the commands in a transaction are serialized and executed sequentially.
- There will never be a case when a new request is issued by another client and is served **in the middle** of the execution of a redis transaction. This guarantees that the commands are executed as a single isolated operation.
- Transaction are atomic, either all of the commands are processed or none are.
    - If a client asses invalid commands to the server in the context of a transaction then none of the operations are performed.
---

## Multi, Exec, Watch And Discard Command
- `multi` - starts a transaction, every command issued after this is in the context of a transaction. All commands will be placed into a queue until executed.
- `exec` - will execute the queued commands in the transaction. This also ends the current transaction.
- `discard` - aborts a transaction.
- `watch [key...]` - provides a check and set behavior in redis. You can monitor your keys to detect changes in them and if the key that is being watched is modified before a transaction starts, the whole transaction aborts when the 'exec' command is called. The 'exec' command will also cause the watched command to be unwatched.
- `unwatch` - will stop watching every watched key.
---

## Errors Inside A Transaction
- It is possible to encounter errors during a transaction.
- Types of errors:
  - A command may fail to be queued, so there may be an error before `EXEC` is called.
  - A command could be syntactically wrong.
  - A command may fail after `EXEC` is called. Running list operations on a string(redis will have to exec the command to determine the error).
- Redis discards the transaction if errors are present.
---

