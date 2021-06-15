## Redis Streams
- It's a data structure.
- It acts as an append only log(you can only store data at the end of it).
- Each entry in a stream is structured as a set of key-value pairs.
- Every stream-entry has unqiue IDs. By default,IDs are time prefixes(you can query data based on date, month and year). This is a benefit over Redis Hashes. 
- There are consumers(who consume data) and producers(who produce the data)
- Streams can be consumed by and processed by multiple distinct sets of consumers known as consumer groups.
  - Consumer groups make sure that multiple instances of the same application does not try to consume the same data, if one of them does it, the others will not consume it.
- The difference between this and `pubsub` is that you can permanently save streams where as that's no the case with `pubsub`.
---

## Redis Producer API
- It only contains one command: `xadd <key> <ID> <field> [value...]`
  - `xadd temperature * temp_f 43.7 location_id 3020 tempf_ 59.6 location_id 2050` - the `*` means that the redis server is going to generate the ID for you.
    - This will return the generated ID. `"1623717557010-0"` it will look something like that. The numbers before the "-" Is timestamp in milliseconds which is an unsigned 64 bit integer, the part after the "-" is an internal sequence kept by redis that provides some millisecond resolution because it is possible for multiple messages to be added inside the span of a single message. Let say you added 100 messages in 1 millisecond, then that number will increase from `0`
    - Message IDs are immutable, once created you cannot change them. They are also bound to the same location in the stream, so if the ID was create at the first location then it will always be at the first location in the stream no matter what.
    - `10 xadd temperature * temp_f 43.7 location_id 3020 tempf_ 59.6 location_id 2050` - The `10` will make this command get executed 10 times.
    - `xadd myTemperature 123-1 foo bar`- can give any integer for the ID but it should follow this format `10202-1` or `250`, it will automatically append a `-0` or something of that naturally after a whole integer that does not specify `-` this.
      - Ex. `250` will turn to `250-0`
    - The ID number must always be greater than the ID that precedes it, EX. `1-0`. `0-0`(error), `2-0`(good)
- `xrange <key> <start> <end> <count>` - access data in a stream
  - `xrange temperature - +` - gets everything in the stream. `-` means beginning, `+` means the end.
  - `xrange temperature 0 + COUNT 5` - returns the first 5
- `xrevrange <key> <start> <end> <count>` - starts from the end of the stream.
- The data stored in the value of a stream is a string, so you can store about 500 MB of data in a single value.
- Streams are 0(1) time complexity for accessing data. Always a sub millisecond latency for accessing data in stream no matter how big it is.
---

## Commands
- `xlen <key>` - get the length of your stream
- `exists <key>`
- `xdel <key> <value>`
- `xtrim maxlen <number> ` - trims numbers from your stream and deletes the rest. You can use `[~]` to tell redis to optimize for efficient memory management.
- `xread <count> <Block ms> streams [key...] [id...]` - is a replacement for `xrange` but you can subscribe and get new items that arrive in the stream.
  - `xread COUNT 3 STREAMS numbers 0-0` - It expects the last message ID as the final argument. Gets 3 elements from the stream.
  - `xread COUNT 3 block 1000 streams numbers 0-0` - blocks for 1000 ms. If messages don't arrive in 1000 ms then the consumer will timeout
  - `xread COUNT 3 block 1000 streams numbers $` - `$` means the last message ID. You should only use that dollar symbol once(according to the docs)
- `xgroup create <key> <group name> <id(starting point of the stream)>` - create consumer group
  - `xgroup create numbers group0 0` - creates a consumer group from ID 0(beginning of the stream)
  - `xgroup create numbers group0 0 mkstream` - makes a stream if it doesn't exists.
- `xinfo groups <key>` - get group info.
- `xinfo consumers <key> <consumer name>` - get consumer info.
- `xgroup destroy <key> <group name>` - destroys a group.
- `xreadgroup group <group name> <consumer name> count <number> block <milliseconds> streams <key> >` - allows you to read data from a group, creates a consumer. `>` means the message ID that is greater than all of the other message IDs(so start reading from the next message to be delivered). If you use `0` instead of `>`, the consumer will read all of the messages from the stream.
- `xack <key> <group name> <message id>` - acknowledge that a message has been processed.
---
