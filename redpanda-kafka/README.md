# kafkarepo
# What is Redpanda?
Redpanda is an event streaming platform: it provides the infrastructure for streaming real-time data.

Redpanda is a fault-tolerant transaction log for storing event streams
Producers and consumers interact with Redpanda using the Kafka API

# Tiered Storage
Redpanda Tiered Storage is a multi-tiered object storage solution 
Tiered Storage can be combined with "local storage" to provide "long-term data retention and disaster    recovery" on a per-topic basis

Consumers that read 
    from more "recent offsets" continue to read from "local storage",
    from "historical offsets" read from "object storage", 
    all with the same API

# Partitions
To scale topics, Redpanda shards them into one or more partitions that are distributed across the nodes in a cluster

This allows for concurrent writing and reading from multiple nodes
Events with the same key (like a stock ticker) are always routed to the same partition, and Redpanda guarantees the order of events at the partition level

Events with the same key (like a stock ticker) are always routed to the same partition, and Redpanda guarantees the order of events at the partition level

If a key is not specified, then events are sent to all topic-partitions in a round-robin fashion.

Consumers read events from a partition in the order that they were written

# Raft consensus algorithm
1. Data safety 
2. fault tolerance

The Raft consensus algorithm is used for data replication. 
Events written to a topic-partition are appended to a log file on disk
They can be replicated to other nodes in the cluster and appended to their copies of the log file on disk to prevent data loss in the event of failure

Every topic-partition forms a Raft group
 consisting of a "single elected leader" and "zero or more followers"

A Raft group can tolerate ƒ failures given 2ƒ+1 nodes
    5 nodes topic with replication factor 5
    If 2 nodes fails the topic remains fully operational 

A Raft is majority vote algoritham

The leader to acknowlege en a event is committed to topic partition, majority of replicas have written that event to copy of their logs
When a majority (quorum) of responses have been received, the leader can make the event available to consumers and acknowledge receipt of the event when acks=all (-1)




1. docker compose up
2. docker exec -it redpanda-0 rpk cluster info
3. docker exec -it redpanda-0 rpk topic create chat-room
4. ocker exec -it redpanda-0 rpk topic produce chat-room
