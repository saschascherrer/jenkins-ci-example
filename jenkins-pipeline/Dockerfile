FROM docker.io/library/openjdk:11.0-jre-slim-buster
COPY target/spring-petclinic-*.jar /petclinic-app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/petclinic-app.jar"]
