FROM openjdk:8-jdk-alpine
VOLUME /tmp
#ARG JAR_FILE
ADD target/microservice2.jar application.jar
EXPOSE 9090
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","application.jar"]