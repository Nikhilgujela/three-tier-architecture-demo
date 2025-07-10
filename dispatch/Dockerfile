FROM golang:latest

WORKDIR /app

COPY *.go ./

RUN go mod init dispatch && go mod tidy

RUN go build -o dispatch

CMD ["./dispatch"]
