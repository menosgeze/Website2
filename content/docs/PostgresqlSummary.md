# PostgreSQL

## Installing.

1. Go to the github source 
[here](https://github.com/postgres/postgres) 
and download the github from source. 
2. Go to documentation 
[here](https://www.postgresql.org/docs/9.3/install-short.html) 
or for a full process here
[here](https://www.postgresql.org/docs/9.3/install-procedure.html)
3. My idea of installing it from source is that you can
build with specific option. In particular, I was interested in
**--with-python**:  
	cd /path_to_folder_POSTGRESQL
	./configure
	mkdir build_dir
	cd build_dir
	/path_to_source_tree/configure --with-python
	gmake

## Some minor problems
1. It could be that the settings of the computer don't know 
the path to the command "pg_ctl"
	export PATH=$PATH:/path_to_pg_ctl

This is usually found in:
	cd /usr/local/psql/bin

To make it permanent, so one does not need to write it again
the next time one logs in Linux, one goes to:
	vim ~/.profile

and adds it there. Or add it to the initializing file of your
personalized shell.


2. It could be that one forgets to close the server at the end 
of a session and then one has problems to restart the server. Then
go to the folder of the data (see below psql_data)
and then look for **postmaster.pid**,
which is a log file that conflicts with the server. One could delete it
by:
	rm postmaster.pid

3. It could be that still one cannot open the database. In this case,
kill the process by identifying it with:
	top
The problem is that the line
    export PGHOST="path_to_data_folder"
might have been forgotten.



## Creating a Database

I love Cocalc/SageMath documentation for using postgreSQL in Cocalc in 2 min
[here](https://doc.cocalc.com/howto/postgresql.html)

In here, either one has properly set up your **HOME** variable or replace it by what **pwd** prints.

1. Create the data folder 
	cd
	pg_ctl initdb -D psql_data 
	echo "unix_socket_directories = '$HOME/psql_data'" >> psql_data/postgresql.conf
	echo "unix_socket_permissions = 0700" >> psql_data/postgresql.conf

Alternatively, after creating this folder, 
move to it and write the lines
in quotes to the file **postgresql.conf**
inside the folder.

2. Start the postgresql server
	pg_ctl -D psql_data -l logfile -o "-h ''" start

The options are to write a logfile in case of errors. However,
one can just start it with:
	pg_ctl -D psql_data start

And check it is running with:
	pg_ctl -D psql_data status

3. Create the database using these configuration in this folder:
	export PGHOST="$HOME/psql_data"
	createdb dbname
	psql dbname
Don't forget to replace the database name **dbname**.

4. Exit the database and close the server with:
	pg_ctl -D psql_data stop


	
