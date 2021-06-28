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

[BM Frontend](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/bm_frontend/)

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

[BM Eureka Service](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/eureka-service/)

Once cloned, run below command from root to run it locally:

```
mvn clean install
java -jar target/eureka-service-0.0.1-SNAPSHOT.jar
```
It should start locally eureka server on port 8761. It can be changed in application.properties (server.port)

**Gateway Service**

This is a gateway service. It is entry guard to backend microservices world.

[BM Gateway Service](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/gateway-service/)

Once cloned, run below command from root to run it locally:

```
mvn clean install
java -jar target/gateway-service-0.0.1-SNAPSHOT.jar 
```
It should start locally gateway server on port 8765. It can be changed in application.yml (server.port)
It uses eureka service to lookup other services. It's URL can be changed in application.yml (eureka.client.serviceUrl.defaultZone)

**User Service**

[BM User Service](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/user-service/)

This is a user service. It provides endpoints to register user and authenticate registered user.

Once cloned, run below command from root to run it locally:

*prerequisite*

* Service uses eureka service to register itself. It's URL can be changed in application.properties (eureka.client.serviceUrl.defaultZone)

```
mvn clean install
java -jar target/users-service-0.0.1-SNAPSHOT.jar
```

It should start locally user service on port 5100. It can be changed in application.yml (server.port)
To check data : http://localhost:5100/h2-ui

**Stock Service**

[BM Stock Service](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/stock-service/)

This is a stock service. It provides endpoints to search stock, adding stock to watchlist, updating stock and removing stock from watchlist.
It also provided endpoint to query list of watchlist stocks using user id.

Once cloned, run below command from root to run it locally:

*prerequisite*

* Service uses eureka service to register itself. It's URL can be changed in application.properties (eureka.client.serviceUrl.defaultZone)

```
mvn clean install
java -jar target/stocks-service-0.0.1-SNAPSHOT.jar
```
It should start locally user service on port 5000. It can be changed in application.yml (server.port)
To check data : http://localhost:5000/h2-ui

## Application snapshots

1. Signup & Login
![signup_login](https://github.com/harshalmistry/images/blob/main/singup_login.png)
2. Stock Dashboard (watchlist stocks refresh every 1 min, it can be changed in frontend application - refreshRate)
![stock_dashboard](https://github.com/harshalmistry/images/blob/main/stock_dashboard.png)
3. Stock Search
![stock_search](https://github.com/harshalmistry/images/blob/main/stock_search.png)
4. Add to watchlist
![add_watchlist](https://github.com/harshalmistry/images/blob/main/add_watchlist.png)
5. Update Stock
![update_stock](https://github.com/harshalmistry/images/blob/main/update_stock.png)

## Karate Testing

For User and Stock service functional/integration testing is done using karate framework. Please find code for same below.

[BM Karate](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/bmstock_karate_perf/)

Once cloned, run below command from root directory:

*prerequisite*

* User and Stock service should be up and running. Service URLs can be configured in src/test/java/karate-config.js file (stocksUrl, usersUrl)

```
mvn test
```
It should generate test report under target/karate-reports. Click on <feature_name>.html to view report.

Sample report for add stock to watchlist
![karate](https://github.com/harshalmistry/images/blob/main/karate_new.png)

## Gatling Performance Testing

For User and Stock service performance testing is done using karate and gatling framework. Please find code for same below.

[BM Karate](https://bitbucket.org/abhijitingle/ws-harshal-mistry/src/master/bmstock_karate_perf/)

Once cloned, run below command from root directory:

*prerequisite*

* User and Stock service should be up and running. Service URLs can be configured in src/test/java/karate-config.js file (stocksUrl, usersUrl)

```
mvn clean test-compile gatling:test
```

It should generate test report under target/gatling. Click on index.html to view report.

Sample gatling report
![Gatling](https://github.com/harshalmistry/images/blob/main/performance.png)
