# Discovery Server

The Discovery Server *([Eureka from Netflix OSS](https://github.com/Netflix/eureka))* is one of the key tenets of a microservice based architecture. It facilitates the Consent2Share (C2S) microservices to dynamically discover each other and promotes the scalability of the C2S system. It provides the following:

 + Registry of C2S service instances
 + Provides means for C2S service instances to register, de-register and query instances with the registry
 + Registry propagation to other C2S microservice (Eureka client) and Discovery Server (Eureka server cluster) instances


## Build

### Prerequisites

+ [Oracle Java JDK 8 with Java Cryptography Extension (JCE) Unlimited Strength Jurisdiction Policy](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
+ [Docker Engine](https://docs.docker.com/engine/installation/) (for building a Docker image from the project)

### Commands

This is a Maven project and requires [Apache Maven](https://maven.apache.org/) 3.3.3 or greater to build it. It is recommended to use the *Maven Wrapper* scripts provided with this project. *Maven Wrapper* requires an internet connection to download Maven and project dependencies for the very first build.

To build the project, navigate to the folder that contains `pom.xml` file using the terminal/command line.

+ To build a JAR:
    + For Windows, run `mvnw.cmd clean install`
    + For *nix systems, run `mvnw clean install`
+ To build a Docker Image (this will create an image with `bhits/discovery-server:latest` tag):
    + For Windows, run `mvnw.cmd clean package docker:build`
    + For *nix systems, run `mvnw clean package docker:build`

## Run

### Commands

This is a [Spring Boot](https://projects.spring.io/spring-boot/) project and serves the API via an embedded Tomcat instance. Therefore, there is no need for a separate application server to run this service.
+ Run as a JAR file: `java -jar discovery-server-x.x.x-SNAPSHOT.jar <additional program arguments>`
+ Run as a Docker Container: `docker run -d bhits/discovery-server:latest <additional program arguments>`

*NOTE: In order for this API to fully function as a microservice in the C2S application, it is required to setup the dependency microservices and support level infrastructure. Please refer to the [C2S Deployment](https://github.com/bhits/consent2share/releases/download/2.1.0/c2s-deployment-guide.pdf) Guide for instructions to setup the C2S infrastructure.*

## Configure

This API runs with some default configuration that is primarily targeted for development environment. However, [Spring Boot](https://projects.spring.io/spring-boot/) supports several methods to override the default configuration to configure the API for a certain deployment environment.

Please see the [default configuration](discovery-server/src/main/resources/application.yml) for this API as a guidance and override the specific configuration per environment as needed. Also, please refer to [Spring Boot Externalized Configuration](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html) documentation to see how Spring Boot applies the order to load the properties and [Spring Boot Common Properties](http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html) documentation to see the common properties used by Spring Boot.

The [default configuration](discovery-server/src/main/resources/application.yml) provided with this Discovery Server is setup to run as a Standalone Discovery Server. To run as a Discovery Server Cluster in High Availability mode, please refer to the [Spring Cloud documentation](http://projects.spring.io/spring-cloud/docs/1.0.3/spring-cloud.html#_high_availability_zones_and_regions).

### Examples for Overriding a Configuration in Spring Boot

#### Override a Configuration Using Program Arguments While Running as a JAR:

+ `java -jar discovery-server-x.x.x-SNAPSHOT.jar --server.port=80`

#### Override a Configuration Using Program Arguments While Running as a Docker Container:

+ `docker run -d bhits/discovery-server:latest --server.port=80`

+ In a `docker-compose.yml`, this can be provided as:
```yml
version: '2'
services:
...
  discovery-server.c2s.com:
    image: "bhits/discovery-server:latest"
    command: ["--server.port=80"]
...
```
*NOTE: Please note that these additional arguments will be appended to the default `ENTRYPOINT` specified in the `Dockerfile` unless the `ENTRYPOINT` is overridden.*

### Enable SSL

For simplicity in development and testing environments, SSL is **NOT** enabled by default configuration. SSL can easily be enabled following the examples below:

#### Enable SSL While Running as a JAR

+ `java -jar discovery-server-x.x.x-SNAPSHOT.jar --spring.profiles.active=ssl --server.ssl.key-store=/path/to/ssl_keystore.keystore --server.ssl.key-store-password=strongkeystorepassword`

#### Enable SSL While Running as a Docker Container

+ `docker run -d -v "/path/on/dockerhost/ssl_keystore.keystore:/path/to/ssl_keystore.keystore" bhits/discovery-server:latest --spring.profiles.active=ssl --server.ssl.key-store=/path/to/ssl_keystore.keystore --server.ssl.key-store-password=strongkeystorepassword`
+ In a `docker-compose.yml`, this can be provided as:
```yml
version: '2'
services:
...
  discovery-server.c2s.com:
    image: "bhits/discovery-server:latest"
    command: ["--spring.profiles.active=ssl","--server.ssl.key-store=/path/to/ssl_keystore.keystore", "--server.ssl.key-store-password=strongkeystorepassword"]
    volumes:
      - /path/on/dockerhost/ssl_keystore.keystore:/path/to/ssl_keystore.keystore
...
```

*NOTE: As seen in the examples above, `/path/to/ssl_keystore.keystore` is made available to the container via a volume mounted from the Docker host running this container.*

### Override Java CA Certificates Store In Docker Environment

Java has a default CA Certificates Store that allows it to trust well-known certificate authorities. For development and testing purposes, one might want to trust additional self-signed certificates. In order to override the default Java CA Certificates Store in a Docker container, one can mount a custom `cacerts` file over the default one in the Docker image as follows: `docker run -d -v "/path/on/dockerhost/to/custom/cacerts:/etc/ssl/certs/java/cacerts" bhits/discovery-server:latest`

*NOTE: The `cacerts` references regarding volume mapping above are files, not directories.*

[//]: # (## API Documentation)

[//]: # (## Notes)

[//]: # (## Contribute)

## Contact

If you have any questions, comments, or concerns please see [Consent2Share](https://bhits.github.io/consent2share/) project site.

## Report Issues

Please use [GitHub Issues](https://github.com/bhits/discovery-server/issues) page to report issues.

[//]: # (License)
