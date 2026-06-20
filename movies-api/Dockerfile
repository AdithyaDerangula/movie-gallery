FROM eclipse-temurin:25-jdk-noble AS build
RUN apt-get update && apt-get install -y maven
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

FROM eclipse-temurin:25-jre-noble
WORKDIR /app
COPY --from=build /app/target/movies-api-0.0.1-SNAPSHOT.jar app.jar
ENV JAVA_OPTS="-Xms128m -Xmx350m"
EXPOSE 9080
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
