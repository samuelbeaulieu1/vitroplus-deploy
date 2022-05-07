FROM golang:1.18-alpine

ENV DB_PASS
ENV DB_HOST mariadb
ENV DB_USER root
ENV DB vitroplus
ENV TOKEN_SECRET

RUN apk update && apk add git

WORKDIR /app

COPY app/go.mod app/go.sum ./

RUN go mod download && go mod verify

COPY app/ .

RUN go build -v -o server

EXPOSE 8000

ENTRYPOINT ["/app/server"]