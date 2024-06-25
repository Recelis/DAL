+++
title = 'S3'
date = 2024-03-17T21:38:06+10:00
draft = true
+++

# SQS
SQS is a way to pass data in the same order. 

# Dead Letter Queue (DLQ)
[docs](https://aws.amazon.com/what-is/dead-letter-queue/)
These are queues that temporarily store errornenous messages so that they can be picked up and analysed at a different time.

If a message is continuously errorenous, processing them over and over again until they expire is just a waste of money. Better to put them in the DLQ.

Also can allow developers to see only the errorenous messages instead of the thousands of other non-offending messages.

## Redrive Policy
This is the policy of rules that determine when softwareshould move messages to the DLQ i.e. maximum retry count.



