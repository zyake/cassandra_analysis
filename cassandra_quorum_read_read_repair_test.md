# Cassandra Quorum Read and Read Repair Test

## Lcal Quorum Read(One Fast Path Read + One Digest)

    cqlsh:tutorialspoint> consistency local_quorum
    Consistency level set to LOCAL_QUORUM.
    cqlsh:tutorialspoint> SELECT * FROM test WHERe id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)

    Tracing session: 149483b0-c7b8-11ec-b3a8-f1ff27734e85

    activity                                                                                    | timestamp                  | source    | source_elapsed | client
    ---------------------------------------------------------------------------------------------+----------------------------+-----------+----------------+-----------
                                                                            Execute CQL3 query | 2022-04-29 21:29:57.486000 | 127.0.0.1 |              0 | 127.0.0.1
                        Parsing SELECT * FROM test WHERe id = 1; [Native-Transport-Requests-1] | 2022-04-29 21:29:57.491000 | 127.0.0.1 |           5287 | 127.0.0.1
                                            Preparing statement [Native-Transport-Requests-1] | 2022-04-29 21:29:57.492000 | 127.0.0.1 |           6526 | 127.0.0.1
                                reading data from /127.0.0.1:8100 [Native-Transport-Requests-1] | 2022-04-29 21:29:57.500000 | 127.0.0.1 |          14820 | 127.0.0.1
                            reading digest from /127.0.0.1:9000 [Native-Transport-Requests-1] | 2022-04-29 21:29:57.503000 | 127.0.0.1 |          17043 | 127.0.0.1
    Sending READ_REQ message to /127.0.0.1:8100 message size 79 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:29:57.503000 | 127.0.0.1 |          17142 | 127.0.0.1
    Sending READ_REQ message to /127.0.0.1:9000 message size 80 bytes [Messaging-EventLoop-3-4] | 2022-04-29 21:29:57.503000 | 127.0.0.1 |          17873 | 127.0.0.1
                        READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-2] | 2022-04-29 21:29:57.506000 | 127.0.0.1 |             95 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-2] | 2022-04-29 21:29:57.507000 | 127.0.0.1 |           1553 | 127.0.0.1
                        READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-8] | 2022-04-29 21:29:57.507000 | 127.0.0.1 |            572 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-2] | 2022-04-29 21:29:57.507000 | 127.0.0.1 |           1951 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-2] | 2022-04-29 21:29:57.508000 | 127.0.0.1 |           2229 | 127.0.0.1
                                Partition index with 0 entries found for sstable 1 [ReadStage-2] | 2022-04-29 21:29:57.510000 | 127.0.0.1 |           4703 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-2] | 2022-04-29 21:29:57.513000 | 127.0.0.1 |           7214 | 127.0.0.1
                                            Enqueuing response to /127.0.0.1:7000 [ReadStage-2] | 2022-04-29 21:29:57.513000 | 127.0.0.1 |           7754 | 127.0.0.1
    Sending READ_RSP message to /127.0.0.1:7000 message size 50 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:29:57.514000 | 127.0.0.1 |           8400 | 127.0.0.1
                        READ_RSP message received from /127.0.0.1:9000 [Messaging-EventLoop-3-3] | 2022-04-29 21:29:57.517000 | 127.0.0.1 |          31707 | 127.0.0.1
                            Processing response from /127.0.0.1:9000 [RequestResponseStage-2] | 2022-04-29 21:29:57.518000 | 127.0.0.1 |          32628 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-1] | 2022-04-29 21:29:57.518000 | 127.0.0.1 |          11223 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-1] | 2022-04-29 21:29:57.518000 | 127.0.0.1 |          11674 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-1] | 2022-04-29 21:29:57.518000 | 127.0.0.1 |          11979 | 127.0.0.1
                                Partition index with 0 entries found for sstable 5 [ReadStage-1] | 2022-04-29 21:29:57.519000 | 127.0.0.1 |          12581 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 21:29:57.520000 | 127.0.0.1 |          13749 | 127.0.0.1
                                            Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-29 21:29:57.520000 | 127.0.0.1 |          14060 | 127.0.0.1
    Sending READ_RSP message to /127.0.0.1:7000 message size 64 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:29:57.521000 | 127.0.0.1 |          14746 | 127.0.0.1
                        READ_RSP message received from /127.0.0.1:8100 [Messaging-EventLoop-3-8] | 2022-04-29 21:29:57.522000 | 127.0.0.1 |          36700 | 127.0.0.1
                            Processing response from /127.0.0.1:8100 [RequestResponseStage-2] | 2022-04-29 21:29:57.523000 | 127.0.0.1 |          37413 | 127.0.0.1
                                                                                Request complete | 2022-04-29 21:29:57.535315 | 127.0.0.1 |          49315 | 127.0.0.1

