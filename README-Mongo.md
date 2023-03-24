# MongoDB Preparation - Running locally


## Assumptions  
1- Docker desktop is installed // you have docker in your local machine.  
To verify, run the following:  
docker version  
docker-compose version  

## Steps  

1. create docker-compose.yaml with the below content:  
```
version: '3.8'
services:
  mongodb:
    image: mongo:latest # use the latest image.
    container_name: mongodb-horus
    environment: # set required env variables to access mongo
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: password
    ports:
      - 27017:27017
    volumes: # optional to preserve database after container is deleted.
      - ./database-data:/data/db 
 ```

2. Verify that the container of mongodb-horus is available by running the below command:  
`docker ps`



3. Enter inside the docker container using the following command:  
`docker exec -it mongodb-horus bash`
then inside the contsiner run: 
`mongosh --username root --password password`  
Note: username, password and container_name shall be matching with the docker compose YAML content.



4- Since we are inside the Mongo, we can run commands like:

4.1 Create a DB:

`use mentor-horus`

4.2 Create DB table (Collection):  
`db.createCollection("mentors")`  
  Table name/ collection name in our case is mentors.

4.3 Insert a record (Document) to the table (Collection). 
`db.mentors.insertOne({ "fname": "Mahmoud", "lname": "Shahin", "bdate": "19871126", "phone": "01007787781", "email": "mahmoudshahin.it@gmail.com", "skills": [ "java", "c#", "MongoDB", "Cassandra", "Angular"] })`

To verify,  
`db.mentors.find()`


4.4
Insert multiple documents to the mentors collections:  
4.4.1: Objects to be in a variable mentorsobjects:  
``` 
mentorsobjects = [
  {
  "fname": "Katy",
  "lname": "Pau",
  "bdate": "20001101",
  "phone": "01118629777",
  "email": "kpau.1@gmail.com",
  "skills": ["AWS","devops", "Golang", "Kafka"]
  },
  {
  "fname": "AbdelAziz",
  "lname": "Allam",
  "bdate": "19880404",
  "phone": "01000089891",
  "email": "abd.ibrahim.allam@gmail.com",
  "skills": ["Kotlin","Scala","React", "Flutter"]
  },
  {
  "fname": "John",
  "lname": "Samir",
  "bdate": "20001022",
  "phone": "0100010002",
  "email": "j.samir@gmail.com",
  "skills": ["devops","cloud","infra"]
  }
]
```

4.4.2: Insert many:  
`db.mentors.insertMany(mentorsobjects)`


4.5: find/ select from the collection:  
`db.mentors.find()`  
`db.mentors.find().sort({fname:-1})`  
`db.mentors.find().sort({fname:1})`  
`db.mentors.find().sort({fname:1}).limit(2)`  
`db.mentors.find({fname: { $ne: 'Katy'}}, {_id:0}).sort({fname:1})`  
`db.mentors.find({fname: { $in: ['Katy', 'John']}}, {_id:0}).sort({fname:1})`  

4.6 add a new mentor without bdate:
```
db.mentors.insertOne({
    fname: 'Samy',
    lname: 'Najm',
    phone: '0100100200200',
    email: 'snjm.1@gmail.com',
    skills: ['RabbitMQ', 'CouchBase']
   }
 )
```
then select from all documents mentors without BDate available:  
`db.mentors.find({bdate:{$exists: false}})`

then select the total count:
`db.mentors.countDocuments()`
`db.mentors.countDocuments({bdate:{$exists : false}})`

4.7 updating document:
`db.mentors.updateOne({fname:"samy"},{$set: {lname:"Gamal"}})`
The below figure shows the impact of the above updateOne command:  

<img width="859" alt="image" src="https://user-images.githubusercontent.com/112946481/227550886-0425a404-f300-4a6f-8bce-5887caaa61db.png">

  
Also, we can add/remove to/from an array more items (ie: Skills):  

`db.mentors.updateOne({fname: "Samy"}, {$push: {skills: "Redis"}})`  
`db.mentors.updateOne({fname: "Samy"}, {$pull: {skills: "CouchBase"}})`



<img width="937" alt="image" src="https://user-images.githubusercontent.com/112946481/227553374-61d0598d-bc01-4036-aef3-691ea374d59b.png">

4.8 deleteOne  
`db.mentors.deleteOne({fname: "Samy"})`
Expected output is:  
{ acknowledged: true, deletedCount: 1 } // if match condition is true  
{ acknowledged: true, deletedCount: 0 } // if match condition is false  

Also, deleteMany can be used to do the same for many documents.  

----
To add more details later:  
$and: [{}, {}]  
$or: [{}, {}]  
$not:  
$inc  
$rename  
updateMany  
ReplaceOne  
ReplaceMany  


update VS. Replace?  


