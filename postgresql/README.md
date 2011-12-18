# SM Framework Postgresql Extension


## Installation and Configuration

In order to install the latest version of PostgreSQL known to the component do:

    sm postgresql install

Initialize the PostgreSQL database as the postgres user:

    su - postgres -c 'sm postgresql initialize'

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

If you wish to setup replication, first perform Installation and Configuration
on both the master and replica servers. Once you have tuned your
configuration files to your liking, on the replica server run:

    sm postgresql replication setup master postgres@{{master}}

You will need to ensure that the postgres user can ssh to and from each host.

