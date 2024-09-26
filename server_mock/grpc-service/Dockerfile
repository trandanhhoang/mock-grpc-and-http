# Use an official base image with protoc installed or install protoc in this stage
FROM namely/protoc-all:1.43_0 as builder
WORKDIR /app
COPY . .
RUN protoc --descriptor_set_out=services.dsc --proto_path=./protos ./protos/*.proto

# Use the official WireMock Docker image
FROM wiremock/wiremock:3.2.0
COPY --from=builder /app/services.dsc /home/wiremock/grpc/services.dsc
COPY --from=builder /app/extensions /var/wiremock/extensions
EXPOSE 8080