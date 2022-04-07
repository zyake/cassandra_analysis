# Data Distribution Test

## Node configuration

    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-1/bin$ ./nodetool -pp status
    Datacenter: datacenter1
    =======================
    Status=Up/Down
    |/ State=Normal/Leaving/Joining/Moving
    --  Address         Load        Tokens  Owns (effective)  Host ID                               Rack
    UN  127.0.0.1:7000  180.61 KiB  16      48.8%             9147128a-dd9e-4909-966d-f849e6ce2206  rack1
    UN  127.0.0.1:8000  191.78 KiB  16      51.2%             373bdbea-ec2b-4647-aedb-6e31adda7732  rack1

## Creata keyspace and table

    cqlsh:tutorialspoint> CREATE KEYSPACE tutorialspoint WITH replication = {'class':'SimpleStrategy', 'replication_factor' : 1};
    cqlsh:tutorialspoint> CREATE TABLE TEST(id int primary key, name varchar);
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(1, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(2, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(3, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(4, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(5, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(6, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(7, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(8, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(9, 'test');
    cqlsh:tutorialspoint> INSERT INTO test(id, name) VALUES(10, 'test');

## Check tokens

    cqlsh:tutorialspoint> SELECT id, name, token(id) FROM test;

    id | name | system.token(id)
    ----+------+----------------------
    5 | test | -7509452495886106294
    10 | test | -6715243485458697746
    1 | test | -4069959284402364209
    8 | test | -3799847372828181882
    2 | test | -3248873570005575792
    4 | test | -2729420104000364805
    7 | test |  1634052884888577606
    6 | test |  2705480034054113608
    9 | test |  3728482343045213994
    3 | test |  9010454139840013625

## Get token ring

    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp ring

    Datacenter: datacenter1
    ==========
    Address         Rack        Status State   Load            Owns                Token
                                                                                9004567555828459807
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -8782219492892387546
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -8183755801935648719
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -7804074040376273470
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -7149455155260244352
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -6723168653979114962
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -6025439739552686341
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -5372966827404437078
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -4929397839317748052
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -4315590310475478861
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -3838100728625535792
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -2993724081134374036
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -2193895410473243752
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -1553455851231790796
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -935474819329590718
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -481955234891757984
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              128866170443472677
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              535896169850669031
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              1353568102463019825
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              1948870471423419075
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              2429323983688599364
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              3165239676399493005
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              3825934800936405104
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              4244612483847094663
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              4994889724115419483
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              5488132489527686635
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              6044013524444610571
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              6455496732606222134
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              6979926697236125991
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              7395564356807765871
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              8045501285120120772
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              8607244703479747016
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              9004567555828459807

    Warning: "nodetool ring" is used to output all the tokens of a node.
    To view status related info of a node use "nodetool status" instead.

=> 16 virtual nodes per physical node

## Get endpoints for tokens

    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 1
    127.0.0.1:7000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 2
    127.0.0.1:7000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 3
    127.0.0.1:7000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 4
    127.0.0.1:8000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 5
    127.0.0.1:8000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 6
    127.0.0.1:7000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 7
    127.0.0.1:8000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 8
    127.0.0.1:7000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 9
    127.0.0.1:8000
    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 10
    127.0.0.1:7000

## Comparing token ring and endpoint

### pkey=1

    id | name | system.token(id)
    ----+------+----------------------
    1 | test | -4069959284402364209

    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -4315590310475478861
    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -3838100728625535792

    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 1
    127.0.0.1:7000

    -4315590310475478861 < token(1) < -3838100728625535792 => 127.0.0.1:7000

### pkey=4

    id | name | system.token(id)
    ----+------+----------------------
    4 | test | -2729420104000364805

    127.0.0.1:7000  rack1       Up     Normal  272.09 KiB      48.85%              -2993724081134374036
    127.0.0.1:8000  rack1       Up     Normal  148.88 KiB      51.15%              -2193895410473243752

    zyake@DESKTOP-P3FQ6DK:~/apache-cassandra-4.0.3-2/bin$ ./nodetool -pp getendpoints tutorialspoint test 4
    127.0.0.1:8000

    -2993724081134374036 < token(4) < -2193895410473243752 => 127.0.0.1:8000

## Tracing cross node SELECT

    cqlsh:tutorialspoint> tracing on;
    Now Tracing is enabled
    cqlsh:tutorialspoint> SELECT * FROM test WHERE id = 4;

    id | name
    ----+------
    4 | test

    (1 rows)

    Tracing session: ccddaaa0-b679-11ec-9aed-67212999bbf7

    activity                                                                                    | timestamp                  | source    | source_elapsed | client
    ---------------------------------------------------------------------------------------------+----------------------------+-----------+----------------+-----------
                                                                            Execute CQL3 query | 2022-04-07 22:51:18.606000 | 127.0.0.1 |              0 | 127.0.0.1
                        Parsing SELECT * FROM test WHERE id = 4; [Native-Transport-Requests-1] | 2022-04-07 22:51:18.612000 | 127.0.0.1 |           5894 | 127.0.0.1
                                            Preparing statement [Native-Transport-Requests-1] | 2022-04-07 22:51:18.612000 | 127.0.0.1 |           6748 | 127.0.0.1
                                reading data from /127.0.0.1:8000 [Native-Transport-Requests-1] | 2022-04-07 22:51:18.614000 | 127.0.0.1 |           8299 | 127.0.0.1
    Sending READ_REQ message to /127.0.0.1:8000 message size 80 bytes [Messaging-EventLoop-3-1] | 2022-04-07 22:51:18.615000 | 127.0.0.1 |           9152 | 127.0.0.1
                        READ_REQ message received from /127.0.0.1:7000 [Messaging-EventLoop-3-5] | 2022-04-07 22:51:18.619000 | 127.0.0.1 |            589 | 127.0.0.1
                                        Executing single-partition query on test [ReadStage-1] | 2022-04-07 22:51:18.623000 | 127.0.0.1 |           4775 | 127.0.0.1
                                                    Acquiring sstable references [ReadStage-1] | 2022-04-07 22:51:18.623000 | 127.0.0.1 |           5159 | 127.0.0.1
                                                        Merging memtable contents [ReadStage-1] | 2022-04-07 22:51:18.625000 | 127.0.0.1 |           6577 | 127.0.0.1
                                            Read 1 live rows and 0 tombstone cells [ReadStage-1] | 2022-04-07 22:51:18.626000 | 127.0.0.1 |           7532 | 127.0.0.1
                                            Enqueuing response to /127.0.0.1:7000 [ReadStage-1] | 2022-04-07 22:51:18.626000 | 127.0.0.1 |           7816 | 127.0.0.1
    Sending READ_RSP message to /127.0.0.1:7000 message size 65 bytes [Messaging-EventLoop-3-1] | 2022-04-07 22:51:18.628000 | 127.0.0.1 |           9717 | 127.0.0.1
                        READ_RSP message received from /127.0.0.1:8000 [Messaging-EventLoop-3-5] | 2022-04-07 22:51:18.629000 | 127.0.0.1 |          23494 | 127.0.0.1
                            Processing response from /127.0.0.1:8000 [RequestResponseStage-3] | 2022-04-07 22:51:18.631000 | 127.0.0.1 |          25333 | 127.0.0.1
                                                                                Request complete | 2022-04-07 22:51:18.632437 | 127.0.0.1 |          26437 | 127.0.0.1



