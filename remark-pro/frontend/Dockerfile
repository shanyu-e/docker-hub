# Dockerfile.prod.base.frontend
# 前端生产编译环境基础镜像
# 由 GitHub Actions 构建，推送到 gitea.mc.dj/chinadengjing/remark42-frontend-pro-base:latest
#
# 包含：
#   - node:16.20-alpine
#   - pnpm@7
#   - 预下载的 npm 依赖
#
# 用途：
#   Dockerfile.prod 使用此镜像，仅复制源码并执行 pnpm build
#   无需再次下载依赖，加速构建

FROM node:16.20-alpine

# 安装 pnpm
RUN npm i -g pnpm@7

# 预下载前端依赖（利用 Docker 缓存）
WORKDIR /srv/frontend/

# 复制依赖声明文件
COPY frontend/package.json frontend/pnpm-lock.yaml frontend/pnpm-workspace.yaml ./
COPY frontend/apps/remark42/package.json ./apps/remark42/

# 安装依赖
RUN --mount=type=cache,id=pnpm,target=/root/.pnpm-store/v3 \
    pnpm i

# 验证环境
RUN pnpm --version && \
    echo "Frontend build environment ready" && \
    echo "Updated: 2026-07-09-3"
