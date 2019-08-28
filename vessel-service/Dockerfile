FROM golang:1.12 as builder
RUN apk add upx && apk add git
RUN makdir /go/vessel-service
WORKDIR /go/vessel-service

ENV GOPROXY="https://athens.azurefd.net"
ENV GO111MODULE=on
RUN go mod download
COPY . .

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -ldflags "-s -w" -o .
FROM alpine:latest

RUN apk --no-cache add ca-certificates

RUN mkdir /app
WORKDIR /app
COPY --from=builder /go/vessel-service/vessel-service .

CMD ["./vessel-service"]


