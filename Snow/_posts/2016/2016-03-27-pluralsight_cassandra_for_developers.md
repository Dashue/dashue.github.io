---
layout: post
title: Notes for Pluralsight course: Cassandra for Developers 
categories: Study, Cassandra, Database 
published: published
---

---excerpt
Notes from a study into Cassandra using the "Cassandra for Developers" Pluralsight course by Paul O'Fallon
---end

To my future self: This was what you found noteworthy while studying the Pluralsight course [Cassandra for Developers](https://app.pluralsight.com/library/courses/cassandra-developers/table-of-contents) by Paul O'Fallon.

Topology
--------

Data centres | Nodes | Virtual nodes

Keyspace | Table | Partition | Row

Tunable Consistency
-------------------

__Replication Count__:
Number of replicas of the data stored across the cluster.

__Read Consistency__: 
How many nodes the coordinator node will consult in an attempt to return the most current information to the caller.

__Write Consistency__: 
From how many nodes the coordinator node will wait to receive an acknowledgement of the write, before returning a positive response to the caller.
Configurable with each statement.

__Hinted handoff__:
Strategy to help when a write fails to a node. The data is written down in the coordinator node and is retried.

__Read repair__:
When the coordinator node realizes there is a missmatch in data between nodes, it will identify what right data is and update the incorrect nodes.

Identifications depends on the configuration of _Read Consistency_.

_*Note: Recommended to periodically run 'nodetool repair'_


__Strong consistency__:
(Write Consitency + Read Consistency) > Replication Factor

Only one consistent state can be observed, as oppose to eventual consistency where the read state depends on which nodes data was requested from.


Keys
----

Primary keys consists of one or multiple partition keys and/or one or multiple clustering keys. Data is sorted based on clustering key.

((P<sub>Key1</sub>, P<sub>Key2</sub>...),(C<sub>Key1</sub>, C<sub>Key2</sub>...))

_*Note: The approach to defining keys enables modelling of different levels within the same partition_


Simple Datatypes
----------------

__Counter__:
Specific type of column to achieve performant counters in a distributed environment

__Static__:
Static columns are associated with a partition and the data held are shared by all rows within that partition. It is therefore not necessary to provide them when adding or updating non static data. This is to prevents duplication inbetween rows. An example would be _Balance_ within a _Bills_ table, where the table would hold multiple bills for a user but only one balance.	

__TimeUUID__:
Guaranteed Unique Timestamps to handle timestamps in a distributed environment. Includes Mac Address, so even if 2 Timestamps are generated at the same time in two different machines, they will be unique and distinguishable from each other.

_*Note: Adding an aditional static TimeUUID static to a table is a good approach to keeping track of the latest entry_


Complex Datatypes
-----------------

__Set__:
Provides an unordered collection of elements. Example usage: book genres

__List__:
Provides an ordered collection of elements. Allows to edit by position in the collection. Example usage: book chapters

__Map__:
Provides a key,value based collection
Example usage chapter read progress

__Tuples__:
Fixed-length set of typed poitional fields

__User Defined Types__:
Re-usable custom column type

_*Note: Complex types enables modelling of additional levels within the same partition_


Expiring data with TTLs
-----------------------

TTLs can be configured on a _Column_, _Row_ and _Table_ basis.

__Tombstones__:
Data is replaced by tombstones upon TTL expire, to prevent read repair from happening. Tombstones are periodically cleaned during garbage collection.


Data Partitioning using Time Buckets
-------------------------------------

By selecting the partition key to be year and month as a varchar "2016-03", data will be broken down into partitions, preventing partitions from growing too large.

_*Note: Be aware that querying multiple partitions will impact performance_


Seconary Indexes
----------------

Since queries can only be run against columns partaking in the primary key, secondary indexes provides additional query options on other columns. 

_*Note: Secondary indexes can even be placed on Collections_


Manually Maintained Indexes
---------------------------

Secondary indexes comes with some restrictions, manually maintained indexes supply a workaround.

An example: The books table contains a list of genres which is static and therefore cannot be indexed against. Whenever an action happens against a books genres, also update the index table. Searching books by genres utilizes this dedicated table which acts as a read model for books by genre.

_*Note: It's quite common to maintain your own indexes when using Cassandra_


Batches
-------

Intended for keeping tables in sync, not for fast loading of data. One use-case is for keeping manually maintained indexes in sync, like books by genre.

Unlogged Batches is intended for multiple inserts into the same partition. The batch will be sent to the appropriate node as a single write.

_*Note: A Batch is not a Transaction_


Compare-and-Set Operations (Light Transactions)
--------------------------

Conditional Insert

    INSERT INTO ...
    ...
    IF NOT EXISTS;

Conditional Update

    UPDATE ... SET ...
    WHERE ...
    IF ... = ...

Reset password example: 
	
	update users set password = 'newpassword', reset_token = null
	where id = 'user-name' 
	if reset_token = 'expected-token'


_*Note: Works with Batches as well_