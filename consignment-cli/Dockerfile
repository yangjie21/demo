FROM golang:1.12.1-alpine3.9 as build-env
RUN apk add upx && apk add git
RUN makdir /go/consignment-cli
WORKDIR /go/consignment-cli

ENV GOPROXY="https://athens.azurefd.net"
ENV GO111MODULE=on
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-s -w" -o .
FROM alpine:latest

RUN apk --no-cache add ca-certificates

RUN mkdir /app
WORKDIR /app
COPY consignment.json /app/consignment.json
COPY --from=builder /go/consignment-cli/consignment-cli .

ENTRYPOINT ["./consignment-cli", "consignment.json"]
CMD ["./consignment-cli"]
