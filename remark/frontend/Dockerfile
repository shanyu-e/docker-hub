FROM node:16.20-alpine

# 1. 基础工具 + SSH 服务器
# 版本号参考 frontend/apps/remark42/package.json
RUN apk add --no-cache \
    git \
    curl \
    bash \
    openssh-server \
    openssh-client \
    && npm i -g \
    pnpm@7.33.6 \
    cross-env@7.0.3 \
    typescript@4.7.4 \
    eslint@8.18.0 \
    prettier@2.7.1

# 2. SSH 配置（用于 VS Code Remote SSH）
RUN mkdir -p /var/run/sshd \
	&& ssh-keygen -A \
	&& echo "root:root" | chpasswd \
	&& sed -i 's/#PermitRootLogin.*/PermitRootLogin yes/' /etc/ssh/sshd_config \
	&& sed -i 's/#PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config \
	&& sed -i -E 's/^[[:space:]]*#?[[:space:]]*AllowTcpForwarding.*/AllowTcpForwarding yes/' /etc/ssh/sshd_config \
	&& sed -i -E '/^[[:space:]]*#?[[:space:]]*AllowTcpForwarding/d' /etc/ssh/sshd_config \
	&& echo "AllowTcpForwarding yes" >> /etc/ssh/sshd_config \
	&& echo "GatewayPorts no" >> /etc/ssh/sshd_config \
	&& echo "PermitRootLogin yes" >> /etc/ssh/sshd_config \
	&& echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

WORKDIR /srv/frontend

# 3. 预下载前端依赖（利用缓存，加速本地构建）
COPY frontend/package.json frontend/pnpm-lock.yaml frontend/pnpm-workspace.yaml ./
COPY frontend/apps/remark42/package.json ./apps/remark42/
RUN --mount=type=cache,id=pnpm,target=/root/.pnpm-store/v3 pnpm i
