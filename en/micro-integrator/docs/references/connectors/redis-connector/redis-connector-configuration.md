# Setting up the Redis Environment 

The Redis connector allows you to access the Redis commands through the WSO2 EI. Redis stands for remote dictionary server. Redis store/server that stores data as key-value pairs and this key-value store can be used as a database.

## Setting up the environment

Before you start configuring the Redis connector, you need WSO2 EI.Download [WSO2 EI](https://wso2.com/integration/micro-integrator/) and extract the zip file to a known location. In this setup guide we refer to that location as <PRODUCT_HOME>.

To configure the Redis connector, download the following client libraries from the given locations and copy to the `<PRODUCT_HOME>/lib` directory.

* [jedis-2.1.0.jar](https://mvnrepository.com/artifact/redis.clients/jedis/2.1.0)

## Setting up the Redis server 

1. Download the [Redis server](http://redis.io/download) and follow the steps given in this page to install this in your local machine.
2. After setting up the **Redis Server**, navigate to the location you installed Redis and execute the **sudo make install** command.
3. Enter **redis-server** command to start the Redis server.
3. In the command line, you can see the Redis **port** and **PID** as shown below.
    
   <img src="../../../../assets/img/connectors/redis-server.png" title="Redis server" width="600" alt="Redis server"/> 
 
4. You can interact with Redis using the built-in client. In the command line, navigate to the location you installed Redis. Enter `redis-cli`.

   <img src="../../../../assets/img/connectors/redis-client.png" title="Redis Client" width="600" alt="Redis Client"/> 
   
## Set up the Stockquote back-end service 

1. Download the [stockquote_service.jar](https://github.com/wso2-docs/WSO2_EI/blob/master/Back-End-Service/stockquote_service.jar).

2. Open a terminal, navigate to the location of the downloaded service, and run it using the following command:

   ```
   java -jar stockquote_service.jar
   ```
3. You will see the following window after starting the back-end server.   

   <img src="../../../../assets/img/connectors/redis-client.png" title="Redis Client" width="600" alt="Redis Client"/>  