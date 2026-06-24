FROM node:16.20-alpine

# 基础工具
RUN apk add --no-cache \
    git \
    curl \
    bash \
    && npm i -g \
    pnpm@7 \
    typescript \
    eslint \
    prettier

WORKDIR /srv/frontend
