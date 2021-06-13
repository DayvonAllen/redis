## List Data Types And Commands
- Redis lists are implemented via Linked Lists.
- Accessing an element by index is very fast in lists implemented via arrays and not so fast in list implemented vis linked lists.
- `lpush <key> <[values...]>` - insert an element from the left. If you use lpush then the last element pushed will always be at the beginning(head) like `unshift()` in JavaScript.
  - `lpush names john sally harold`
- `lrange <key> <start> <stop>` - prints the values of a list based on start and stop values
  - Ex. `lrange names 0 2` - prints the 0,1 and 2 index. -1 is the last element, if you want to get everything use `lrange names 0 -1`
- `rpush <key> <[values...]>` - the opposite of `lpush`, the last value pushed will always be at the end(tail), like `push()` in JavaScript.
- `rpushx <key> <value>` - you can only insert the value if the key exists.
- `lpushx <key> <value>` - you can only insert the value if the key exists.
- `rpop <key>` - will delete the tail element(the last value in your list) and return the element.
  - Ex. `rpop names`
- `lpop <key>` - will delete the head element(the first element in the linked list) and return it.
- `ltrim <key> <start> <stop>` - It will delete everything in the list that is not specified in the 'start' and 'stop' range.
  - Ex. `ltrim numbers 0 2` - If numbers contained `[0, 1, 2, 3, 4, 5]` it will turn into `[0, 1, 2]`
- `lset <key> <index> <value>` - it will set the element at a specified index.
  - Ex. `lset names 0 Harry`
- `lindex <key> <index>` - it returns the value at a specified index location
  - Ex. `lindex names 0`
- `linsert <key> [BEFORE|AFTER] <pivot(reference value in the list)> <value>` - inserts a value in a list either before or after the reference value(pivot, which is already in the list).
  - Ex. `linsert names before john  blake` - Starting list: `['john', 'harold', 'beth']`, after insert: `['blake', 'john', 'harold', 'beth']`
  - Ex. `linsert names after john  blake` - Starting list: `['john', 'harold', 'beth']`, after insert: `['john', 'blake', 'harold', 'beth']`
- `llen <key>` - returns the total length of the list.
- `lrem <key> <count> <value>` - this removes the number of occurences(specified by `count`) of an element equal to the 'value' stored in the list(key) .
- 'lrem' rules:
  1. If `count > 0`: remove elements equal to the value moving from the head to the tail.
     - Ex. `lrem names 1 john` - Starting list: `['john', 'harold', 'beth']`, after rem: `['harold', 'beth']`. One occurrence of 'john' was removed. The elements will be removed from left to right in the list.
  2. If `count < 0`: remove elements equal to the value moving from right to left
     - Ex. `lrem names -1 john` - Starting list: `['john', 'harold', 'beth', 'john']`, after rem: `['john', 'harold', 'beth']`. One occurrence of 'john' was removed. The elements will be removed from right to left in this case in the list because the `count` is a negative number.
  3. If `count == 0`: Remove all elements equal to the value in the list:
     - Ex. `lrem names 0 john` - Starting list: `['john', 'harold', 'beth', 'john']`, after rem: `['harold', 'beth']`. All occurrences of 'john' were removed.
---