## Local Quorum Read(one node down)

    zyake@DESKTOP-P3FQ6DK:~$ ./apache-cassandra-4.0.3-1/bin/nodetool -pp describecluster
    Cluster Information:
            Name: Test Cluster
            Snitch: org.apache.cassandra.locator.SimpleSnitch
            DynamicEndPointSnitch: enabled
            Partitioner: org.apache.cassandra.dht.Murmur3Partitioner
            Schema versions:
                    bbcf35fa-5d65-3137-896f-a2866efe3856: [127.0.0.1:9000, 127.0.0.1:7000]

                    UNREACHABLE: [127.0.0.1:8100]

    Stats for all nodes:
            Live: 2
            Joining: 0
            Moving: 0
            Leaving: 0
            Unreachable: 1

    Data Centers:
            datacenter1 #Nodes: 3 #Down: 0

    Database versions:
            4.0.3: [127.0.0.1:9000, 127.0.0.1:7000, 127.0.0.1:8100]

    Keyspaces:
            system_schema -> Replication class: LocalStrategy {}
            system -> Replication class: LocalStrategy {}
            system_auth -> Replication class: SimpleStrategy {replication_factor=1}
            tutorialspoint -> Replication class: SimpleStrategy {replication_factor=3}
            system_distributed -> Replication class: SimpleStrategy {replication_factor=3}
            system_traces -> Replication class: SimpleStrategy {replication_factor=2}

    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)
    Unable to fetch query trace: Error from server: code=1000 [Unavailable exception] message="Cannot achieve consistency level LOCAL_QUORUM" info={'consistency': 'LOCAL_QUORUM', 'required_replicas': 2, 'alive_replicas': 1}
    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)

    Tracing session: 7bbaf880-c7bd-11ec-94bc-39f3b99b6eb5

    activity                                                                                    | timestamp                  | source    | source_elapsed | client
    ---------------------------------------------------------------------------------------------+----------------------------+-----------+----------------+-----------
                                                                            Execute CQL3 query | 2022-04-29 22:08:38.024000 | 127.0.0.1 |              0 | 127.0.0.1
                        Parsing SELECT * FROM test WHERE id = 1; [Native-Transport-Requests-1] | 2022-04-29 22:08:38.024000 | 127.0.0.1 |            430 | 127.0.0.1
                                            Preparing statement [Native-Transport-Requests-1] | 2022-04-29 22:08:38.025000 | 127.0.0.1 |            823 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-3] | 2022-04-29 22:08:38.025000 | 127.0.0.1 |           1741 | 127.0.0.1
                            reading digest from /127.0.0.1:7000 [Native-Transport-Requests-1] | 2022-04-29 22:08:38.025000 | 127.0.0.1 |           1654 | 127.0.0.1
    Sending READ_REQ message to /127.0.0.1:7000 message size 80 bytes [Messaging-EventLoop-3-1] | 2022-04-29 22:08:38.026000 | 127.0.0.1 |           2355 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-3] | 2022-04-29 22:08:38.026000 | 127.0.0.1 |           2761 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-3] | 2022-04-29 22:08:38.027000 | 127.0.0.1 |           3144 | 127.0.0.1
                        READ_REQ message received from /127.0.0.1:9000 [Messaging-EventLoop-3-8] | 2022-04-29 22:08:38.027000 | 127.0.0.1 |             90 | 127.0.0.1
                                                    Key cache hit for sstable 1 [ReadStage-3] | 2022-04-29 22:08:38.027000 | 127.0.0.1 |           3507 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-3] | 2022-04-29 22:08:38.028000 | 127.0.0.1 |           4272 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-1] | 2022-04-29 22:08:38.029000 | 127.0.0.1 |           1501 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-1] | 2022-04-29 22:08:38.029000 | 127.0.0.1 |           1833 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-1] | 2022-04-29 22:08:38.029000 | 127.0.0.1 |           2010 | 127.0.0.1
                                            Bloom filter allows skipping sstable 2 [ReadStage-1] | 2022-04-29 22:08:38.029000 | 127.0.0.1 |           2243 | 127.0.0.1
                                                    Key cache hit for sstable 1 [ReadStage-1] | 2022-04-29 22:08:38.030000 | 127.0.0.1 |           2569 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 22:08:38.031000 | 127.0.0.1 |           3785 | 127.0.0.1
                                            Enqueuing response to /127.0.0.1:9000 [ReadStage-1] | 2022-04-29 22:08:38.031000 | 127.0.0.1 |           4032 | 127.0.0.1
    Sending READ_RSP message to /127.0.0.1:9000 message size 50 bytes [Messaging-EventLoop-3-4] | 2022-04-29 22:08:38.031000 | 127.0.0.1 |           4309 | 127.0.0.1
                        READ_RSP message received from /127.0.0.1:7000 [Messaging-EventLoop-3-2] | 2022-04-29 22:08:38.032000 | 127.0.0.1 |           8274 | 127.0.0.1
                            Processing response from /127.0.0.1:7000 [RequestResponseStage-2] | 2022-04-29 22:08:38.033000 | 127.0.0.1 |           8965 | 127.0.0.1
                                                                                Request complete | 2022-04-29 22:08:38.034958 | 127.0.0.1 |          10958 | 127.0.0.1
    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)

    Tracing session: a35de9b0-c7bd-11ec-b3a8-f1ff27734e85

    activity                                                                                    | timestamp                  | source    | source_elapsed | client
    ---------------------------------------------------------------------------------------------+----------------------------+-----------+----------------+-----------
                                                                            Execute CQL3 query | 2022-04-29 22:09:44.523000 | 127.0.0.1 |              0 | 127.0.0.1
                        Parsing SELECT * FROM test WHERE id = 1; [Native-Transport-Requests-1] | 2022-04-29 22:09:44.524000 | 127.0.0.1 |            714 | 127.0.0.1
                                            Preparing statement [Native-Transport-Requests-1] | 2022-04-29 22:09:44.525000 | 127.0.0.1 |           1790 | 127.0.0.1
                            reading digest from /127.0.0.1:9000 [Native-Transport-Requests-1] | 2022-04-29 22:09:44.526000 | 127.0.0.1 |           3144 | 127.0.0.1
                        READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |             80 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           3413 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           3601 | 127.0.0.1
    Sending READ_REQ message to /127.0.0.1:9000 message size 80 bytes [Messaging-EventLoop-3-4] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           3601 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           3738 | 127.0.0.1
                                            Bloom filter allows skipping sstable 2 [ReadStage-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           3980 | 127.0.0.1
                                                    Key cache hit for sstable 1 [ReadStage-2] | 2022-04-29 22:09:44.527000 | 127.0.0.1 |           4276 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-2] | 2022-04-29 22:09:44.528000 | 127.0.0.1 |            605 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-2] | 2022-04-29 22:09:44.528000 | 127.0.0.1 |            779 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-2] | 2022-04-29 22:09:44.528000 | 127.0.0.1 |            912 | 127.0.0.1
                                                    Key cache hit for sstable 1 [ReadStage-2] | 2022-04-29 22:09:44.529000 | 127.0.0.1 |           1271 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-2] | 2022-04-29 22:09:44.529000 | 127.0.0.1 |           5633 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-2] | 2022-04-29 22:09:44.530000 | 127.0.0.1 |           3041 | 127.0.0.1
                                            Enqueuing response to /127.0.0.1:7000 [ReadStage-2] | 2022-04-29 22:09:44.530000 | 127.0.0.1 |           3222 | 127.0.0.1
    Sending READ_RSP message to /127.0.0.1:7000 message size 50 bytes [Messaging-EventLoop-3-1] | 2022-04-29 22:09:44.531000 | 127.0.0.1 |           3885 | 127.0.0.1
                        READ_RSP message received from /127.0.0.1:9000 [Messaging-EventLoop-3-8] | 2022-04-29 22:09:44.532000 | 127.0.0.1 |           8459 | 127.0.0.1
                            Processing response from /127.0.0.1:9000 [RequestResponseStage-3] | 2022-04-29 22:09:44.532000 | 127.0.0.1 |           8775 | 127.0.0.1
                                                                                Request complete | 2022-04-29 22:09:44.533471 | 127.0.0.1 |          10471 | 127.0.0.1


    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)
    Unable to fetch query trace: Error from server: code=1000 [Unavailable exception] message="Cannot achieve consistency level LOCAL_QUORUM" info={'consistency': 'LOCAL_QUORUM', 'required_replicas': 2, 'alive_replicas': 1}
    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

