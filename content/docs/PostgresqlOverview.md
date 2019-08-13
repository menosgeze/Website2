# PostgreSQL

## Installing

1. Download the github source 
[here](https://github.com/postgres/postgres) 
 
2. Follow the documentation 
[here](https://www.postgresql.org/docs/9.3/install-short.html) 
or the full process
[here](https://www.postgresql.org/docs/9.3/install-procedure.html)

3. I like installing it from source because you can
build it with specific options. In particular, I was interested in
`--with-python`: 
{{< highlight bash >}}
	cd /path_to_folder_POSTGRESQL
	./configure
	mkdir build_dir
	cd build_dir
	/path_to_source_tree/configure --with-python
	gmake
{{< /highlight >}}

## Some minor problems
1. It could be that the settings of the computer don't know 
the path to the command `pg_ctl`

{{< highlight bash >}}
	export PATH=path/to/pg_ctl:$PATH
{{</highlight>}}

This is where I usually found it:
{{< highlight bash >}}
	/usr/local/psql/bin
{{</highlight>}}

To make it permanent, add it to the file:
{{< highlight bash >}}
	~/.profile
{{</highlight>}}

Or add it to the initializing file of your personalized shell.


2. It could be that one forgets to close the server at the end 
of a session and then one has problems to restart the server. Then
go to the folder of the data, and then remove `rm` the file `postmaster.pid`
which is a conflicting with the server due to not closing the server.

3. It could be that still one cannot open the database. Sometimes the problem 
is that you need to rewrite the line.

{{< highlight bash >}}
    export PGHOST="path_to_data_folder"
{{</highlight>}}




## Creating a Database

I love Cocalc/SageMath documentation for using postgreSQL in Cocalc in 2 min
[here](https://doc.cocalc.com/howto/postgresql.html)
where either one has properly set up the `HOME` variable or replace it by what the command `pwd` shows.

1. Create the data folder, for us it will be `psql_data` 
{{<highlight bash>}}
	cd
	pg_ctl initdb -D psql_data 
	echo "unix_socket_directories = '$HOME/psql_data'" >> psql_data/postgresql.conf
	echo "unix_socket_permissions = 0700" >> psql_data/postgresql.conf
{{</highlight>}}

Alternatively, after creating this folder, 
go to it and write the lines
in quotes to the file `postgresql.conf`
inside the folder.

2. Start the postgresql server
{{<highlight bash>}}
pg_ctl -D psql_data -l logfile -o "-h ''" start
{{</highlight>}}

The options are to write a logfile in case of errors. However,
one can just start it with: `pg_ctl -D psql_data start`. 
And check it is running with: `pg_ctl -D psql_data status`.

3. Create the database using these configuration in this folder:
{{<highlight bash>}}
    export PGHOST="$HOME/psql_data"
	createdb dbname
	psql dbname
{{</highlight>}}

Don't forget to replace the database name `dbname`, and if
this does not work, replace also `$HOME` by what you think the actual
path to `HOME` should be.

4. Exit the database with `\q` and close the server with:
{{<highlight bash>}}
	pg_ctl -D psql_data stop
{{</highlight>}}

## A great tutorial
A great tutorial for Postgres is [here](https://pgexercises.com/)
where they don't use any other SQL language but Postgres. 
I tried other ones where they used old versions of MySQL,
and they were too different, and you cannot rely in the 
documentation because the latest version has improved so much.

## The first few commnands

* Creating Tables [here](https://www.postgresql.org/docs/9.1/sql-createtable.html)
{{<highlight postgresql>}}
CREATE TABLE tablename (type1 arg1 constraint1, type2 arg2 constraint2, etc.);
{{</highlight>}}

A basic list of types is: `varchar` or `varchar(n)`,`int`, `float`, `boolean`, `int[]` although when inserting from python witn psycopg2 one inserts as `'{val1,...}'`. See [here](https://www.guru99.com/postgresql-data-types.html)

* Retrieve data
{{<highlight postgresql>}}
SELECT attribute1, attribute2 FROM tablename WHERE conditions 

SELECT attribute1, count(\*) AS count FROM tablename WHERE conditions GROUP BY attribute1;

SELECT attribute1, SUM(attribute2) AS sum FROM tablename WHERE conditions GROUP BY attribute1;
{{</highlight>}}

Remark: Recall that you can use set notation `attribute in (list as a tupple);`


* Insert data:
{{<highlight postgresql>}}
    INSERT INTO tablename (list of attributes) VALUES (data1 as a tuple), (data2 as a tuple), ...;
{{</highlight>}}
Or
{{<highlight postgresql>}}
INSERT INTO tablename (attributes) SELECT same attributes FROM tablename2 WHERE conditions on table2;
{{</highlight>}}

See [here](https://www.postgresql.org/docs/9.5/sql-insert.html)

* Update attributes:
{{<highlight postgresql>}}
    UPDATE tablename SET attributetochange = newvalue WHERE conditions;
{{</highlight>}}

* Classification:
{{<highlight postgresql>}}
    CASE WHEN condition THEN value WHEN condition2 THEN value2 ELSE othervalue END
{{</highlight>}}

* To delete:
    - to delete some rows:
{{<highlight postgresql>}}
    DELETE FROM tablename WHERE conditions;
{{</highlight>}}

    - To delete all rows:
{{<highlight postgresql>}}
    TRUNCATE tablename
{{</highlight>}}

    - To delete the table and all rows:
{{<highlight postgresql>}}
    DROP TABLE tablename;
{{</highlight>}}

* To retrieve portions of a time stamp

    - To retrieve multiple parts:
{{<highlight postgresql>}}
    TO_CHAR(attribute, 'YYYY-MM')
{{</highlight>}}

    - To retrieve a single part:
{{<highlight postgresql>}}
    DATE_PART('part as a string', attribute)
{{</highlight>}}

where the part could be `'month'` or `'year'`, etc.


* Mix data from several tables. 
{{<highlight postgresql>}}
SELECT tabname.attribute1, tabname.attribute2
FROM tab1 JOIN tab2 ON tab1.attribute = tab2.attribute
WHERE conditions on tab1.attributes and tab2.attributes;
{{</highlight>}}

    - `inner join` is the same as `joint`.
    - `outer join` takes both tables and if an attribute is not in one table shows it as null. In other words, 
this is the `union` of the attributes of both tables. Do not confuse with
`union` of data from both tables. See the command `union`.
    - `left outer join` takes rows from first table and shows them as null if the combined data does not have that propery.

See [here](https://www.postgresql.org/docs/8.3/tutorial-join.html) for a larger explanation.

* With Clause:
{{<highlight postgresql>}}
WITH temporaryTable AS (select statment) SELECT statement using another table and temporaryTable;
{{</highlight>}}

There could be several temporary tables separated by comma, but the last one does not have comma before the final select statement. 

* The With Recursive Clause:
{{<highlight postgresql>}}
WITH RECURSIVE temp_table AS (
SELECT attributes FROM tab1 WHERE conditions
UNION
SELECT tab1.attributes FROM tab1
INNER JOIN temp_table 
ON temp_table.parent_attribute = tab1.child_attribute)
SELECT * FROM temp_table;
{{</highlight>}}

This is a basic example where one can be trying 
to trace ascendants/descendants
in some sense of a single data: recommended by, manager/employee of,
parent/child of, etc.

* Other cool stuff:
    - Strings are appended with ||: `attribute || ', ' || attribute2.`
    - UNION to list rows from two tables with the similar attributes, and usually the same number of attributes.
    - Mathematical operators [here](https://www.postgresql.org/docs/9.1/functions-math.html) including the `width_bucket(attribute, min, max, number_of_buckets)` which splits the range from min inclusive to max exclusive to the number of buckets and tells where attribute falls. 
    - Use `str like 'beginning%'` to check if it begins with a pattern, or `UPPER(str)` to make it all upper case.  
    - For histograms with Postgresql see [here](https://tapoueh.org/blog/2014/02/postgresql-aggregates-and-histograms/)

## Back up
There are several things to know here:

* To dump a copy use:
{{<highlight bash>}}
pg_dump -U username -Z9 dbname > dbfile.pgsql
{{</highlight>}}
where the option `-Z9` is for maximal compression, and `-Z0` is for no
compression at all.

    - And to restore it:
{{<highlight bash>}}
psql dbname < file.pgsql
{{</highlight>}}

    - Or even better:
{{<highlight bash>}}
pg_restore --dbname=dbname
{{</highlight>}}

where there are multiple options to determine 
what you want to restore, including indexes.

 - For example:
    1. `-a` for data only, 
    2. `-e` for exit on error, 
    3. `-f` for filename, 
    4. `-F` for format,
    5. `-I` to restore only certain indices, 
    6. `-j` for the number of jobs.

See also options `-l` and `-L` for list of things to restore, [here](https://www.postgresql.org/docs/9.2/app-pgrestore.html)

* To insert data into a table from a CVS file:
{{<highlight postgresql>}}
COPY tablename(attributeListAsATuple) FROM 'path\cvsfile.ext' DELIMITER ',' NULL '\n' CSV HEADER; 
{{</highlight>}}

See [here](https://www.postgresql.org/docs/9.0/sql-copy.html)
 

	
