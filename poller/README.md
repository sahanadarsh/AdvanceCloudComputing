# poller
## Continuous Integration
### Instance set up
  - Once the jenkins instance is up make sure you have docker permissions by running ```docker run hello-world``` SSHing into the instance
  - If you face any permission issues run ```sudo chmod 666 /var/run/docker.sock```
  - Generate a new ssh key ```ssh-keygen```
### Configure GitHub
  - Add the public key generated in the SSH & GPG keys of Github
  - Add a webhook in the webapp repository with ```Payload URL as https://your_jenkins_subdomain/github-webhook/``` and ```Content type as application/json```
  - Leave the rest of the options as default
### Set up Jenkins Job
  - Navigate to Manage Jenkins > Manage Plugins > Available and search for ```docker```
  - Select ```Cloudbees Docker Build and Publish``` and  ```Docker Pipeline``` plugins and click ```Install without restart```
  - Again Navigate to Manage Jenkins > Manage Credentials > Jenkins > Global Credentials > Add Credentials
  - Select ```Kind as SSH Username with private key``` and ID and username of your choice. For Private Key select```Enter directly``` and ```Add``` Copy the private key generated and click OK
  - Add another credentials with 
    - ```Kind as Username with password``` 
    - ```Username as your_docker_profile_username```
    - ```Password as your_docker_Account_password```
    - ```ID as docker-cred```
  - Navigate to Manage Jenkins > Configure Systems > Global Properties and select ```Environment Variables``` and click add to add ```DOCKER_ID as key``` and ```your_docker_repository as value```
  - From the Jenkins main page Select ```New Item``` and give the Name of your choice and select ```Pipeline``` and click ok
  - Scroll to ```Build Triggers``` and check ```GitHub hook trigger for GITScm polling```
  - Under Pipeline select ```Pipeline Script from SCM```, ```Git as SCM``` and provide the path & credentials to the repository
  - Under ```Branch Specifier as */* for testing and */main for demo```  and click SAVE
## Running the webapp locally
### Kafka
#### Start The Kafka Environment
>**NOTE**: Your local environment must have Java 8+ installed.

Demo Commit

# Go to poller/kafka folder
```shell script
# cd poller/kafka
```
Run the "zk-single-kafka-single.yml" file using following command:
```shell script
$ docker-compose -f zk-single-kafka-single.yml up
```
```shell script
# Go to the kafka folder you downloaded in another terminal
cd kafka_2.13-2.6.0
```
### Sequence of events
- webapp produce messages on watch topic as soon as user creates/updates/deletes watch
- poller will consume messages from watch topic and store in the poller watch database
- poller will poll weather api for every 5 minutes based on zipcode present in the watch object and produce weather message on weather topic

Kafka cli commands
```shell script
# To get all the kafta topics
$ ./kafka-topics.sh --list --bootstrap-server localhost:9092
# To create the kafta topic
$ ./kafka-topics.sh --create --topic watch --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
# To get details of the particular kafta topic
$ ./kafka-topics.sh --describe --topic new1 --bootstrap-server localhost:9092
# To produce messages on the kafta topic
$ ./kafka-console-producer.sh --topic weather --bootstrap-server localhost:9092 
# To consume messages on the kafta topic
$ ./kafka-console-consumer.sh --topic watch --from-beginning --bootstrap-server localhost:9092 --group kafka-sandbox
# To delete the kafta topic
./kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic watch
```
#### Terminate The Kafka Environment
- Stop the "zk-single-kafka-single.yml" with `Ctrl-C`.
- Stop all the Kafka events with `Ctrl-C`.

If you also want to delete any data of your local Kafka environment including any events you have created along the way, run the command:
```shell script
$ rm -rf /tmp/kafka-logs /tmp/zookeeper
```

### Docker
#### Pulling Images
  - ```docker pull your_docker_repository:latest```
  - ```docker pull mysql``` to pull images
#### Creating Containers
  - ```docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=112233 -e MYSQL_DATABASE=csye7125 -e MYSQL_USER=root -d mysql```
  - ```docker run -d -p 8080:8080 --name webapp-container --link mysql-container sahithis/cloud-backend```
#### To check status of containers (Optional)
  - ```docker container logs mysql-container```
  - ```docker container logs webapp-container```
Now you are all set to run your application on Postman/Browser 
#### To stop the containers
  - ```docker stop mysql-container```
  - ```docker stop webapp-container```
