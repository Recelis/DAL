# SQS
SQS is a way to pass data in the same order. 

# Dead Letter Queue (DLQ)
[docs](https://aws.amazon.com/what-is/dead-letter-queue/)
These are queues that temporarily store erroneous messages so that they can be picked up and analysed at a different time.

If a message is continuously erroneous, processing them over and over again until they expire is just a waste of money. Better to put them in the DLQ.

Also can allow developers to see only the erroneous messages instead of the thousands of other non-offending messages.

## Redrive Policy
This is the policy of rules that determine when software  should move messages to the DLQ i.e. maximum retry count.



