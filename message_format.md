# Message Format

## org.apache.cassandra.net.Message

    /**
     * Each message contains a header with several fixed fields, an optional key-value params section, and then
     * the message payload itself. Below is a visualization of the layout.
     *
     *  The params are prefixed by the count of key-value pairs; this value is encoded as unsigned vint.
     *  An individual param has an unsvint id (more specifically, a {@link ParamType}), and a byte array value.
     *  The param value is prefixed with it's length, encoded as an unsigned vint, followed by by the value's bytes.
     *
     * Legacy Notes (see {@link Serializer#serialize(Message, DataOutputPlus, int)} for complete details):
     * - pre 4.0, the IP address was sent along in the header, before the verb. The IP address may be either IPv4 (4 bytes) or IPv6 (16 bytes)
     * - pre-4.0, the verb was encoded as a 4-byte integer; in 4.0 and up it is an unsigned vint
     * - pre-4.0, the payloadSize was encoded as a 4-byte integer; in 4.0 and up it is an unsigned vint
     * - pre-4.0, the count of param key-value pairs was encoded as a 4-byte integer; in 4.0 and up it is an unsigned vint
     * - pre-4.0, param names were encoded as strings; in 4.0 they are encoded as enum id vints
     * - pre-4.0, expiry time wasn't encoded at all; in 4.0 it's an unsigned vint
     * - pre-4.0, message id was an int; in 4.0 and up it's an unsigned vint
     * - pre-4.0, messages included PROTOCOL MAGIC BYTES; post-4.0, we rely on frame CRCs instead
     * - pre-4.0, messages would serialize boolean params as dummy ONE_BYTEs; post-4.0 we have a dedicated 'flags' vint
     *
     * <pre>
     * {@code
     *            1 1 1 1 1 2 2 2 2 2 3
     *  0 2 4 6 8 0 2 4 6 8 0 2 4 6 8 0
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Message ID (vint)             |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Creation timestamp (int)      |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Expiry (vint)                 |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Verb (vint)                   |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Flags (vint)                  |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Param count (vint)            |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * |                               /
     * /           Params              /
     * /                               |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * | Payload size (vint)           |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * |                               /
     * /           Payload             /
     * /                               |
     * +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
     * }
     * </pre>
     */

## Message payload for Gossip Protocol

### GossipDigestSyn

    /**
    * This is the first message that gets sent out as a start of the Gossip protocol in a
    * round.
    */
    public class GossipDigestSyn
    {
        public static final IVersionedSerializer<GossipDigestSyn> serializer = new GossipDigestSynSerializer();

        final String clusterId;
        final String partioner;
        final List<GossipDigest> gDigests;

### GossipDigestAck

    /**
    * This ack gets sent out as a result of the receipt of a GossipDigestSynMessage by an
    * endpoint. This is the 2 stage of the 3 way messaging in the Gossip protocol.
    */
    public class GossipDigestAck
    {
        public static final IVersionedSerializer<GossipDigestAck> serializer = new GossipDigestAckSerializer();

        final List<GossipDigest> gDigestList;
        final Map<InetAddressAndPort, EndpointState> epStateMap;

### GossipDigestAck2

    /**
    * This ack gets sent out as a result of the receipt of a GossipDigestAckMessage. This the
    * last stage of the 3 way messaging of the Gossip protocol.
    */
    public class GossipDigestAck2
    {
        public static final IVersionedSerializer<GossipDigestAck2> serializer = new GossipDigestAck2Serializer();

        final Map<InetAddressAndPort, EndpointState> epStateMap;