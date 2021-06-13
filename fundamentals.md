## Redis
- Redis is an open source in-memory data structure store, used as a database, cache and message broker.
  - Cache- it's a temporary storage component area where data is stored so it can be served faster in the future.
- It's a NoSQL database which follows the principle of key-value store.
- Redis holds its database entirely in memory, using the disk for persistence only. It does not access data from the disk, it's only used for storage, if you choose that option.
  - When redis goes down, you have the option to save the data on your disk(SSD or HDD). 
  - The reason why the data is stored and accessed in memory is because RAM is faster than SSD and HDD to access.
- It supports the following data structures:
  - strings
  - hashes
  - lists
  - sets
  - sorted sets with range queries
  - bitmaps
  - hyperloglogs
- Redis is written in ANSI C.
- It's supported in most programming languages.
- It works only on Linux and Mac.
---