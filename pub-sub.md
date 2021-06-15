## Commands
- `publish <channel name> <msg>`
- `subscribe [channels...]`
- `pubsub` - Lets you do administrative tasks. You can check the number of subscribers, the number of pattern subscriptions
  - `pubsub Numsub <channel>` - checks number of subscribers for a channel.
  - `pubsub numpat` - will find the number of pattern subscriptions
---

## Patterned  Subscription
- `psubscribe [pattern...]`
  - `psubscribe ch*` - will subscribe to any channel that starts with "ch"
---