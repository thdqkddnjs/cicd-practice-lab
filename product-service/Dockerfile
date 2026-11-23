FROM gradle:8.7-jdk17 AS build

# ↓↓↓ 여기 3줄 추가 ↓↓↓
COPY corp-ca.pem /usr/local/share/ca-certificates/corp-ca.crt
RUN update-ca-certificates && \
    keytool -importcert -noprompt -cacerts -storepass changeit \
      -alias corp-ca -file /usr/local/share/ca-certificates/corp-ca.crt
# ↑↑↑ 여기까지 ↑↑↑

WORKDIR /workspace
COPY . .
RUN gradle bootJar --no-daemon

FROM eclipse-temurin:17-jre
WORKDIR /app
COPY --from=build /workspace/build/libs/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
