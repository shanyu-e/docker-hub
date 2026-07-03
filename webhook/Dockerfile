FROM almir/webhook

# 安装常用工具
RUN apk update && apk add --no-cache \
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
