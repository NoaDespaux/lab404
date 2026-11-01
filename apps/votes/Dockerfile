FROM --platform=$BUILDPLATFORM dhi.io/golang:1.26.10-dev@sha256:7e044fc5a969032610eaadf997f2ebac4639623cc87ba80d47006600e7e39c4f AS build

ARG TARGETOS
ARG TARGETARCH

WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=$TARGETOS GOARCH=$TARGETARCH \
    go build -trimpath -ldflags="-s -w" -o /out/votes .

FROM scratch

COPY --from=build /out/votes /votes

USER 65532:65532
EXPOSE 8081
ENTRYPOINT ["/votes"]
