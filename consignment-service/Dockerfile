FROM golang:1.12.1-alpine3.9 as build-env
RUN apk add upx && apk add git
RUN mkdir /go/consignment-service
WORKDIR /go/consignment-service

ENV GOPROXY="https://athens.azurefd.net"
ENV GO111MODULE=on
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-s -w" -o .
FROM alpine:latest

RUN apk --no-cache add ca-certificates

RUN mkdir /app
WORKDIR /app
COPY --from=builder /go/consignment-service/consignment-service .

CMD ["./consignment-service"]

