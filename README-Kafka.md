# Kafka Preparation - Running locally




## Assumptions 
1- Docker desktop is installed // you have docker in your local machine.  
To verify, run the following:  
```
docker version  

docker-compose version   
```




## Steps
1- donwload related docker compose file and go to its local directory:  
https://github.com/mahmoudshahin-it/KH-LocalEnvironmentPreparation/blob/master/docker-compose-kafka.yml  

2- run the following command:  
```docker-compose -f /Users/mhshahin/docker-compose.yml up -d  ```   
or if you are inside the directory itself ``` docker compose up -d ```   
Note: Please change the directory to where the file resides.  
After running it, you can verify by executing docker ps. 

3- From inside the docker  
Get inside the running container by executing ```docker exec -it $kafka /bin/sh  ```  
put the right value for $kafka (to have the container name)  
Note: All kafka sh files reside inside the container file system where you can create topics normally.  

```kafka-topics --create --replication-factor 1 --partitions 1 --topic $MyTOPIC --bootstrap-server localhost:9092  ```  
```kafka-topics --list --bootstrap-server localhost:9092  ```  

``` kafka-console-producer --bootstrap-server localhost:9092 --topic $MyTOPIC ```

``` kafka-console-consumer --bootstrap-server localhost:9092 --topic $MyTOPIC --from-beginning ```

``` kafka-topics --bootstrap-server localhost:9092 --delete --topic $MyTOPIC ```


One producer to a topic horus-payment-topic.
Three consumers (three terminals)
```
kafka-topics --create --replication-factor 1 --partitions 2 --topic horus-payment-topic --bootstrap-server localhost:9092   
kafka-topics --list --bootstrap-server localhost:9092  
kafka-topics --bootstrap-server localhost:9092 --describe --topic horus-payment-topic   
kafka-console-producer --bootstrap-server localhost:9092 --topic horus-payment-topic
kafka-console-consumer --bootstrap-server localhost:9092 --topic horus-payment-topic
kafka-console-consumer --bootstrap-server localhost:9092 --topic horus-payment-topic
kafka-console-consumer --bootstrap-server localhost:9092 --topic horus-payment-topic
```

Since Confluent Kafka docker image is used, the above commands can be found under: /bin  
In case you use Kafka from different vendor, you will need to adjust a little bit and adapt accordingly.

4- From outside the docker  
Also, from outside, we can run commands to interact with kafka:  

```
docker exec cp-kafka-container-demo \
kafka-topics --bootstrap-server localhost:9092 \
             --create \
             --topic horus-payment-topic  
```             
             
```
docker exec cp-kafka-container-demo \
kafka-topics --bootstrap-server localhost:9092 \
             --list  
             
 ```
 
 ```
 docker exec cp-kafka-container-demo kafka-console-consumer --bootstrap-server localhost:9092 --topic horus-payment-topic --from-beginning
 
 ```
 
 ```
 docker exec -it cp-kafka-container-demo kafka-console-producer --bootstrap-server localhost:9092 --topic horus-payment-topic

 ```
 
```
docker exec cp-kafka-container-demo \
kafka-topics --bootstrap-server localhost-1:9092 \
             --describe \
             --topic horus-payment-topic

```
  
  Note: In this demo, we use   
  https://hub.docker.com/r/confluentinc/cp-zookeeper  
  https://hub.docker.com/r/confluentinc/cp-kafka  
  
## Examples 
