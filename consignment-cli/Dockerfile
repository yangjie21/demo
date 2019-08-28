FROM golang:1.12.1-alpine3.9 as build-env
RUN apk add upx && apk add git
RUN mkdir /consignment-cli
WORKDIR /consignment-cli
RUN go mod init
ENV GOPROXY="https://athens.azurefd.net"
ENV GO111MODULE=on
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-s -w" -o /go/bin/consignment-cli
RUN upx /go/bin/consignment-cli

FROM alpine:latest
WORKDIR /go/bin

RUN apk update && apk add ca-certificates && apk add --no-cache tzdata&& rm -rf /var/cache/apk/*
COPY --from=build-env /go/bin/consignment-service /go/bin/consignment-cli
COPY consignment.json /app/consignment.json
ENTRYPOINT ["./consignment-cli", "consignment.json"]

CMD ["./consignment-cli"]


