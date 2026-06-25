# =========================
# Build Stage
# =========================
FROM maven:3.9.6-eclipse-temurin-11 AS builder

WORKDIR /build

COPY pom.xml .

RUN mvn -B dependency:resolve dependency:resolve-plugins

COPY src ./src

RUN mvn clean package -DskipTests

# =========================
# Runtime Stage
# =========================
FROM eclipse-temurin:11-jre

RUN useradd -m appuser

WORKDIR /app

COPY --from=builder /build/target/*.jar app.jar

RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 8080

ENTRYPOINT ["java","-jar","app.jar"]