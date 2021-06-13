## Redis Basic Commands
- `docker exec -it <redis container name> redis-cli` allows for you to interact with redis.
- `set <key> <value> [optional parameters]` store a key value pair into redis. If a value is already set then it will be overwritten.
  - Ex. `set word "hello"` stores 'word' as the key and "hello" as the value.
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
---

## Redis KEYS Command
- In Redis if you use the KEYS command, you can return all the keys that match a pattern.
- Supported glob-style patterns:
  - `h?llo`, this matches hello, hallo and hxllo. `?` means any one character can be inserted there. The length of the key must be equal to `h?llo`
  - `h*llo`, this matches hllo, heeeello. Any number of characters(even no character) can be placed between 'h' and 'llo'. The length of the key does not have to be equal to `h?llo`
  - `h[ae]llo`, this matches hello and hallo, but not hillo. Either 'e' or 'a' can be inserted between 'h' and 'llo'.
  - `h[^e]llo`, this matches hallo, hbllo but not hello. This means anything but 'e' can be inserted between 'h' and 'llo'.
  - `h[a-b]llo`, this matches hallo and hbllo. This means any letters between 'a' and 'b' can be inserted between 'h' and 'llo'. 
- `keys <pattern>`
  - Ex. `keys name?`
  - Ex. `keys n*me`
  - Ex. `keys n[ae]me`
  - Ex. `keys *` - gets all the keys stored in our database.
---