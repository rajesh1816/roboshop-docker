FROM python:3.9.23-alpine3.22 AS builder
WORKDIR /opt/server
# Install build dependencies
RUN apk add --no-cache python3-dev build-base linux-headers pcre-dev
COPY requirements.txt .
RUN pip3 install --no-cache-dir --prefix=/install -r requirements.txt



# Stage 2: Final image
FROM python:3.9.23-alpine3.22
EXPOSE 8080
WORKDIR /opt/server
# Runtime dependencies only
RUN apk add --no-cache pcre
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
USER roboshop
# Copy installed Python packages from builder
COPY --from=builder /install /usr/local
# Copy application code
COPY --chown=roboshop:roboshop payment.ini .
COPY --chown=roboshop:roboshop *.py .
CMD ["uwsgi", "--ini", "payment.ini"]



# FROM python:3.9 AS builder
# WORKDIR /opt/server
# COPY requirements.txt .

# # Install build dependencies for uwsgi
# RUN apt-get update && apt-get install -y \
#     build-essential \
#     gcc \
#     libpcre2-dev \
#     && rm -rf /var/lib/apt/lists/*

# RUN pip3 install --no-cache-dir -r requirements.txt



# FROM python:3.9-slim
# WORKDIR /opt/server
# RUN apt-get update && apt-get install -y \
#     libpcre2-8-0 \
#     && rm -rf /var/lib/apt/lists/*
# COPY --from=builder /usr/local/lib/python3.9 /usr/local/lib/python3.9 
# COPY --from=builder /usr/local/bin /usr/local/bin
# COPY payment.ini .
# COPY *.py .
# RUN groupadd -r roboshop && useradd -r -g roboshop roboshop \
#     && chown -R roboshop:roboshop /opt/server
# USER roboshop
# EXPOSE 8080
# CMD [ "uwsgi", "--ini", "payment.ini" ]




# FROM python:3.9
# WORKDIR /opt/server
# COPY payment.ini .
# COPY *.py .
# COPY requirements.txt .
# RUN pip3 install -r requirements.txt
# CMD [ "uwsgi", "--ini", "payment.ini" ]