FROM docker.io/library/golang:1.25-alpine AS build

WORKDIR /src
RUN apk --no-cache add git ca-certificates && \
    git clone --depth=1 --branch go-go-gobbonet https://github.com/jmccardle/gobbonet.git . && \
    go mod download && \
    ./stage-web.sh && \
    VERSION="$(tr -d '[:space:]' < VERSION)-go-$(git rev-parse --short HEAD)" && \
    CGO_ENABLED=0 go build -trimpath \
      -ldflags "-s -w -X github.com/jmccardle/gobbonet/internal/version.Version=$VERSION" \
      -o /src/gobbonet ./cmd/gobbonet

FROM scratch
WORKDIR /app
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=build /src/gobbonet .
COPY --from=build /src/web ./web
EXPOSE 9066
VOLUME ["/data"]
ENV GOBBONET_CONFIG=/data/config.toml \
    GOBBONET_DATA_DIR=/data
ENTRYPOINT ["/app/gobbonet"]
