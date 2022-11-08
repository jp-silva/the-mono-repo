FROM --platform=linux/amd64 gradle:7.5.1-jdk17-alpine AS BUILD
COPY ./ ./
RUN gradle clean bootJar

FROM --platform=linux/amd64 eclipse-temurin:17.0.5_8-jre-alpine
COPY --from=BUILD /home/gradle/build/libs/spring-native-test-*.jar /spring-native-test.jar
CMD ["java", "-jar", "/spring-native-test.jar"]
