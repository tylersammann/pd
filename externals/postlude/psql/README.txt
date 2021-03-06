PSQL - a postgres client for PD
----

This is the Readme file is adapted from the Readme for 'sqlsingle' by Iain Mott 2001 - http://soundart.tripod.com

'psql' allows you to send SQL messages to a PostgreSQL database from Pd and retrieve the results.
In order to use the 'psql' external you will need to install PostgreSQL - available from http://www.postgresql.org.
There's a bit of work to do compiling the source, setting up environment variables etc - but the package comes with good documentation. Read the installation guide and do the introductory tutorial before trying 'psql'. See also 'some tips & notes' below before you decide to use 'psql'.

As with all applications that connect to a PostgreSQL database, 'psql' requires the 'postmaster' daemon to be running.
'psql' may connect to a database on a local host or on a remote machine via TCP/IP (handled by the external).

If no arguments are specified in 'psql' - messages will be sent to a postmaster via a local UNIX socket and will attempt to connect to the default PostgreSQL 'template1' database. Such a connection may be used to create new database by sending the SQL message: 'CREATE DATABASE databasename'. Before this can be done, you will need to set your username as a PostgreSQL user. This can be done from a PostgreSQL superuser account (not root) generally called 'postgres' which handles
database administrative duties including access privileges.

Note: all SQL messages sent to the 'psql' must start with 'sql' and terminate with 'sqlend'. By convention, all PostgreSQL commands (some are non-standard SQL) are written in allcaps. A comma between an 'sqlend' and a 'sql' in a PD message, will execute a new query or command.

Once you have created a database you can send messages to it with a second 'psql' object with the argument 'databasename'. See the patch 'psql.pd' for an example of this.

If you wish to connect with a database on a networked machine - two arguments are needed in 'psql' in addition to the name of the database. The first is the name of the machine - the second is the port number. To facilitate this, the server machine must run the 'postmaster' with  a '-i' flag to enable remote connections. The postmaster command line must also specify the port number ('-p <num>'). See PostgreSQL documentation for details. A startup script is available (in the Linux distribution of PostgreSQL at least) somewhere to enable automated activation of the postmaster (i think this is described in one of the install guides in the distribution tarball?). For security reasons, the postmaster should 
not be run as root. Rather, it should be run from the 'postgres' account.

psql outputs results of queries as lists. Often there will be more than one instance for a given query (instances are referred to as 'tuples'). eg. the query 'Select * FROM tablename' will return all rows from the specified table in a database. 'psql' retrieves each row as a seperate message, each preceded by an index starting at zero. 


At present 'psql' handles 6 PostgreSQL data types: PGINT4; PGFLOAT8; PGDOUBLE; PGDATE; PGDATETIME and PGVARCHAR. If a query returns tuples of a different data type, these will be compiled into a list (on output) as Pd 'symbols'. In contrast, PGINT4, PGFLOAT8 and PGDOUBLE are compiled into lists as floats. Like non-specified data types, PGDATE, PGDATETIME and PGVARCHAR are compiled into lists as 'symbols'.


Requirements
------------

This object reuires

libpq >= 7.4
PD >= 0.37

Installation
------------

Make sure the Makefile includes the path to your PostgreSQL headers.
On most GNU/Linux systems that will be /usr/include/postgresql, but on
Mac OS X you'll need to get them via Fink, MacPorts or something like
that.

Then from a shell prompt type:

make
make install



