## HyperLogLog
- It allows you to count a unique number of items in a more efficient way than a set.
- It keeps a counter of items that is incremented when new items are added that have not been previously added.
- The main use is to check for unique data like IP's(for determining unique users on your site).
- This provides a very low error rate when estimating the unique items(cardinality) of a set.
- You can't view data in a hyperloglog, it just keeps count of unique elements.
- 
---

## Commands
- `pfadd <key> [element...]` - creates a hyperloglog
  - `pfadd website Alex Bob Alex Rob` - If there is a change in the cardinality then this command will return 1 otherwise it returns 0.
- `pfcount [key...]` - counts the elements in the hyperloglog
  - `pfadd website Alex Bob Alex Rob` 
  - `pfcount website` - returns 3
- `pfmerge <destkey> [sourcekey...]` - You can use this to merge multiple hyperloglogs together.
  - `pfmerge website "new website"`
---