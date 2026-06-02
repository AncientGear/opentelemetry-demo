># Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM golang:1.25.10-alpine3.23 AS builder

WORKDIR /usr/src/app/

COPY ./go.mod go.mod
COPY ./go.sum go.sum

RUN go mod download

COPY ./genproto/oteldemo/ genproto/oteldemo/
COPY ./flags/ flags/
COPY ./main.go main.go

RUN CGO_ENABLED=0 GOOS=linux GO111MODULE=on go build -ldflags "-s -w" -o product-catalog main.go

FROM alpine:3.23

WORKDIR /usr/src/app/

COPY --from=builder product-catalog ./
EXPOSE ${PRODUCT_CATALOG_PORT}
ENTRYPOINT [ "./product-catalog" ]


