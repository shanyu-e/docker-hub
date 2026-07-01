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
    linux-headers \
    openssh-server

# 2. SSH 配置（用于 VS Code Remote SSH）
RUN mkdir -p /var/run/sshd \
    && ssh-keygen -A \
    && echo "root:root" | chpasswd \
    && sed -i 's/#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config

# 3. golangci-lint
RUN curl -sSfL https://golangci-lint.run/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.55.2

# 4. Go 开发工具
RUN go install github.com/go-delve/delve/cmd/dlv@v1.22.1
RUN go install github.com/cosmtrek/air@v1.49.0
RUN go install golang.org/x/tools/cmd/goimports@v0.17.0
RUN go install mvdan.cc/gofumpt@v0.5.0

WORKDIR /build/backend

# 5. 预下载 Go 依赖（利用缓存，加速本地构建）
COPY backend/go.mod backend/go.sum ./
RUN go mod download
