# SM Framework Postgresql Extension Set

This SM Framework extension set contains the following postgresql related extensions:

### postgresql

The PostgreSQL database system

    postgresql/README.md

### pgpool

Provides Connection Pooling, Replication, Load Balancing, Limiting Exceeding Connections, Parallel Query

    pgpool/README.md

### pgbouncer

Provides Session pooling, Transaction pooling, Statement pooling

    pgbouncer/README.md

### postgis

adds support for geographic "spatial" objects to the PostgreSQL object-relational database.

    postgis/README.md

### queue\_classic

a PostgreSQL-backed queueing library that is focused on concurrent job locking, minimizing database load & providing a simple & intuitive user experience

    queue_classic/README.md


### repmgr

a set of open source tools that helps DBAs and System administrators manage a cluster of PostgreSQL databases

    repmgr/README.md

### slony

a "master to multiple slaves" trigger based replication system for PostgreSQL supporting cascading (e.g. - a node can feed another node which feeds another node...) and failover.

    slony/README.md

# Installation

To install this extension set, with the latest SM Framework you can do:

    sm get head               # Ensuring we are on the latest SM Framework.
    sm set install postgresql # Installing this (known) extension set.

Alternatively the uri may be specified:

    sm set install postgresql git://github.com/sm/sm-postgresql.git

After this you may issue the extension specific commands you require such as:

    sm postgresql install
    sm pgpool install

NOTE: postgresql used to be located in sm-databases, so if you have an
older sm-databases upgrade it so that you no longer have a postgresql
extension in it:

    sm set install databases

