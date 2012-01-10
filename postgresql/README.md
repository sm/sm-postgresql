# SM Framework Postgresql Extension


## Installation and Configuration

In order to install the latest version of PostgreSQL known to the component do:

    sm postgresql install

Initialize the PostgreSQL database as the postgres user:

    sm postgresql initialize

Configure the sytem to run PostgreSQL:

    sm postgresql configure

If we are running as root then we should next configure the init.d/rc.d script:

    root# sm postgresql initd configure

If we use Monit, configure the PostgreSQL monitrc file:

    sm postgresql monit configure

If we use systemd, configure the PostgreSQL systemd file:

    sm postgresql systemd configure

## Running PostgreSQL

To start PostgreSQL:

    sm postgresql start

To stop PostgreSQL:

    sm postgresql stop

To restart PostgreSQL:

    sm postgresql restart

To obtain the status of the PostgreSQL process:

    sm postgresql status

## Advanced

### Extensions

If you wish to work with extensions then you can do the following:

    sm postgresql extensions available # lists all installable extensions in src

In order to install an extension, say hstore and tsearch2:

    sm postgresql extensions install hstore
    sm postgresql extensions install tsearch2

Note that you should do this while the PostgreSQL cluter is running so that the
CREATE EXTENSION statement is successful.

In order to list the all/available/installed extensions:

    sm postgresql extensions list
    sm postgresql extensions available
    sm postgresql extensions installed

Installation automatically enables (creates) an extension within the
running PostgreSQL cluster. If you wish to toggle any of the installed
extensions you may do so:

    sm postgresql extensions disable hstore tsearch2
    sm postgresql extensions enable hstore tsearch2

### Replication

NOTE: Replication is a work in progress, it needs some more testing,
help with this is *greatly* appreciated!

If you wish to setup replication, first perform Installation and Configuration
on both the master and replica servers.

Next ensure that the pg\_hba.conf on master has a replication line configured
for the replica(s) you will use. So for example if I have one replica
with IP address 10.0.5.212 then I would put an entry as follows

    host    replication     postgres        10.0.5.212/32            trust

Now place the ~/.ssh/id\_replication.pub file from the replica into the master's
.ssh/authorized\_keys file and similarly from the master onto the replica.

Be sure to turn hot standby on on replica server so that you may connect
and perform SELECT queries against it.

    hot_standby = on

Once these steps are complete and you have tuned your configuration files as
required for replication you are ready to configure replicatoin.
On each replica server run:

    sm postgresql replication configure master postgres@{{master}}

