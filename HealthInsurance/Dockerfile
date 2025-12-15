# Build stage
FROM maven:3.9-amazoncorretto-17 AS build
WORKDIR /build
COPY . .
RUN mvn clean package -DskipTests

# Run stage
FROM amazoncorretto:17-alpine
WORKDIR /app
COPY --from=build /build/target/*.jar app.jar
EXPOSE 8089
ENTRYPOINT ["java", "-jar", "app.jar"]
