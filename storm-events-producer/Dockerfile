FROM golang as build-stage
WORKDIR /app

COPY go.mod /app
COPY go.sum /app
RUN go mod download

COPY main.go /app
COPY kafka/ /app/kafka
COPY StormEvents.csv /app/StormEvents.csv
#RUN curl -L -o StormEvents.csv "https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D"

RUN cd /app && CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o producer

FROM alpine
COPY --from=build-stage /app/producer /
COPY --from=build-stage /app/StormEvents.csv /
RUN ls -lrt
CMD ["/producer"]