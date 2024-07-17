
# Setup

## Java
- install Java 21 (e.g. via IntelliJ) openjdk
## Maven
- setup access for remote gitlab maven repository with a `read_api` personal access token
- create file `settings.xml` in `.m2` folder

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
  <servers>
    <server>
      <id>gitlab-maven</id>
      <configuration>
        <httpHeaders>
          <property>
            <name>Private-Token</name>
            <value>TOKEN</value>
          </property>
        </httpHeaders>
      </configuration>
    </server>
  </servers>
</settings>
```

## Mongo DB 

## Atlas
- cloud based MongoDB
- we are using mongo DB Atlas. also for local development (because we can easily use multiple databases and search indexes and so on) (if required or no internet connection you can use local mongoDB Docker container (see docker-compose))
- create free shared MongoDB organisation, project and cluster
- configure network access to allow only your IP
- configure database access user e.g. username `sanker` and password `Test1234`
- adjust `application-local.yml` in `config-service` and configure mongoDB access (get URI from mongoDB Atlas -> Connect -> Drivers -> copy URI)
```yml
spring:  
  data:  
    mongodb:  
      # for pw special characters: .-!_ # port in URI not possible  
      uri: "mongodb+srv://sanker:Test1234!@XXX.mongodb.net"
```

## Docker
- for local development if network issues INSTEAD of Atlas
- run docker with `docker-compose.yml`
- make sure to have the `mongo-init.js` file for access on the `database`database:
```js
// for setting up the access permssions on the database "database"
db.createUser(
	{
		user: "sanker",
		pwd: "Test1234!",
		roles: [
			{
				role: "readWrite",
				db: "database"
			}
		]
	}
);
```
- access DB e.g. via JetBrains or any other tool:
![](attachments/Pasted%20image%2020240717205219.png)

## Docker-Compose
- some services are required for the microservices (e.g. rabbitMQ, zipkin etc)
- start docker stack with `docker-compose up -d rabbitmq zipkin`: (you can skip mongo if you use Atlas)
```yml
version: '3.7'
  
services:
  # mongo:
  #   image: mongo
  #   environment:
  #     - MONGO_INITDB_DATABASE=database
  #     - MONGO_INITDB_ROOT_USERNAME=sanker
  #     - MONGO_INITDB_ROOT_PASSWORD=Test1234!
  #   volumes:
  #     - mongodb:/data/db
  #.    - ./mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro
  #   ports:
  #     - 27017:27017
  rabbitmq:
    image: rabbitmq:3.11.7-management-alpine
    environment:
      - RABBITMQ_DEFAULT_USER=sanker
      - RABBITMQ_DEFAULT_PASS=Test1234!
    ports:
      - 5672:5672
      - 15672:15672
  zipkin:
    image: openzipkin/zipkin
    ports:
      - 9411:9411

# volumes:
  # mongodb:
```

## Adjust local configuration
- adjust local configuration files in `config-service` if required (mongo DB URI must be changed, all others are optional)

## Set env variables
- set `PROFILES=local` on every java microservice
- set (optional) envs for local tools and networking (see README.md in config-service)