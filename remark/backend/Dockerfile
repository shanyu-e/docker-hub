FROM golang:1.21-alpine

# 基础工具
RUN apk add --no-cache \
    git \
    curl \
    bash \
    gcc \
    libc-dev

# golangci-lint 使用官方安装脚本
RUN curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin latest

# Go 开发工具
RUN go install github.com/go-delve/delve/cmd/dlv@latest \
    && go install github.com/air-verse/air@latest \
    && go install golang.org/x/tools/cmd/goimports@latest \
    && go install mvdan.cc/gofumpt@latest

WORKDIR /build/backend
