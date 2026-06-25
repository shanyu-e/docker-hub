FROM golang:1.21-alpine

# 1. 修复基础工具依赖
RUN apk add --no-cache \
    git \
    curl \
    bash \
    gcc \
    libc-dev \
    coreutils \
    build-base \
    linux-headers

# 2. 修复 golangci-lint 安装
# 弃用存在 SBOM 校验 Bug 的 master 分支，改用官方推荐的 URL (指向已修复的 main 分支)
RUN curl -sSfL https://golangci-lint.run/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.55.2

# 3. Go 开发工具
# 注意：delve 在 alpine (musl) 下即使编译成功，运行时调试也可能存在兼容性问题。
RUN go install github.com/go-delve/delve/cmd/dlv@v1.22.1
RUN go install github.com/cosmtrek/air@v1.49.0
RUN go install golang.org/x/tools/cmd/goimports@v0.17.0
RUN go install mvdan.cc/gofumpt@v0.5.0

WORKDIR /build/backend
