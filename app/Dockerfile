# BUILD STAGE
# Take a maven docker image as base image (java 11)
FROM maven:3.8.1-jdk-11-slim AS builder

# Create a working directory
WORKDIR /app

# Copy pom.xml files into the working repository
COPY pom.xml .
COPY webapp/pom.xml ./webapp/

# Install dependencies using maven and pom.xml file
RUN mvn dependency:go-offline -B

# Copy application source files /src
COPY webapp/src ./webapp/src

# Build the application
RUN mvn clean package -B

# RUNTIME STAGE
# Take tomcat server docker image (java 11)
FROM tomcat:9-jdk11-openjdk-slim

# Remove default webapps and copy our WAR file
RUN rm -rf /usr/local/tomcat/webapps/*
COPY --from=builder ./app/webapp/target/*.war /usr/local/tomcat/webapps/ROOT.war

# Install curl for healthcheck and remove the installation package
RUN apt-get update && apt-get install curl -y && rm -rf /var/lib/apt/lists/*

# Create a non-root system user to enhance the security
RUN groupadd -r appuser && useradd -r -g appuser appuser

# Give the new user the ownership of the webapps folder
RUN chown -R appuser:appuser /usr/local/tomcat

# Change user 
USER appuser

# Create a port to our docker image
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD [ "curl -f http://localhost:8080/ || exit 1" ]

# Start tomcat
CMD [ "catalina.sh", "run" ]