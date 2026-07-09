# Dockerfile.prod.base.backend
# 后端生产编译环境基础镜像
# 由 GitHub Actions 构建，推送到 gitea.mc.dj/chinadengjing/remark42-backend-pro-base:latest
#
# 包含：
#   - golang:1.21-alpine
#   - gcc, libc-dev (编译依赖)
#   - 预下载的 Go 模块依赖
#
# 用途：
#   Dockerfile.prod 使用此镜像，仅复制源码并执行 go build
#   无需再次下载依赖，加速构建

FROM golang:1.21-alpine

# 安装编译依赖
RUN apk add --no-cache \
    git \
    gcc \
    libc-dev \
    build-base \
    linux-headers

# 预下载 Go 依赖（利用 Docker 缓存）
WORKDIR /build/backend
COPY backend/go.mod backend/go.sum ./
RUN go mod download

# 验证环境
RUN go version && \
    echo "Backend build environment ready" && \
    echo "Updated: 2026-07-09-2"
