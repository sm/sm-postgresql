# pgpool-II Extension Set

## About pgpool-II

( From the documentation: http://www.pgpool.net/pgpool-web/pgpool-II/doc/pgpool-en.html )

pgpool-II is a middleware that works between PostgreSQL servers and a PostgreSQL database client. It is licensed under BSD license. It provides the following features.

* Connection Pooling

pgpool-II saves connections to the PostgreSQL servers, and reuse them whenever a new connection with the same properties (i.e. username, database, protocol version) comes in. It reduces connection overhead, and improves system's overall throughput.

* Replication

pgpool-II can manage multiple PostgreSQL servers. Using the replication function enables creating a realtime backup on 2 or more physical disks, so that the service can continue without stopping servers in case of a disk failure.

* Load Balance

If a database is replicated, executing a SELECT query on any server will return the same result. pgpool-II takes an advantage of the replication feature to reduce the load on each PostgreSQL server by distributing SELECT queries among multiple servers, improving system's overall throughput. At best, performance improves proportionally to the number of PostgreSQL servers. Load balance works best in a situation where there are a lot of users executing many queries at the same time.

* Limiting Exceeding Connections

There is a limit on the maximum number of concurrent connections with PostgreSQL, and connections are rejected after this many connections. Setting the maximum number of connections, however, increases resource consumption and affect system performance. pgpool-II also has a limit on the maximum number of connections, but extra connections will be queued instead of returning an error immediately.

* Parallel Query

Using the parallel query function, data can be divided among the multiple servers, so that a query can be executed on all the servers concurrently to reduce the overall execution time. Parallel query works the best when searching large-scale data.
pgpool-II talks PostgreSQL's backend and frontend protocol, and relays a connection between them. Therefore, a database application (frontend) thinks that pgpool-II is the actual PostgreSQL server, and the server (backend) sees pgpool-II as one of its clients. Because pgpool-II is transparent to both the server and the client, an existing database application can be used with pgpool-II almost without a change to its sources.

