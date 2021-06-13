## Redis Hashes
- Redis Hashes are maps between string fields and string values.
- They are good for representing objects.
- Every hash can store up to 4 billion(2^32) field and value pairs.
---

## Hash Commands
- `hset <key> <field> <value>` - creates a hash under a key and sets the field and value.
  - Ex. `hset student name John`
- `hget <key> <field>` - get a value from the hash
  - Ex. `hget student name` - returns value.
- If you update a field in a hash, instead of creating a new one, `0` will be returned
  - Ex. `hset student name Bob` - since name already exists as a field, it will update the name to 'Bob' and return 0.
- `hmset <key> <field> [field value...]` - allows you create multiple field and values at a time.
  - Ex. `hmset student name Beth age 21`
- `hgetall <key>` - gets all of the field and value pairs 
- `hmget <key> [field...]` - allows you to fetch values from multiple fields.
  - Ex. `hmget student name age`
- `hvals <key>` - you can extract all of the values from a key
  - `hvals student`
- `hkeys <key>` - you can extract all of the fields from a key
  - `hkeys student`
- `hexists <key> <field>` - allows you to check whether a field exists in our hash or not. Return 1 if exists and 0 if it doesn't
- `hlen <key>` - returns the number of fields in the hash.
- `hsetnx <key> <field> <value>` - creates a new field if it doesn't exist and does nothing if a field already exists.
- `hdel <key> [field...]` - deletes one or multiple fields from your hash.
- `hincrby <key> <field> <count>` - only works with integers. Increments by the count
- `hincrbyfloat <key> <field> <count>` - works with floats and integers. Increments by the count
- `hstrlen <key> <field>` - gets the string's length of a value that's associated with a field.
---