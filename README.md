# SM Framework Postgresql Extension Set


This set contains the following postgresql related extensions:

* postgresql
* pgpool
* pgbouncer
* slony

To install this extension, with the latest SM Framework you can do:

    sm set install postgresql

Alternatively the uri may be specified:

    sm set install postgresql git://github.com/sm/sm-postgresql.git

After this you may issue the extension specific commands you require such as:

    sm postgresql install
    sm pgpool install

NOTE: postgresql used to be located in sm-databases, so if you have an
older sm-databases upgrade it so that you no longer have a postgresql
extension in it:

    sm set install databases

