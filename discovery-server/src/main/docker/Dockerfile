FROM java:8
VOLUME /java
ARG finalName
ADD $finalName /discovery-server.jar
ENTRYPOINT ["java","-Djavax.net.ssl.trustStore=/java/keystore/c2s.keystore","-Djavax.net.ssl.trustStorePassword=changeit","-jar","/discovery-server.jar","--spring.profiles.active=ssl,docker"]