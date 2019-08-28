FROM golang:1.12.1-alpine3.9 as build-env
RUN apk add upx && apk add git
RUN mkdir /user-service
WORKDIR /user-service
RUN go mod init
ENV GOPROXY="https://athens.azurefd.net"
ENV GO111MODULE=on
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-s -w" -o /go/bin/user-service
RUN upx /go/bin/user-service

FROM alpine:latest
WORKDIR /go/bin
RUN apk update && apk add ca-certificates && apk add --no-cache tzdata&& rm -rf /var/cache/apk/*
COPY --from=build-env /go/bin/user-service /go/bin/user-service

CMD ["./user-service"]
