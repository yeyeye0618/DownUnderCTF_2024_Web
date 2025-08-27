# syntax=docker/dockerfile:1
FROM python:3.8-slim-bullseye

# 安裝 cron
RUN apt-get update && apt-get install -y cron && apt-get clean && rm -rf /var/lib/apt/lists/*

# 建立非 root 帳號
RUN useradd -m swilliams

WORKDIR /app

# 複製 requirements 並安裝
COPY requirements.txt ./
RUN pip3 install --no-cache-dir -r requirements.txt gunicorn

# 複製程式碼
COPY ./ ./

# 建立 logs 目錄
RUN mkdir -p logs && chmod 777 logs

# 設定 cron 工作，每 10 分鐘清空 logs 下的所有檔案
RUN echo "*/10 * * * * root find /app/logs -type f -exec truncate -s 0 {} \;" > /etc/cron.d/log-prune
RUN chmod 0644 /etc/cron.d/log-prune
RUN crontab /etc/cron.d/log-prune

# 設定環境變數
ENV FLASK_APP=app
EXPOSE 5000

# 啟動 cron 與 Gunicorn（前景模式）
CMD ["sh", "-c", "cron && su swilliams -c 'gunicorn -b 0.0.0.0:5000 -w 1 app:app'"]
