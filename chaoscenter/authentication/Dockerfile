# BUILD STAGE
FROM golang:1.22 AS builder

ARG TARGETOS=linux
ARG TARGETARCH

ADD . /auth-server
WORKDIR /auth-server

ENV GOOS=${TARGETOS} \
    GOARCH=${TARGETARCH}

RUN go env

RUN CGO_ENABLED=0 go build -o /output/server -v ./api/

# Packaging stage
# Use RedHat UBI minimal image as base
FROM registry.access.redhat.com/ubi9/ubi-minimal:9.5

LABEL maintainer="LitmusChaos"

ENV APP_DIR="/litmus"

COPY --from=builder /output/server $APP_DIR/
RUN chown 65534:0 $APP_DIR/server && chmod 755 $APP_DIR/server

WORKDIR $APP_DIR
USER 65534

CMD ["./server"]

EXPOSE 3000