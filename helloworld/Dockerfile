FROM maven:3.6.1-jdk-8 AS build-env
ADD . /opt
WORKDIR /opt
RUN mvn package

FROM harbor.wise-paas.io/library/mc:v1 AS UploadBlob
COPY --from=build-env /opt /opt
RUN mkdir helloword && cp /opt/target/*.jar helloword && cp /opt/manifest.yml helloword
WORKDIR /helloword
RUN zip helloworld-0.0.1.zip ./* && mc config host add minio http://52.175.26.151:9000 wisepaasdevelop nOLlnJbDWMHry+IpvE6cJvbszLgKnrXnQULCNJ6ug1iNHYSjXeRPWxpIX/OZhUmiFvFqBB3h/2jZvO0gbO+3CA== && mc cp helloworld-0.0.1.zip minio/ci-demo


FROM harbor.wise-paas.io/distroless/java:8
COPY --from=build-env /opt/target /opt/target
WORKDIR /opt/target
EXPOSE 8080
CMD ["helloworld-0.0.1.jar"]
