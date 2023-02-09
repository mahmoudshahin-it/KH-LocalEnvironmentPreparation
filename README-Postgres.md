# Postgres Preparation - Running locally


## Assumptions  
1- Docker desktop is installed // you have docker in your local machine.  
To verify, run the following:  
docker version  
docker-compose version  

## Steps  
1- Run docker image to have the postgres container up and running using the below command:  
Command: docker run --name $postgrescontainer -e POSTGRES_USER=$myusername -e POSTGRES_PASSWORD=$mypassword -p 5432:5432 -d postgres  
Example: docker run --name postgrescontainer -e POSTGRES_USER=myusername -e POSTGRES_PASSWORD=mypassword -p 5432:5432 -d postgres  
Limitation: No persistent volume is used right now. To be used and updated shortly  

2- Run the following command to get inside the PSQL container:  
Command: docker exec -it $postgrescontainer psql -U $myusername  
Example: docker exec -it postgrescontainer psql -U myusername  

Note: all $vars shall be replaced with your preferred values.  



After being inside the container, the following commands can be executed:
\l : list all DBs. 
\d : connect to DB.
\c : swtich to anothr DB.
\dt: list database tables.
\d : describe a table.
\dn : list all schemas.
\du : list all users.

Also, creating DBs, tables, .. etc.


3- to check the postgres container is running, run the following command:
docker ps


4- Run PGAdmin container. this can be using the following command:  
docker run --name pgadmincontainer -p 80:80 \
    -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
    -d dpage/pgadmin4
    
Then we can use the ip of it shown in the docker ps to open the PGAdmin portal using the above email and password. Example: http://localhost:80/

Next is to add the ip of the postgresdb itself. to do that we need two steps:
1. Get the docker container id of the postgrescontainer via the following command:  docker ps | awk '/postgrescontainer/ {print $1}'   
2. Run the following command and find the IPAddress from the output: docker inspect 12a9dd9d42bb.  

Based on that we can see the PGAdmin visualizing our DB tables normally.

----

Another enhancement is to use docker compose and utilize using it. 


https://github.com/mahmoudshahin-it/KH-LocalEnvironmentPreparation/blob/master/docker-compose-postgres.yaml

If used the above docker compose file, you can follow the below steps:
* `docker compose up -d `
* Open your browser and go to pgAdmin URL: http://localhost:5050/  
* Login & add a new server

Add a new server in pgAdmin. 
<img width="406" alt="image" src="https://user-images.githubusercontent.com/112946481/217856052-4315a53d-d591-4656-9993-e13f32360230.png">

Add postgres DB details with hostname = the $PostgresContainerName "postgres-container-demo" & the container port "5432".  
<img width="402" alt="image" src="https://user-images.githubusercontent.com/112946481/217857731-6f8584a0-a623-4555-b572-7e75c6749650.png">

----
## Example

Now let us create a very minimal simple use case for a small company. It has the following key tables:
Departments (id, name).  
Employees (id, firstname, lastname, birthdate, mobile, email, salary, departmentid). // one to many relation between employee and department.  
Skills (id, name) // lookup table for list of predefined skills.  
EmployeeSkills (employeeid, skillid) // many to many relation between employee and skills.  

The goal here is to try different type of DB relations when trying the java springboot application & JPA.
