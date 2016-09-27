FROM java:8
VOLUME /java
ARG finalName
ADD $finalName /discovery-server.jar
ENTRYPOINT ["java","-jar","/discovery-server.jar","--spring.profiles.active=docker"]