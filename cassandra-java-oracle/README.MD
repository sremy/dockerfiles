# Cassandra Docker image with Oracle Java #
## Image building ##

    $ docker build -t cassandra-java-oracle .
or

    $ docker build -t cassandra-java-oracle https://github.com/sremy/dockerfiles/raw/master/cassandra-java-oracle/cassandra-java-oracle.tar.xz

## Containers creation ##

    $ docker run --name mycassandra1 -m3g cassandra-java-oracle
    $ seedip=$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' mycassandra1) && echo $seedip
    $ docker run -d --name mycassandra2 -m3g cassandra-java-oracle -s $seedip
    $ docker run --name mycassandra3 -m3g cassandra-java-oracle -s $seedip

## cqlsh ##
    # docker logs mycassandra2
    # docker exec -it mycassandra3 bash
    # nodetool status
    # cqlsh

    cqlsh> CREATE KEYSPACE univers WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3'}  AND durable_writes = true;
    cqlsh> USE univers;
    cqlsh:univers> CREATE TABLE emp(
       emp_id int PRIMARY KEY,
       emp_name text,
       emp_service text,
       emp_salary varint,
       emp_phone varint
       );

    cqlsh:univers> INSERT INTO emp (emp_id, emp_name, emp_service, emp_phone, emp_salary) VALUES(1,'Joe', 'IT', 0112233445, 50000);

    cqlsh:univers> SELECT * FROM emp;

    cqlsh:univers> INSERT INTO emp (emp_id, emp_name, emp_service, emp_phone, emp_salary) VALUES(3,'Ben', 'HR', 0122334455, 45000);

    cqlsh:univers> SELECT * FROM emp;

    cqlsh:univers> INSERT INTO emp (emp_id, emp_name, emp_service, emp_phone, emp_salary) VALUES(2,'Henri', 'DIR', 0111222333, 200000);
