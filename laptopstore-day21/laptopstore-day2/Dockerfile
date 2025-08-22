# --- build stage ---
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /src
COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .
RUN chmod +x mvnw && ./mvnw -q -DskipTests dependency:go-offline

COPY . .
RUN ./mvnw -q -DskipTests package

# --- runtime stage ---
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /src/target/*.jar app.jar

EXPOSE 8086
ENTRYPOINT ["java", "-jar", "app.jar"]
