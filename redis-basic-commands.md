## Redis Basic Commands
- `docker exec -it <redis container name> redis-cli` allows for you to interact with redis.
- `set <key> <value> [optional parameters]` store a key value pair into redis. If a value is already set then it will be overwritten.
  - Ex. `set word "hello"` stores 'word' as the key and "hello" as the value. You don't need to use double quotes to specify a string in redis.
  - Ex. `set word hello nx`, it will set the value 'hello' to the key 'word' if the key word doesn't exist.
  - Ex. `set word hello xx`, it will set the value 'hello' to the key 'word' if the key word only if it already exists in the DB. 

- `get <key>` gets a value by key from redis.
  - Ex. `get word` - returns the value
- `del <key1> <key2>` - deletes key value pairs by key. You can delete 1 or multiple keys.
  - Ex `del word` - returns how many keys were deleted, in this case, it was just 1.
  - `del word word1 word2` - if all those keys exists, it will delete them and return 3 because that's how many keys were deleted.
- `exists <key> <key>` - used to check whether keys exist or not.
  - Ex. `exists name` - Will return `0` if the key does not exist and `1` if the key exists.
  - `exists name name2` - Will return `2` if both exists, `1` if one exists and `0` if neither exists.
- Expiration of keys:
  1. `set name "John" ex 10` - `ex` is for expires and `10` means 10 second, so the key will expire after 10 seconds.
    - `ttl <key>` - gives you how many seconds are left until your key expires
    - Ex. `ttl name` - if it return `-2` then the key is no longer in the redis database(it will also return `-2` if the key never existed as well).
    - If there is no expiration set for a key then `ttl <key>` will return `-1`
    - If you don't want to use seconds and prefer to use milliseconds then you would use `Px <milliseconds>` instead of `ex <seconds>`
    - Ex. `set name "John" Px 1000`
    - To check the time to live in milliseconds, you have to use `pttl <key>` instead of `ttl <key>`. `ttl <key>` will tell you the time to live in seconds.
  2. If a key exists in the database then you can set the expiration like this:
    - `Expire <key> <seconds>`
    - Ex. `Expire name 20` - the key 'name' will expire after 20 seconds.
    - To expire in milliseconds then you would use `pexpire <key> <milliseconds>`
    - Ex. `pexpire name 10000`
  3. Remove expiration from a key `persist <key>`, this will get rid of an expiration on a key
    - Ex. `persist name`
- `Randomkey` - gives you a random key from the DB.
- `Rename <key> <newKey>` - renames key. Doesn't check if no key name already exists in the DB.
  - Ex. `Rename name friend`
  - If you rename a key to one that exists in the DB, then you will overwrite the value for that key.
- `renamenx <key> <newKey>` - renames the key if the new key name doesn't exists in the DB. If the new key name exists, then redis will return `0` and not overwrite the value and will not delete the previous key that we are trying to rename. `nx` means not exists.
  - `renamenx name friend` - if the friend key already exists then redis will just print `0` and not overwrite. If friend does not exists then redis will return `1` which means that it renamed `name` to `friend` successfully.
- `touch <key or [...keys]>` - allows you to change the last access time of a key.
  - `touch name` - will return 1, meaning the operation was successful.
- `unlink <key or [...keys]>` - is like `del` but it works asynchronously and works on a separate thread, `del` is synchronous and blocks the current thread.
  - `unlink name` - will return `1` if deleted successfully. `unlink` should be used over `del` when you have a lot of keys to delete for performance reason.
- `type <key>` - tells you the type of value being stored for a key.
- `type name` - will return the data type.
---

## Redis Shutdown Command
- Shutdowns down redis server.
- `shutdown options[NOSAVE|SAVE]` - if you save your data will be stored on the disk if you choose not to save then it won't be saved onto the disk.
- Ex. `shutdown save`
- Ex. `shutdown nosave`
---

## Redis Dump and Restore Commands
- The dump command serializes the value stored in a key in a redis specific format that we can't read. It will use that serialized data to back up your data.
- This dump command is used to create a backup for data stored in your keys.
- If you use the dump command you can restore data at any point.
- A redis value encoded by a specific redis version cannot be decoded by another redis version.
- `dump <key>` - will return a serialized value in the terminal.
  - Ex. `dump name`
  - After dumping you can restore the data
  - `restore <key> <ttl(when the key should expire)> <serialized value from the dump command>`
    - Ex. `restore name 0 "\x00\x04John\t\x00\x83\x1a\xe6\x95\x95\r\x17\xa9"` - will look something like this, if you put `0` for `ttl` the key will not expire.
    - `restore` only works if the target key is already deleted.
    - If you would like to replace the key in your DB use the `REPLACE` option:
      - `restore name 0 "\x00\x04John\t\x00\x83\x1a\xe6\x95\x95\r\x17\xa9" REPLACE` - is will replace the key if it is not deleted from your DB.
---