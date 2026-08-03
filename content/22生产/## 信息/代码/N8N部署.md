cd '/volume1/FMn0s001UNAS/## Docker/n8n'
docker compose up -d
http://192.168.1.200:5678/

version: '3.8'

services:
  n8n:
    image: docker.n8n.io/n8nio/n8n:latest
    container_name: n8n
    restart: unless-stopped
    ports:
      - "5678:5678" # 冒号左边是NAS的端口，可以改成别的，比如 15678:5678
    environment:
      # --- 数据库连接设置 ---
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres # 直接使用下面的服务名
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      # !!! 务必修改为一个你自己的强密码 !!!
      - DB_POSTGRESDB_PASSWORD=uBnTpF99qs208828
    
      # --- 时区设置 ---
      - TZ=Asia/Shanghai
      - GENERIC_TIMEZONE=Asia/Shanghai
    
      # --- n8n Webhook的公网访问地址 (可选，但推荐配置) ---
      # 如果你有公网域名，请取消注释并修改。否则n8n会尝试自动检测。
      # - WEBHOOK_URL=https://n8n.yourdomain.com/
    
    volumes:
      # 将n8n的配置文件、加密密钥等挂载到当前目录下的 n8n_data 文件夹
      - ./n8n_data:/home/node/.n8n
    networks:
      - n8n-network
    depends_on:
      postgres:
        condition: service_healthy # 等待postgres健康检查通过后再启动

  postgres:
    image: postgres:15 # 推荐使用具体的版本号，而不是latest
    container_name: n8n_postgres
    restart: unless-stopped
    environment:
      - POSTGRES_DB=n8n
      - POSTGRES_USER=n8n
      # !!! 这里的密码必须和上面n8n服务中的密码完全一致 !!!
      - POSTGRES_PASSWORD=uBnTpF99qs208828
    volumes:
      # 将PostgreSQL的数据持久化到当前目录下的 postgres_data 文件夹
      - ./postgres_data:/var/lib/postgresql/data
    networks:
      - n8n-network
    healthcheck:
      # 健康检查，确保数据库服务真正可用
      test: ["CMD-SHELL", "pg_isready -U n8n -d n8n"]
      interval: 5s
      timeout: 5s
      retries: 10

networks:
  n8n-network:
    driver: bridge
