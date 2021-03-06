- [Apache Hadoop Website](http://hadoop.apache.org/) - a framework for running applications on large cluster built of commodity hardware.
- [Hadoop Javadoc](http://hadoop.apache.org/docs/current/api/)
- [Hue - wikipedia](https://en.wikipedia.org/wiki/Hue_(Hadoop))
- [Hadoop Docs](https://hadoop.apache.org/docs/)
- [HDFS-3150 - use host name for accessing data nodes](https://issues.apache.org/jira/browse/HDFS-3150)
- https://rainerpeter.wordpress.com/2014/02/12/connect-to-hdfs-running-in-ec2-using-public-ip-addresses/

# [Hadoop Wiki](https://wiki.apache.org/hadoop)
- [Sequence Files on Hadoop wiki](https://wiki.apache.org/hadoop/SequenceFile) ✓
- [MapReduce on Hadoop wiki](https://wiki.apache.org/hadoop/MapReduce)
- https://wiki.apache.org/hadoop/HadoopMapReduce

# Scientific Papers
- [MapReduce: Simplified Data Processing on Large Clusters by Jeffrey Dean and Sanjay Ghemawat](http://research.google.com/archive/mapreduce.html)
- [Same paper on USENIX](https://www.usenix.org/legacy/events/osdi04/tech/full_papers/dean/dean_html/index.html)

# The user:
- thinks about how to parallelize the computation
- specify a map function that processes a key/value pair to generate a set of intermediate key/value pairs.
- specify a reduce function that merges all intermediate values associated with the same intermediate key.

# The runtime system:
- partitions the input data
- distributes work around the cluster
    + moves the data to a particular node
    + schedules program's execution across a set of machines
- handles machine failures (fault tolerance)
    + thru re-execution
- manages required inter-machine communication
- load balancing

Hadoop implements a computational paradigm named Map/Reduce, where the application is divided into many small fragments of work, each of which may be executed or re-executed on any node in the cluster.
In addition, it provides a distributed file system (HDFS) that stores data on the compute nodes, providing very high aggregate bandwidth across the cluster.
Both MapReduce and the Hadoop Distributed File System are designed so that node failures are automatically handled by the framework.

MapReduce is a programming model for processing large data sets.
MapReduce programs are automatically parallelized and executed (distributed) on a large cluster of commodity machines. (parallel and distributed system)
A typical MapReduce computation processes many terabytes of data on thousands of machines.

# Maven Central
- `org.apache.hadoop:hadoop-common` (there are also Cloudera releases)
- `org.apache.hadoop:hadoop-hdfs`

# Notable hosting providers
- https://cloudera.com/

# Sequence files
Sequence files are flat files consisting of binary key/value pairs (records). Its widely used as input/output format for Hadoop MapReduce jobs. Also internally in Hadoop the temporary outputs of maps are stored as sequence files.

Sequence files may use compression:
- uncompressed
- record compression (only values are compressed)
- block compression (records are collected into a configurable size blocks and those blocks are compressed)

Sequence file format:
- header
    + magic number
    + key class name
    + value class name
    + isCompressed
    + isBlockCompressionUsed
    + compression codec class name
    + metadata
    + sync marker
- body (list of records/blocks separated with sync markers). The sync marker permits seeking to a random point in a file and then re-synchronizing input with record boundaries. This is required to be able to efficiently split large files for MapReduce processing.

# Hadoop Data Types
- BytesWritable (byte[])
- Text (String)

Operations:
- write keys
- read keys
- sort keys

# Writer
- hflush (flushes to the client's user buffer
- hsync (flushes to a disk device, similar to posix `fsync`)

# Reader

# FileContext (org.apache.hadoop.fs.FileContext)
Provides interface to the writer for using the HDFS (create, open, list, etc.).

# DFSOutputStream
Creates files from a stream of bytes.

The client application writes data that is cached internally by
this stream. Data is broken up into packets, each packet is
typically 64K in size. A packet comprises of chunks. Each chunk
is typically 512 bytes and has an associated checksum with it.

When a client application fills up the currentPacket, it is
enqueued into dataQueue.  The DataStreamer thread picks up
packets from the dataQueue, sends it to the first datanode in
the pipeline and moves it from the dataQueue to the ackQueue.
The ResponseProcessor receives acks from the datanodes. When an
successful ack for a packet is received from all datanodes, the
ResponseProcessor removes the corresponding packet from the
ackQueue.

In case of error, all outstanding packets and moved from
ackQueue. A new pipeline is setup by eliminating the bad
datanode from the original pipeline. The DataStreamer now
starts sending packets from the dataQueue.
