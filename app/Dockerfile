#------ STAGE BUILD -------#
FROM maven:3.8.1-openjdk-11-slim as BUILD
WORKDIR /app
COPY pom.xml .
COPY webapp/pom.xml ./webapp/
COPY server_backup/pom.xml ./server_backup/



#------ STAGE RUNTIME -----#