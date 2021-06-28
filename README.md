# BlackMagic Stocks Market

Invest today and secure future.

Using this application, one can make easy investment. Create own stocks dashboard. 

## Architecture

This is a highlevel architecutre for application.

![bm_architecture](https://github.com/harshalmistry/images/blob/main/bm_architecture.png)

Bakcend architecture

![bm_architecture_backend](https://github.com/harshalmistry/images/blob/main/bm_stocks_backend.png)

Frontend architecutre

![bm_architecture_frontend](https://github.com/harshalmistry/images/blob/main/bm_stocks_frontend.png)

## Minimum Viable Product

MVP has following functionalities:

1. User Registration
2. User Login
3. Stocks Dashboard
4. Search Stock
5. Stock Detail
6. Add stock to watchlist
7. Stocks watchlist
8. Add custom notes to watchlist stock
9. Remove stock from watchlist


## Code Repositories & local setup

**Frontend**

[BM Frontend](https://github.com/harshalmistry/blackmagic-stocksmarket)

Once cloned, run below command from root folder to run it locally:

```
ng serve --open
```

*important*

Application uses following configuration (can be found under environments properties): 

```
  userServiceBaseUrl: 'http://localhost:8765/bw-users-service/api/users',
  stockServiceBaseUrl: 'http://localhost:8765/bw-stocks-service/api/stocks',
  refreshRate: 60000,
  
```

here **localhost:8765** is locally running spring cloud gateway service url. 
**refreshRate** is interval property, which application uses in order to refresh stocks watchlist data.

**Bakcend**

Backend is devided into multiple microservices.

**Eureka Service Discovery**

This is a service registery and discovery service. Other applications register/deregister and lookup for other services using this service.

[BM Eureka Service](https://github.com/harshalmistry/eureka-service)

Once cloned, run below command from root to run it locally:

```
mvn clean install
java -jar target/eureka-service-0.0.1-SNAPSHOT.jar
```
It should start locally eureka server on port 8761. It can be changed in application.properties (server.port)

**Gateway Service**

This is a gateway service. It is entry guard to backend microservices world.

[BM Gateway Service](https://github.com/harshalmistry/gateway-service)

Once cloned, run below command from root to run it locally:

```
mvn clean install
java -jar target/gateway-service-0.0.1-SNAPSHOT.jar 
```
It should start locally gateway server on port 8765. It can be changed in application.yml (server.port)
It uses eureka service to lookup other services. It's URL can be changed in application.yml (eureka.client.serviceUrl.defaultZone)

[BM User Service](https://github.com/harshalmistry/users-service)

This is a user service. It provides endpoints to register user and authenticate registered user.

Once cloned, run below command from root to run it locally:

*prerequisite*

* Service uses locally running MySQL server. Please create database (usersDB) before. Please update credentials for same.
* Service uses eureka service to register itself. It's URL can be changed in application.properties (eureka.client.serviceUrl.defaultZone)

```
mvn clean install
java -jar target/users-service-0.0.1-SNAPSHOT.jar
```

It should start locally user service on port 5100. It can be changed in application.yml (server.port)

[BM Stock Service](https://github.com/harshalmistry/stocks-service)

This is a stock service. It provides endpoints to search stock, adding stock to watchlist, updating stock and removing stock from watchlist.
It also provided endpoint to query list of watchlist stocks using user id.

Once cloned, run below command from root to run it locally:

*prerequisite*

* Service uses locally running MySQL server. Please create database (stocksDB) before. Please update credentials for same.
* Service uses eureka service to register itself. It's URL can be changed in application.properties (eureka.client.serviceUrl.defaultZone)

```
mvn clean install
java -jar target/stocks-service-0.0.1-SNAPSHOT.jar
```
It should start locally user service on port 5000. It can be changed in application.yml (server.port)


