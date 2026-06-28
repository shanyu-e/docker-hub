FROM node:16.20-alpine

# 1. 基础工具
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

# 2. 预下载前端依赖（利用缓存，加速本地构建）
COPY ./remark/frontend/package.json ./remark/frontend/pnpm-lock.yaml remark/frontend/pnpm-workspace.yaml ./
COPY ./remark/frontend/apps/remark42/package.json ./apps/remark42/
RUN --mount=type=cache,id=pnpm,target=/root/.pnpm-store/v3 pnpm i
