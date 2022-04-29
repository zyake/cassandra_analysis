# Comparison Between Quorum Model and Single Leader Model

## Comparison Matrix

||   | Quorum(Cassandra)  | CAS(Cassandra) | Single Leader(DynamoDB)  |
|---|---|---|---|---|
|Linearizability| Linearizability Support | No(with failure and/or Concurrent Write) | - | Yes(Write after Strong Consistent Read) |
|Read| Stronger Single Node Read Consistency | No(Dirty Read)  |-| - | Yes(may not be up to date, but no Dirty Read) |
|| Strong Consistent Read Support | No(Read Quorum cannot ensure Strong Consistent Read even with foreground Read Repair) | - | Yes(Single Leader ensures Strong Consistent Read) |
|| Read Effeciency | No(must read from multiple nodes) |  - | Yes(No Read Quorum) |
|Write| Strong Write Consistency | No(may incur Dirty Data) |  Yes | Yes |
|| Write Effeciency | Yes(Single RT per node, but no Strong Consistency Support) | No(4 round trip due to design(one RT) and implementation(another one RT)) | Yes(minimum 2 round trip) |
|Availability| Availability of a partition | Yes(Multi Writer) | Yes(Multi Writer) | No(Single Leader per partition is a SPoF) | 

## References

https://blog.yugabyte.com/apache-cassandra-lightweight-transactions-secondary-indexes-tunable-consistency/
https://cassandra.apache.org/doc/latest/cassandra/operating/read_repair.html

>The data returned even after a read repair has been performed may not be the most up-to-date data if consistency level is other than one requiring response from all replicas.

https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html

>When you request a strongly consistent read, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful.

https://aws.amazon.com/builders-library/leader-election-in-distributed-systems/

>Leader election is a widely deployed pattern across Amazon. For example:
>• Amazon EBS distributes reads and writes for a volume over many storage servers. To ensure consistency, it uses leader election to elect primaries for each area of the volume, which order the reads and writes. If that primary fails, follower copies steps in using the same leader election mechanism. In Amazon EBS, leader election ensures consistency, while improving performance by avoiding coordination on the data plane. DynamoDB, Amazon Quantum Ledger Database (Amazon QLDB), and Amazon Kinesis (Kinesis) use similar approaches for the same reason.

https://youtu.be/yvBR71D0nAQ?t=345

>AWS re:Invent 2018: Amazon DynamoDB Under the Hood: How We Built a Hyper-Scale Database (DAT321)