#FROM maven AS builder
FROM maven:3.9.9-eclipse-temurin-21-alpine AS builder
WORKDIR /opt/server
COPY pom.xml .
COPY src /opt/server/src
RUN mvn clean package


FROM eclipse-temurin:21-jre-alpine
WORKDIR /opt/server
COPY --from=builder /opt/server/target/*.jar /opt/server/shipping.jar
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop \
    && chown -R roboshop:roboshop /opt/server
USER roboshop
EXPOSE 8080
CMD [ "java", "-jar", "shipping.jar" ]



# FROM maven
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src /opt/server/src
# RUN mvn clean package
# RUN mv /opt/server/target/shipping-1.0.jar shipping.jar
# ENV CART_ENDPOINT=cart:8080 \
#     DB_HOST=mysql 
# CMD [ "java", "-jar", "shipping.jar" ]
