FROM node:16.20-alpine

# 1. 基础工具 + SSH 服务器
RUN apk add --no-cache \
    git \
    curl \
    bash \
    openssh-server \
    && npm i -g \
    pnpm@7 \
    typescript \
    eslint \
    prettier

# 2. SSH 配置（用于 VS Code Remote SSH）
RUN mkdir -p /var/run/sshd \
    && ssh-keygen -A \
    && echo "root:root" | chpasswd \
    && sed -i 's/#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config

WORKDIR /srv/frontend

# 3. 预下载前端依赖（利用缓存，加速本地构建）
COPY frontend/package.json frontend/pnpm-lock.yaml frontend/pnpm-workspace.yaml ./
COPY frontend/apps/remark42/package.json ./apps/remark42/
RUN --mount=type=cache,id=pnpm,target=/root/.pnpm-store/v3 pnpm i
