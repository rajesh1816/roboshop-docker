FROM python:3.9-slim AS builder
WORKDIR /opt/server

# Install build dependencies only (removed in final image)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential gcc libffi-dev \
    && rm -rf /var/lib/apt/lists/*
COPY requirements.txt .
# Install Python dependencies into /install folder
RUN pip3 install --no-cache-dir --prefix=/install -r requirements.txt




FROM python:3.9-slim
WORKDIR /opt/server
# Create roboshop user and group
RUN groupadd -r roboshop && useradd -r -g roboshop roboshop \
    && mkdir -p /opt/server

# Copy installed dependencies from builder stage
COPY --from=builder /install /usr/local
COPY *.py .
COPY payment.ini .
RUN chown -R roboshop:roboshop /opt/server
USER roboshop
EXPOSE 8000

# Run uWSGI in foreground to keep container alive
CMD ["uwsgi", "--ini", "payment.ini", "--master", "--http", "0.0.0.0:80", "--die-on-term"]








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