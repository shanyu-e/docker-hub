# Webhook

自定义的 webhook 镜像，基于 almir/webhook，包含常用工具。

## 包含工具

- curl
- wget
- bash
- jq
- docker-cli
- docker-cli-compose
- git
- openssh-client
- bind-tools (dig, nslookup 等)

## 使用方法

### 基本用法

```bash
docker run -d \
  --name webhook \
  -p 9000:9000 \
  -v ./hooks.json:/etc/webhook/hooks.json \
  -v ./script.sh:/etc/webhook/script.sh \
  -v /var/run/docker.sock:/var/run/docker.sock \
  chinadengjing/webhook:latest
```

### Docker Compose

```yaml
services:
  webhook:
    image: chinadengjing/webhook:latest
    container_name: webhook
    restart: unless-stopped
    ports:
      - "9000:9000"
    volumes:
      - ./hooks.json:/etc/webhook/hooks.json
      - ./script.sh:/etc/webhook/script.sh
      - /var/run/docker.sock:/var/run/docker.sock
    command: ["-hooks", "/etc/webhook/hooks.json", "-verbose"]
```

### hooks.json 示例

```json
[{
  "id": "example-hook",
  "execute-command": "/etc/webhook/script.sh",
  "command-working-directory": "/etc/webhook",
  "trigger-rule": {
    "match": {
      "type": "value",
      "value": "your-secret-here",
      "parameter": { "source": "header", "name": "X-Webhook-Secret" }
    }
  },
  "pass-arguments-to-command": [
    { "source": "payload", "name": "param1" },
    { "source": "payload", "name": "param2" }
  ]
}]
```

### 测试

```bash
curl -X POST "http://localhost:9000/hooks/example-hook" \
  -H "Content-Type: application/json" \
  -H "X-Webhook-Secret: your-secret-here" \
  -d '{"param1":"value1","param2":"value2"}'
```

## 构建

```bash
docker build -t chinadengjing/webhook:latest .
docker push chinadengjing/webhook:latest
```
