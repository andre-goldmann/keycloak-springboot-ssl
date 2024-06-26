FROM maven:3.8.4-openjdk-17 AS builder

# Set the working directory
WORKDIR /app

# Copy the Maven project file
COPY pom.xml .
COPY spotbugs-security-exclude.xml .
COPY spotbugs-security-include.xml .

# Copy the source code
COPY src ./src
#COPY interface ./interface


# Build the application with Maven
RUN mvn clean install -DskipTests

# Use a base image with OpenJDK
FROM eclipse-temurin:17.0.10_7-jre-alpine

# Set the working directory in the container
WORKDIR /app

# Set JAVA_HOME environment variable
#ENV JAVA_HOME /opt/java/openjdk

COPY certs/certificate.pem /usr/local/share/ca-certificates/

RUN update-ca-certificates

# Import the CA certificate(s) into the Java truststore
RUN keytool -import -noprompt -trustcacerts -alias jdg.digital -file /usr/local/share/ca-certificates/certificate.pem -keystore $JAVA_HOME\lib\security\cacerts -storepass yourpwd

COPY --from=builder /app/target/api-gateway-0.0.1-SNAPSHOT.jar .

# Expose the port that the application runs on
EXPOSE 9080

# Define the command to run the application when the container starts
CMD ["java", "-jar", "-Dspring.profiles.active=prod", "api-gateway-0.0.1-SNAPSHOT.jar"]