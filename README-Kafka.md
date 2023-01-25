# Kafka Preparation - Running locally




## Assumptions 
1- Docker desktop is installed // you have docker in your local machine.  
To verify, run the following:  
docker version  
docker-compose version   




## Steps
1- donwload related docker compose file and go to its local directory:  
https://github.com/mahmoudshahin-it/KH-LocalEnvironmentPreparation/blob/master/docker-compose-kafka.yml  

2- run the following command:  
docker-compose -f /Users/mhshahin/docker-compose.yml up -d  
Note: Please change the directory to where the file resides.  
After running it, you can verify by executing docker ps.  

3- Get inside the running container by executing docker exec -it kafka /bin/sh  
Note: All kafka sh files reside inside the container file system where you can create topics normally.  

```kafka-topics --create --replication-factor 1 --partitions 1 --topic first_topic --bootstrap-server localhost:9092  ```  
```kafka-topics --list --bootstrap-server localhost:9092  ```  

Since I am using Confluent Kafka docker image, the above commands can be found under: /bin  
In case you use Kafka from different vendor, you will need to adjust a little bit and adapt accordingly.

4- Also, from outside, we can run commands to interact with kafka:  

```docker exec mhshahin-kafka-1 \
kafka-topics --bootstrap-server mhshahin-kafka-1:9092 \
             --create \
             --topic quickstart  
```             
             
```docker exec mhshahin-kafka-1 \
kafka-topics --bootstrap-server mhshahin-kafka-1:9092 \
             --list  
             
 ```

## Examples 
