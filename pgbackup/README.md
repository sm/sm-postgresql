# Instructions to Deploy and Manage Backups

## Overview

The instructions below review how to setup base and WAL backups into a Swift 
container leveraging wal-e from Heroku. The files in the Swift container can 
then be used to restore databases in a number of ways.

This solution should not be used in production until a method for backing up 
Swift is put into production.

## Install the script files

		$ sm ext install postgresql

## Install wal-e

As root

		$ sm pgbackup install

## Configure pgbackup.conf file

Just like PostgreSQL, pgbackup has it's own configuration file. The 
configuration file contains all the default parameter values that are used in 
the pgbackup scripts.  At the very least you should provide the unique Swift 
container name and Swift credentials, without them none of the scripts will 
work. If you populate this file correctly there is likely no need to pass 
command line parameters in any of the commands noted below.

		$ sm pgbackup configure

If you've clone the repo above as directed the file is located in:

		$ vi /etc/pgbackup/pgbackup.conf

If at any time you make changes to any of the swift* parameters, the configure 
script (next section) should be run to update postgresql.conf with the correct 
settings

## Configure postgresql.conf with Required Settings for wal-e

As user postgres (and for all additional commands)
	
		$ sm pgbackup configure swiftContainerName "pickles"

Available standard command line parameters are:

		Parameter Name        Parameter Value
		swiftContainerName    Example: "pgbackup"
		swiftAuthUrl          Example: "http://10.150.0.60:5000/v2.0/"
		swiftTenant           Example: "bosh"
		swiftUser             Example: "bosh"
		swiftPassword         Example: "bosh"

Once this command is run, at least the master database server needs to be 
restarted, WAL files will begin to stream to the Swift container, the WAL files 
will be useless until you perform a full backup which can be kicked off 
manually (later on this) or at least schedule the base backups to run with the 
cron job below.

## Schedule Base Backups 

		$ sm pgbackup cron backups enable

Note that the output of the last cron execution is written to /home/postgres/cron.backup.log

## Stop Scheduled Base Backups 

		$ sm pgbackup cron backups disable

## Happiness and Bliss

Your WAL files are streaming to Swift and you've scheduled your base backups.  
At this point in a perfect world you are done.  This isn't a perfect world, 
it is full of developers, admins, users and other nasties which may mean you 
need to restore your database.

## Restoring a Backup

There are many different restore scenarios, a few are listed below:
		* From the latest base backup with the most recent WAL files
		* From a base backup
		* From the latest base backup to a particular point in time
		* From one PGHA cluster to another PGHA cluster

### Scenario 1 - Restore from the latest base backup with the most recent WAL files

Disable the PGHA cron run job and stop postgres on all servers in the cluster

		$ sm pgha cron run disable # If you are using pgha
		$ sm pgbouncer stop # If you are using pgbouncer
		$ sm postgresql stop
		$ sm pgha master my_ip <my_ip>

On the master server, run the restore script, provide any command line overrides as needed

		$ sm pgbackup restore base_and_latest_wal

		Note: the standard command line parameters are available

On the remaining replica servers, restart replication by enabling pgha cron
	
		$ sm pgha cron run enable

### Scenario 2 - Restore from a base backup

Important Note - While this solution will let you restore the base backup and 
have the master database available, a timeline error prevents replicas from 
attaching.  Until this is resolved this method of restore should probably be 
avoided.

	Disable the PGHA cron run job and stop postgres on all servers in the cluster

		$ sm pgha cron run disable # if using pgha
		$ sm pgbouncer cron run disable # if using pgbouncer
		$ sm postgresql stop
		$ sm pgha master my_ip <my_ip>

	On the master server, run the restore script, provide any command line overrides as needed

		$ sm pgbackup restore base

		Note: the standard command line parameters are available as well as 1 additional parameter which indicates which base backup to restore. See the next section for how to list the available base backups.

		Parameter Name        Parameter Value
		baseBackupName        Example: base_0000000E00000001000000C6_00000040 (Default is LATEST)

	On the remaining replica servers, restart replication by enabling pgha cron
	
		$ sm pgha cron run enable


### Listing Available Base Backups

To list the base backups available to you, run the following script

		$ sm pgbackup backups list

		Note: the standard command line parameters are available

The output of this command, particularly the first column, can be used when deciding which base backup to restore in the fourth restore scenario above.

### Keep It Clean - Setting a Backup File Retention Policy

	Backups take up space in Swift, fortunately we can delete older backups easily. The command below will delete all base and WAL backups except for the most recent 3.

		$ sm pgbackup maintenance delete previous

		Note: the standard command line parameters are available as well as 1 additional parameter which indicates the number of base and corresponding WAL files to keep

		Parameter Name        Parameter Value
		backupsToKeep         Example: 3 (Default is 5)

	In cases where you want to clear out the entire Swift container, run the following but beware, once deleted there is no "undo".

		$ sm pgbackup maintenance delete all


### Schedule Backup File Maintenance

The previous section describes the scripts you can use to manually maintain the list of backup files, below is the command to schedule a cron job to do the same:

		$ sm pgbackup cron maintenance enable

		Note that the output of the last cron execution is written to /home/postgres/cron.maintenance.log

### Disable Scheduled Backup File Maintenance

If you are performing a long running restore or other maintenance and don't want maintenance to run, disable the job:

		$ sm pgbackup cron maintenance disable

### Final Notes

If you restore a backup from one cluster to another cluster, be sure that the two of them do not write to the same Swift container, they may overwrite each other's base backups and/or WAL files.

If you receive errors about incorrect timelines during a restore, the replicas need to have replication reinitialized.  Run the following command as the postgres user on each of the replicas:

		$ sm postgresql replication configure master <ip address of master>

The cron output log files (/home/postgres/cron.*.log) should be monitored by Nagios or some other tool to monitor for errors.

A nagios script should be created to monitor that base backups have occurred within the last x hours.

