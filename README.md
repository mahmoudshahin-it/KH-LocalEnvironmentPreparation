# PostgresPreparation - Running locally


## Assumptions 
1- Docker desktop is installed // you have docker in your local machine.

2- Run docker image to have the postgres container up and running using the below command:
Command: docker run --name $postgrescontainer -e POSTGRES_USER=$myusername -e POSTGRES_PASSWORD=$mypassword -p 5432:5432 -d postgres
Example: docker run --name postgrescontainer -e POSTGRES_USER=myusername -e POSTGRES_PASSWORD=mypassword -p 5432:5432 -d postgres
Limitation: No persistent volume is used right now. To be used and updated shortly

3- Run the following command to get inside the PSQL container:
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


4- to check the postgres container is running, run the following command:
docker ps



----
## Example

Now let us create a very minimal simple use case for a small company. It has the following key tables:
Departments (id, name).
Employees (id, firstname, lastname, birthdate, mobile, email, salary, departmentid).
Skills (id, name) // lookup table for list of predefined skills.
EmployeeSkills (employeeid, skillid) // many to many relation between employee and skills.
