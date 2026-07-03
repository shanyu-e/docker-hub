FROM alpine:3.20

# 安装常用工具
RUN apk add --no-cache \
    curl \
    wget \
    bash \
    jq \
    git \
    openssh-client \
    bind-tools

# 安装 webhook（TARGETARCH 由 buildx 多架构构建自动注入，值为 amd64/arm64）
ARG TARGETARCH
RUN curl -sSL "https://github.com/adnanh/webhook/releases/download/2.8.1/webhook-linux-${TARGETARCH}.tar.gz" \
    | tar xz -C /usr/local/bin --strip-components=1

WORKDIR /etc/webhook

# 暴露端口
EXPOSE 9000

# 默认命令
ENTRYPOINT ["/usr/local/bin/webhook"]
CMD ["-hooks", "/etc/webhook/hooks.json", "-verbose"]
