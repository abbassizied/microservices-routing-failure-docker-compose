# Stage 1: Build
FROM maven:3.8.6-openjdk-21 AS builder
WORKDIR /app
# Copy POM and download dependencies first (cached layer)
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copy source and build
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM openjdk:21
RUN adduser --system --group appuser
USER appuser
VOLUME /tmp
COPY --from=builder /app/target/*.jar app.jar
ENTRYPOINT ["java", "-Xmx512m", "-jar", "app.jar"]