### Default Snitch Configuration

    # You can use a custom Snitch by setting this to the full class name
    endpoint_snitch: SimpleSnitch

    /**
    * A simple endpoint snitch implementation that treats Strategy order as proximity,
    * allowing non-read-repaired reads to prefer a single endpoint, which improves
    * cache locality.
    */
    public class SimpleSnitch extends AbstractEndpointSnitch
    ...
    @Override
    public <C extends ReplicaCollection<? extends C>> C sortedByProximity(final InetAddressAndPort address, C unsortedAddress)
    {
        // Optimization to avoid walking the list
        return unsortedAddress;
    }

    @Override
    public int compareEndpoints(InetAddressAndPort target, Replica r1, Replica r2)
    {
        // Making all endpoints equal ensures we won't change the original ordering (since
        // Collections.sort is guaranteed to be stable)
        return 0;
    }

### ReplicaCache(NonblockingHashMap)

    static class ReplicaCache<K, V>
    {
        private final AtomicReference<ReplicaHolder<K, V>> cachedReplicas = new AtomicReference<>(new ReplicaHolder<>(0, 4));


    static class ReplicaHolder<K, V>
    {
        private final long ringVersion;
        private final NonBlockingHashMap<K, V> replicas;

=> Unordered


## Blocking Read Repair(Full Data Request)

    cqlsh:tutorialspoint> consistency local_quorum
    Consistency level set to LOCAL_QUORUM.
    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;

    id | id2
    ----+-----
    1 |   1

    (1 rows)

    Tracing session: 2be316b0-c7ba-11ec-b3a8-f1ff27734e85

    activity                                                                                                                           | timestamp                  | source    | source_elapsed | client
    ------------------------------------------------------------------------------------------------------------------------------------+----------------------------+-----------+----------------+-----------
                                                                                                                    Execute CQL3 query | 2022-04-29 21:44:55.581000 | 127.0.0.1 |              0 | 127.0.0.1
                                                                Parsing SELECT * FROM test WHERE id = 1; [Native-Transport-Requests-1] | 2022-04-29 21:44:55.586000 | 127.0.0.1 |           6112 | 127.0.0.1
                                                                                    Preparing statement [Native-Transport-Requests-1] | 2022-04-29 21:44:55.605000 | 127.0.0.1 |          24732 | 127.0.0.1
                                                                        reading data from /127.0.0.1:8100 [Native-Transport-Requests-1] | 2022-04-29 21:44:55.615000 | 127.0.0.1 |          35431 | 127.0.0.1
                                            Sending READ_REQ message to /127.0.0.1:8100 message size 79 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:44:55.618000 | 127.0.0.1 |          38604 | 127.0.0.1
                                                                    reading digest from /127.0.0.1:9000 [Native-Transport-Requests-1] | 2022-04-29 21:44:55.619000 | 127.0.0.1 |          39049 | 127.0.0.1
                                            Sending READ_REQ message to /127.0.0.1:9000 message size 80 bytes [Messaging-EventLoop-3-4] | 2022-04-29 21:44:55.621000 | 127.0.0.1 |          40633 | 127.0.0.1
                                                            READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-8] | 2022-04-29 21:44:55.624000 | 127.0.0.1 |            213 | 127.0.0.1
                                                                                Executing single-partition query on test [ReadStage-1] | 2022-04-29 21:44:55.629000 | 127.0.0.1 |           4965 | 127.0.0.1
                                                                                            Acquiring sstable references [ReadStage-1] | 2022-04-29 21:44:55.629000 | 127.0.0.1 |           5497 | 127.0.0.1
                                                                                                Merging memtable contents [ReadStage-1] | 2022-04-29 21:44:55.630000 | 127.0.0.1 |           5824 | 127.0.0.1
                                                                                            Key cache hit for sstable 5 [ReadStage-1] | 2022-04-29 21:44:55.635000 | 127.0.0.1 |          10980 | 127.0.0.1
                                                            READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-2] | 2022-04-29 21:44:55.637000 | 127.0.0.1 |           1412 | 127.0.0.1
    speculating read retry on Full(localhost/127.0.0.1:7000,(-4315590310475478861,-3838100728625535792]) [Native-Transport-Requests-1] | 2022-04-29 21:44:55.642000 | 127.0.0.1 |          62102 | 127.0.0.1
                                                            Executing single-partition query on peers_v2 [Native-Transport-Requests-1] | 2022-04-29 21:44:55.645000 | 127.0.0.1 |          64862 | 127.0.0.1
                                                                            Acquiring sstable references [Native-Transport-Requests-1] | 2022-04-29 21:44:55.645000 | 127.0.0.1 |          65251 | 127.0.0.1
                                                                                Merging memtable contents [Native-Transport-Requests-1] | 2022-04-29 21:44:55.645000 | 127.0.0.1 |          65583 | 127.0.0.1
                                                                                Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 21:44:55.648000 | 127.0.0.1 |          23850 | 127.0.0.1
                                                                                    Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-29 21:44:55.650000 | 127.0.0.1 |          24344 | 127.0.0.1
                                            Sending READ_RSP message to /127.0.0.1:7000 message size 64 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:44:55.652000 | 127.0.0.1 |          28363 | 127.0.0.1
                                                            READ_RSP message received from /127.0.0.1:8100 [Messaging-EventLoop-3-7] | 2022-04-29 21:44:55.654000 | 127.0.0.1 |          74431 | 127.0.0.1
                                                                    Processing response from /127.0.0.1:8100 [RequestResponseStage-4] | 2022-04-29 21:44:55.655000 | 127.0.0.1 |          75163 | 127.0.0.1
                                                                            Key cache hit for sstable 30 [Native-Transport-Requests-1] | 2022-04-29 21:44:55.662000 | 127.0.0.1 |          82441 | 127.0.0.1
                                                                                Executing single-partition query on test [ReadStage-1] | 2022-04-29 21:44:55.663000 | 127.0.0.1 |          28970 | 127.0.0.1
                                                                                            Acquiring sstable references [ReadStage-1] | 2022-04-29 21:44:55.664000 | 127.0.0.1 |          29539 | 127.0.0.1
                                                                                                Merging memtable contents [ReadStage-1] | 2022-04-29 21:44:55.664000 | 127.0.0.1 |          29868 | 127.0.0.1
                                                                                            Key cache hit for sstable 1 [ReadStage-1] | 2022-04-29 21:44:55.669000 | 127.0.0.1 |          35226 | 127.0.0.1
                                                                Read 0 live rows and 0 tombstone cells [Native-Transport-Requests-1] | 2022-04-29 21:44:55.669000 | 127.0.0.1 |          89582 | 127.0.0.1
                                                                                Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 21:44:55.692000 | 127.0.0.1 |          57816 | 127.0.0.1
                                                                                    Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-29 21:44:55.692000 | 127.0.0.1 |          58153 | 127.0.0.1
                                            Sending READ_RSP message to /127.0.0.1:7000 message size 50 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:44:55.694000 | 127.0.0.1 |          59734 | 127.0.0.1
                                                            READ_RSP message received from /127.0.0.1:9000 [Messaging-EventLoop-3-8] | 2022-04-29 21:45:00.389000 | 127.0.0.1 |        4808649 | 127.0.0.1
                                                                    Processing response from /127.0.0.1:9000 [RequestResponseStage-4] | 2022-04-29 21:45:00.390000 | 127.0.0.1 |        4810398 | 127.0.0.1
                        Digest mismatch: Mismatch for key DecoratedKey(-4069959284402364209, 00000001) [Native-Transport-Requests-1] | 2022-04-29 21:47:01.451000 | 127.0.0.1 |      125869050 | 127.0.0.1
            Enqueuing full data read to Full(/127.0.0.1:8100,(-4315590310475478861,-3838100728625535792]) [Native-Transport-Requests-1] | 2022-04-29 21:47:01.495000 | 127.0.0.1 |      125915275 | 127.0.0.1
                                            Sending READ_REQ message to /127.0.0.1:8100 message size 79 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:47:01.499000 | 127.0.0.1 |      125918949 | 127.0.0.1
            Enqueuing full data read to Full(/127.0.0.1:9000,(-4315590310475478861,-3838100728625535792]) [Native-Transport-Requests-1] | 2022-04-29 21:47:01.499000 | 127.0.0.1 |      125919246 | 127.0.0.1
                                                            READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-8] | 2022-04-29 21:47:01.500000 | 127.0.0.1 |             26 | 127.0.0.1
                                                                                Executing single-partition query on test [ReadStage-1] | 2022-04-29 21:47:01.501000 | 127.0.0.1 |            911 | 127.0.0.1
                                                                                            Acquiring sstable references [ReadStage-1] | 2022-04-29 21:47:01.501000 | 127.0.0.1 |           1271 | 127.0.0.1
                                                                                                Merging memtable contents [ReadStage-1] | 2022-04-29 21:47:01.502000 | 127.0.0.1 |           1608 | 127.0.0.1
                                            Sending READ_REQ message to /127.0.0.1:9000 message size 79 bytes [Messaging-EventLoop-3-4] | 2022-04-29 21:47:01.503000 | 127.0.0.1 |      125923325 | 127.0.0.1
                                                            READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-2] | 2022-04-29 21:47:01.504000 | 127.0.0.1 |             31 | 127.0.0.1
                                                                                Executing single-partition query on test [ReadStage-4] | 2022-04-29 21:47:01.504000 | 127.0.0.1 |      125923949 | 127.0.0.1
                                                                                            Key cache hit for sstable 5 [ReadStage-1] | 2022-04-29 21:47:01.504000 | 127.0.0.1 |           3479 | 127.0.0.1
                                                                                Executing single-partition query on test [ReadStage-1] | 2022-04-29 21:47:01.505000 | 127.0.0.1 |            815 | 127.0.0.1
                                                                                            Acquiring sstable references [ReadStage-4] | 2022-04-29 21:47:01.505000 | 127.0.0.1 |      125925454 | 127.0.0.1
                                                                                Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 21:47:01.505000 | 127.0.0.1 |           4771 | 127.0.0.1
                                                                                            Acquiring sstable references [ReadStage-1] | 2022-04-29 21:47:01.505000 | 127.0.0.1 |           1164 | 127.0.0.1
                                                                                    Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-29 21:47:01.505000 | 127.0.0.1 |           5100 | 127.0.0.1
                                                                                                Merging memtable contents [ReadStage-1] | 2022-04-29 21:47:01.506000 | 127.0.0.1 |           1464 | 127.0.0.1
                                                                                                Merging memtable contents [ReadStage-4] | 2022-04-29 21:47:01.506000 | 127.0.0.1 |      125925842 | 127.0.0.1
                                                                                            Key cache hit for sstable 1 [ReadStage-1] | 2022-04-29 21:47:01.506000 | 127.0.0.1 |           2023 | 127.0.0.1
                                                                                Bloom filter allows skipping sstable 2 [ReadStage-4] | 2022-04-29 21:47:01.506000 | 127.0.0.1 |      125926210 | 127.0.0.1
                                                                                            Key cache hit for sstable 1 [ReadStage-4] | 2022-04-29 21:47:01.506000 | 127.0.0.1 |      125926691 | 127.0.0.1
                                            Sending READ_RSP message to /127.0.0.1:7000 message size 64 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:47:01.508000 | 127.0.0.1 |           8008 | 127.0.0.1
                                                                                Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-29 21:47:01.509000 | 127.0.0.1 |           4979 | 127.0.0.1
                                                                                Read 1 live rows and 0 tombstone cells [ReadStage-4] | 2022-04-29 21:47:01.509000 | 127.0.0.1 |      125929209 | 127.0.0.1
                                                            READ_RSP message received from /127.0.0.1:8100 [Messaging-EventLoop-3-1] | 2022-04-29 21:47:01.509000 | 127.0.0.1 |      125929380 | 127.0.0.1
                                                                                    Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-29 21:47:01.510000 | 127.0.0.1 |           5451 | 127.0.0.1
                                                                    Processing response from /127.0.0.1:8100 [RequestResponseStage-5] | 2022-04-29 21:47:01.512000 | 127.0.0.1 |      125930030 | 127.0.0.1
                                            Sending READ_RSP message to /127.0.0.1:7000 message size 64 bytes [Messaging-EventLoop-3-1] | 2022-04-29 21:47:01.515000 | 127.0.0.1 |          10866 | 127.0.0.1
                                                            READ_RSP message received from /127.0.0.1:9000 [Messaging-EventLoop-3-2] | 2022-04-29 21:47:01.519000 | 127.0.0.1 |      125939455 | 127.0.0.1
                                                                    Processing response from /127.0.0.1:9000 [RequestResponseStage-3] | 2022-04-29 21:47:01.521000 | 127.0.0.1 |      125940713 | 127.0.0.1
                                                                                                                    Request complete | 2022-04-29 21:47:01.579786 | 127.0.0.1 |      125998786 | 127.0.0.1

## Blocking Read Repair(one node down)

    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 1;
    ReadTimeout: Error from server: code=1200 [Coordinator node timed out waiting for replica nodes' responses] message="Operation timed out - received only 1 responses." info={'consistency': 'LOCAL_QUORUM', 'required_responses': 2, 'received_responses': 1}
