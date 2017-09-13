# Short Description
Discovery Server enables the Consent2Share microservices to dynamically discover each other.

# Full Description

# Supported Source Code Tags and Current `Dockerfile` Link

[`0.12.0 (latest)`](https://github.com/bhits/discovery-server/releases/tag/0.12.0), [`0.11.0)`](https://github.com/bhits/discovery-server/releases/tag/0.11.0)

[`Current Dockerfile`](../discovery-server/src/main/docker/Dockerfile)

For more information about this image, the source code, and its history, please see the [GitHub repository](https://github.com/bhits/discovery-server).

# What is Discovery Server?

The Discovery Server ([Eureka from Netflix OSS](https://github.com/Netflix/eureka)) is one of the key tenets of a microservice based architecture. It facilitates the Consent2Share (C2S) microservices to dynamically discover each other and promotes the scalability of the C2S system. It provides the following:
1.Registry of C2S service instances
2.Provides means for C2S service instances to register, de-register and query instances with the registry
3.Registry propagation to other C2S microservice (Eureka client) and Discovery Server (Eureka server cluster) instances

For more information and related downloads for Consent2Share, please visit [Consent2Share](https://bhits.github.io/consent2share/).
# How to use this image

## Start a Discovery Server instance

Be sure to familiarize yourself with the repository's [README.md](https://github.com/bhits/discovery-server) file before starting the instance.

`docker run  --name discovery-server -d bhits/discovery-server:latest <additional program arguments>`

*NOTE: In order for this API to fully function as a microservice in the Consent2Share application, it is required to setup the dependency microservices and the support level infrastructure. Please refer to the Consent2Share Deployment Guide in the corresponding Consent2Share release (see [Consent2Share Releases Page](https://github.com/bhits/consent2share/releases)) for instructions to setup the Consent2Share infrastructure.*


## Configure

This API runs with a [default configuration](https://github.com/bhits/discovery-server/blob/master/discovery-server/src/main/resources/application.yml) that is primarily targeted for the development environment.  The Spring profile `docker` is actived by default when building images. [Spring Boot](https://projects.spring.io/spring-boot/) supports several methods to override the default configuration to configure the API for a certain deployment environment. 

Here is example to override default database password:

`docker run -d bhits/discovery-server:latest --spring.datasource.password=strongpassword`

## Using a custom configuration file

To use custom `application.yml`, mount the file to the docker host and set the environment variable `spring.config.location`.

`docker run -v "/path/on/dockerhost/C2S_PROPS/discovery-server/application.yml:/java/C2S_PROPS/discovery-server/application.yml" -d bhits/discovery-server:tag --spring.config.location="file:/java/C2S_PROPS/discovery-server/"`

## Environment Variables

When you start the Discovery Server image, you can edit the configuration of the Discovery Server instance by passing one or more environment variables on the command line. 

### JAR_FILE

This environment variable is used to setup which jar file will run. you need mount the jar file to the root of container.

`docker run --name discovery-server -e JAR_FILE="discovery-server-latest.jar" -v "/path/on/dockerhost/discovery-server-latest.jar:/discovery-server-latest.jar" -d bhits/discovery-server:latest`

### JAVA_OPTS 

This environment variable is used to setup JVM argument, such as memory configuration.

`docker run --name discovery-server -e "JAVA_OPTS=-Xms512m -Xmx700m -Xss1m" -d bhits/discovery-server:latest`

### DEFAULT_PROGRAM_ARGS 

This environment variable is used to setup an application argument. The default value is "--spring.profiles.active=application-default,docker".

`docker run --name discovery-server -e DEFAULT_PROGRAM_ARGS="--spring.profiles.active=application-default,ssl,docker" -d bhits/discovery-server:latest`

# Supported Docker versions

This image is officially supported on Docker version 1.13.0.

Support for older versions (down to 1.6) is provided on a best-effort basis.

Please see the [Docker installation documentation](https://docs.docker.com/engine/installation/) for details on how to upgrade your Docker daemon.

# License

View [license](https://github.com/bhits/discovery-server/blob/master/LICENSE) information for the software contained in this image.

# User Feedback

## Documentation 

Documentation for this image is stored in the [bhits/discovery-server](https://github.com/bhits/discovery-server) GitHub repository. Be sure to familiarize yourself with the repository's README.md file before attempting a pull request.

## Issues

If you have any problems with or questions about this image, please contact us through a [GitHub issue](https://github.com/bhits/discovery-server/issues).