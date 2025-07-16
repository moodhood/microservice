FROM maven:3.9.10-eclipse-temurin-17 AS builder

WORKDIR /app

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn clean package

FROM openjdk:21-jdk AS runner

WORKDIR /app

COPY --from=builder ./app/target/patient-service-0.0.1-SNAPSHOT.jar ./app.jar

# PostgreSQL environment variables
ENV SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/patient_db
ENV SPRING_DATASOURCE_USERNAME=postgres
ENV SPRING_DATASOURCE_PASSWORD=postgres
ENV SPRING_JPA_HIBERNATE_DDL_AUTO=update
ENV SPRING_SQL_INIT_MODE=always

EXPOSE 4000

ENTRYPOINT ["java", "-jar", "app.jar"]