FROM python:3.9 AS builder
WORKDIR /opt/server
COPY requirements.txt .

# Install build dependencies for uwsgi
RUN apt-get update && apt-get install -y \
    build-essential \
    gcc \
    libpcre2-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip3 install --no-cache-dir -r requirements.txt



FROM python:3.9-slim
WORKDIR /opt/server
RUN apt-get update && apt-get install -y \
    libpcre2-8-0 \
    && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/lib/python3.9 /usr/local/lib/python3.9 
COPY --from=builder /usr/local/bin /usr/local/bin
COPY payment.ini .
COPY *.py .
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop \
    && chown -R roboshop:roboshop /opt/server
USER roboshop
EXPOSE 8080
CMD [ "uwsgi", "--ini", "payment.ini" ]




# FROM python:3.9
# WORKDIR /opt/server
# COPY payment.ini .
# COPY *.py .
# COPY requirements.txt .
# RUN pip3 install -r requirements.txt
# CMD [ "uwsgi", "--ini", "payment.ini" ]