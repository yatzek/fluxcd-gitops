FROM golang:1.26.2 AS build

WORKDIR /go/src/app
COPY main.go go.mod ./

RUN go mod download
RUN CGO_ENABLED=0 go build -o /go/bin/app

FROM gcr.io/distroless/static-debian13
COPY --from=build /go/bin/app /

EXPOSE 8080

ENTRYPOINT ["/app"]
