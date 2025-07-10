#
# Build
#
FROM debian:10 AS build

# Pre-fix for Java certificate issue
RUN apt-get update && \
    apt-get install -y ca-certificates-java && \
    /var/lib/dpkg/info/ca-certificates-java.postinst configure && \
    apt-get install -y openjdk-11-jre-headless maven && \
    apt-get clean && \
    update-ca-certificates -f

WORKDIR /opt/shipping

COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package

#
# Run
#
FROM openjdk:8-jdk

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar

CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]
