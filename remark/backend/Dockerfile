FROM golang:1.21-alpine

# 1. 基础工具依赖
RUN apk add --no-cache \
    git \
    curl \
    bash \
    gcc \
    libc-dev \
    coreutils \
    build-base \
    linux-headers

# 2. golangci-lint
RUN curl -sSfL https://golangci-lint.run/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.55.2

# 3. Go 开发工具
RUN go install github.com/go-delve/delve/cmd/dlv@v1.22.1
RUN go install github.com/cosmtrek/air@v1.49.0
RUN go install golang.org/x/tools/cmd/goimports@v0.17.0
RUN go install mvdan.cc/gofumpt@v0.5.0

WORKDIR /build/backend

# 4. 预下载 Go 依赖（利用缓存，加速本地构建）
COPY backend/go.mod backend/go.sum ./
RUN go mod download
