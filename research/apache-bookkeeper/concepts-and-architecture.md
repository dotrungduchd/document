https://bookkeeper.apache.org/docs/4.8.0/getting-started/concepts/

# Concepts

BookKeeper is a service that provides persistent storage of streams of log entries—aka records—in sequences called ledgers. BookKeeper replicates stored entries across multiple servers.

### Basic terms

In BookKeeper:

- each unit of a log is an entry (aka record)
- streams of log entries are called ledgers
- individual servers storing ledgers of entries are called bookies

BookKeeper is designed to be reliable and resilient to a wide variety of failures. Bookies can crash, corrupt data, or discard data, but as long as there are enough bookies behaving correctly in the ensemble the service as a whole will behave correctly.

### Entries

    - Entries contain the actual data written to ledgers, along with some important metadata.

### Ledgers

    - Ledgers are the basic unit of storage in BookKeeper.

Ledgers are sequences of entries, while each entry is a sequence of bytes. Entries are written to a ledger:

- sequentially, and
- at most once.

This means that ledgers have append-only semantics. Entries cannot be modified once they’ve been written to a ledger. Determining the proper write order is the responsbility of client applications.

### Clients and APIs

BookKeeper clients have two main roles: they create and delete ledgers, and they read entries from and write entries to ledgers.

There are currently two APIs that can be used for interacting with BookKeeper:

- The <a href='https://bookkeeper.apache.org/docs/4.8.0/api/ledger-api'>ledger API</a> is a lower-level API that enables you to interact with ledgers directly.
- The <a href='https://bookkeeper.apache.org/docs/4.8.0/api/distributedlog-api/'>DistributedLog API</a> is a higher-level API that enables you to use BookKeeper without directly interacting with ledgers.

### Bookies

    Bookies are individual BookKeeper servers that handle ledgers (more specifically, fragments of ledgers). Bookies function as part of an ensemble.

A bookie is an individual BookKeeper storage server. Individual bookies store fragments of ledgers, not entire ledgers (for the sake of performance). For any given ledger L, an `ensemble` is the group of bookies storing the entries in L.

### Motivation

BookKeeper provides a number of advantages for such applications:

- Highly efficient writes
- High fault tolerance via replication of messages within ensembles of bookies
- High throughput for write operations via striping (across as many bookies as you wish)

### Metadata storage

BookKeeper requires a metadata storage service to store information related to ledgers and available bookies. BookKeeper currently uses ZooKeeper for this and other tasks.


