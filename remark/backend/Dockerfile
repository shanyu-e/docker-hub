FROM golang:1.21-alpine

# 基础工具
RUN apk add --no-cache \
    git \
    curl \
    bash \
    gcc \
    libc-dev

# Go 开发工具
RUN go install github.com/go-delve/delve/cmd/dlv@latest \
    && go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest \
    && go install github.com/air-verse/air@latest \
    && go install golang.org/x/tools/cmd/goimports@latest \
    && go install mvdan.cc/gofumpt@latest

WORKDIR /build/backend
