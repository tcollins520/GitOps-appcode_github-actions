# multi stage dockerile
FROM maven:3.9.9-eclipse-temurin-21 AS build_image

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

FROM tomcat:10-jdk21

RUN rm -rf /usr/local/tomcat/webapps/*

COPY --from=build_image /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]