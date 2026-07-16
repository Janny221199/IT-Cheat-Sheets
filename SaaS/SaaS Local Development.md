
# Setup

## Java
- install Java 21 (e.g. via IntelliJ) openjdk
## Maven
- setup gitlab token for maven repo:

| Scopes     | Token-Name                                     | Usage                                    |
| ---------- | ---------------------------------------------- | ---------------------------------------- |
| `read_api` | Maven Dependency Token Local Development macOS | local `settings.xml` in `.m2` on MacBook |

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

## Docker-Compose
- some services are required for the microservices (e.g. rabbitMQ etc)
- start docker stack with `docker-compose up -d rabbitmq`: (you can skip mongo if you use Atlas)
```yml
version: '3.7'
  
TODO: 
```

## Adjust local configuration
- adjust local configuration files in `config-repo` if required (mongo DB URI must be changed, all others are optional)

## Set env variables
- set `PROFILES=local` on every java microservice
- set (optional) envs for local tools and networking (see README.md in config-service)