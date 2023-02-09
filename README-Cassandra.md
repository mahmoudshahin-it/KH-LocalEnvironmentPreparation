# Cassandra Preparation - Running locally


## Assumptions  
1- Docker desktop is installed // you have docker in your local machine.  
To verify, run the following:  
docker version  
docker-compose version  

## Steps  

1- Pull the latest version of Cassandra:  

`docker pull cassandra`

2- Verify that it is pulled successfully:  

`docker images`


3- Run docker image to have a  container that we can deal with:

`docker run -d --name mycassandra -p 9042:9042 cassandra`

4- Check the running containers & verify that the new container is up & running:
`docker ps`


5- Get inside docker & do CQL queries:
Either

5.1
`docker exec -it mycassandra cqlsh`

Or

5.2
`docker exec -it mycassandra bash`
then inside the contsiner run: 
`cqlsh`



6- Since we are inside the CQL, we can run commands like:

6.1 Create a key space:

`create keyspace test_keyspace with replication = {'class': 'SimpleStrategy', 'replication_factor': 1};`

6.2 Use the key space:
`use test_keyspace ;`

6.3
`CREATE TABLE test_keyspace.test_table (id int primary key, name text, city text);`

`describe keyspaces;`


6.4
Select from the table:
`select * from test_keyspace.test_table;`


6.5
`insert into test_keyspace.test_table (id, name, city) values (1,'Ramzy', 'Singapore');  `

`insert into test_keyspace.test_table (id, name, city) values (2,'Rania', 'Tokyo’);  `


6.6 
Check the records:  
`select * from test_keyspace.test_table;  `
----

Another enhancement is to use docker compose and utilize using it.   

----
## Example

