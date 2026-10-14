# ==========================================
# Multi-stage Dockerfile for Campus Hub Backend
# ==========================================

# Stage 1: Build the application with Gradle and JDK 21
FROM eclipse-temurin:21-jdk-jammy AS builder
WORKDIR /app

# Copy gradle wrapper and build files
COPY gradlew settings.gradle build.gradle ./
COPY gradle ./gradle

# Fix Windows CRLF line endings and grant execution permission
RUN sed -i 's/\r$//' gradlew && chmod +x gradlew

# Copy source code and build jar
COPY src ./src
RUN ./gradlew bootJar -x test --no-daemon

# Stage 2: Runtime image using lightweight JRE 21
FROM eclipse-temurin:21-jre-jammy
WORKDIR /app

# Copy the built jar from builder stage
COPY --from=builder /app/build/libs/*.jar app.jar

# Expose backend port
EXPOSE 8080

# Run Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
