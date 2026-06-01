# =========================================================
# 基础镜像：Python 3.13 + 依赖环境（不频繁变动）
# =========================================================
FROM docker.1ms.run/python:3.13-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    DJANGO_SETTINGS_MODULE=config.settings \
    UV_FROZEN=true \
    UV_NO_CACHE=true \
    UV_PYTHON_DOWNLOADS=never

# 系统依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    gcc procps libpq-dev curl python3-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# 安装 uv
RUN pip install --no-cache-dir uv

# ✅ 只复制依赖文件（这一层几乎不会变）
COPY pyproject.toml uv.lock ./

# ✅ 安装依赖（这一层是缓存的核心）
RUN uv sync --frozen --no-dev

ENV VIRTUAL_ENV=/app/.venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# 基础镜像到此结束，不需要复制业务代码