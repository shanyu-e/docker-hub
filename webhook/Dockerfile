FROM almir/webhook

# 安装常用工具（修复旧镜像 apk 日志权限问题）
RUN mkdir -p /var/cache/apk /var/log \
    && touch /var/log/apk.log \
    && chmod 666 /var/log/apk.log \
    && apk update \
    && apk add --no-cache \
    curl \
    wget \
    bash \
    jq \
    git \
    openssh-client \
    bind-tools

WORKDIR /etc/webhook

# 暴露端口
EXPOSE 9000

# 默认命令
ENTRYPOINT ["/usr/local/bin/webhook"]
CMD ["-hooks", "/etc/webhook/hooks.json", "-verbose"]
