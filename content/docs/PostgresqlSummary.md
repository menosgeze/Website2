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

## A great tutorial.
A great tutorial for Postgres is [here](https://pgexercises.com/)
where they don't use any other SQL language but Postgres. 
I tried other where they used old versions of MySQL,
and they were too different, and you cannot use even their 
documentation because the latest version has improved too much.

## The first few commnands

* Creating Tables [here](https://www.postgresql.org/docs/9.1/sql-createtable.html)

`create table tablename (type1 arg1 constraint1, type2 arg2 constraint2, etc.);`

A basic list of types is: `varchar` or `varchar(n)`,`int`, `float`, `boolean`, `int[]` although when inserting from python witn psycopg2 one inserts as `'{val1,...}'`. See [here](https://www.guru99.com/postgresql-data-types.html)

* Retrieve data

`select attribute1, attribute2 from tablename where conditions` 

`select attribute1, count(*) as count from tablename 
where conditions group by attribute1;`

`select attribute1, sum(attribute2) as sum from tablename where conditions group by attribute1;`

Remark: Some conditions with OR can be alternatively listed as 
`attribute in (list as a tupple);`


* Insert data:
`insert into tablename (list of attribute) values (data1 as a tuple), (data2 as a tuple), ...;`

Or

`insert into tablename (attributes) select same attributes from tablename2 where conditions on table2;`

See [here](https://www.postgresql.org/docs/9.5/sql-insert.html)

* Update attributes:

`update tablename set attributetochange = newvalue where conditions;`

* Classification:

`case when condition then value when condition2 then value2 else othervalue end`

6. To delete some rows:

`delete from tablename where conditions;`

To delete all rows:

`truncate tablename`

To delete the table and all rows:

`drop table tablename;`

* To retrieve portions of a time stamp

`to_char(attribute, 'YYYY-MM')`

`date_part('part as a string', attribute)` where the part could be 'month', 'year', etc.

* Mix data from several tables. 
`select tabname.attribute1, tabname.attribute2
from tab1 join tab2 on tab1.attribute = tab2.attribute
where conditions on tab1.attributes and tab2.attributes;`

    - `inner join` is the same as `joint`.
    - `outer join` takes both tables and if an attribute is not in one table shows it as null. In other words, 
this is the **union** of both tables.
    - `left outer join` takes rows from first table and shows them as null if the combined data does not have that propery.

See [here](https://www.postgresql.org/docs/8.3/tutorial-join.html) for a larger explanation.

* With Clause:
`With temporaryTableName as (select statment) select statement using another table and temporaryTableName;`
There could be several temporaryTableNames separated by comma but the last one does not have comma. 

* Other cool stuff:
    - Strings are appended with ||: `attribute || ', ' || attribute2.`
    - UNION to list rows from two tables with the similar attributes, and usually the same number of attributes.
    - Mathematical operators [here](https://www.postgresql.org/docs/9.1/functions-math.html) including the `width_bucket(attribute, min, max, number_of_buckets)` which splits the range from min inclusive to max exclusive to the number of buckets and tells where attribute falls. 
    - Use `str like 'beginning%'` to check if it begins with a pattern, or `UPPER(str)` to make it all upper case.  
    - For histograms with Postgresql see [here](https://tapoueh.org/blog/2014/02/postgresql-aggregates-and-histograms/)

## Back up:
There are several things to know here:

* To dump a copy use:
`pg_dump -U username dbname > dbfile.pgsql`
Add the option `-Z0` to `-Z9` for compression.

And to restore it:

`psql dbname < file.pgsql`

Or even better:
`pg_restore --dbname=dbname`

where there are multiple options to determine 
what you want to restore, including indexes.

For example:
    - `-a` for data only, 
    - `-e` for exit on error, 
    - `-f` for filename, 
    - `-F` for format,
    - `-I` to restore only certain indices, 
    - `-j` for the number of jobs.

See also options `-l` and `-L` for list of things to restore, [here](https://www.postgresql.org/docs/9.2/app-pgrestore.html)

* To insert data into a table from a CVS file:

`copy tablename(attributeListAsATuple) from 'path\cvsfile.ext' delimiter ',' null '\n' CSV HEADER;` 

See [here](https://www.postgresql.org/docs/9.0/sql-copy.html)
 

	
