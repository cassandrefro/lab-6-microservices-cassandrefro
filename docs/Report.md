# Report
## Objective
The purpose of this assignment is to learn how to use Spring Cloud Config and Eureka to deploy an application based on a microservice architecture.

## Config Service
To launch the config service, it currently uses a file located in a different github repository. We will change it and use our own repository you can find on the next [GitHub link](https://github.com/cassandrefro/lab6-microservices-config-repo)

## Two services accounts (2222) and web are running and registered (two terminals)
We will launch the services using 
```./gradlew discovery:bootRun ``` and ``` ./gradlew config:bootRun``` 

Then we can launch the accounts service and the web service using ```./gradlew accounts:bootRun``` and ``` ./gradlew web:bootRun```

### Accounts Service launched on port 2222
![Accounts Service launched on port 2222](accounts-service-2222.png)
### Web Service launched on port 4444
![Web Service launched on port 4444](web-service-4444.png)

We can check on eureka if the services are launched and registered.
## The service registration service has these two services registered
![Eureka Dashboard with 2 registered services](eureka-dashboard-2-services.png)


## Update the configuration repository so that the accounts service uses now the port 3333.
The next step is to update the remote configuration repository so the accounts service launches on a different port. We will change the port from 2222 to 3333. [Link to the commit](https://github.com/cassandrefro/lab6-microservices-config-repo/commit/f2e511a89595b83ada003ba21609d5cd003fa249)

## A second instance of the accounts service is run using the new configuration
Let's run a second instance of the accounts service using the new configuration on another terminal. We will use the same command as before 
```
./gradlew accounts:bootRun
```
Both of the instances of the accounts service are running simultaneously and both are registered in Eureka.

![Eureka Dashboard with 2 registered accounts services](eureka-dashboard-2-accounts-services.png)

## Kill Accounts Service at port 2222 and do a request to Web Service
When we kill the first accounts service at port 2222, Eureka will detect that that service has failed and will delete it from the list of account services.

Because Eureka Dashboard stated "EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE.", I couldn't make the screenshot to capture that the first accounts service was deleted from the list of services.

### Can the web service provide information about the accounts again?
Yes. Eureka updated their list and when the web service makes a request to the accounts service, it will be redirected to the second accounts service at port 3333. So web service can still do request to the accounts service because the second accounts service is still running.