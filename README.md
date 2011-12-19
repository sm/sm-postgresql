# SM Framework Postgresql Extension Set


This set contains the following postgresql related extensions:

* postgresql
* pgpool
* pgbouncer
* postgis
* queue\_classic
* slony

Please see each extension's readme file for more details than are provied below:

    postgresql/README.md
    pgbouncer/README.md
    pgpool/README.md
    postgis/README.md
    queue_classic/README.md
    slony/README.md

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

