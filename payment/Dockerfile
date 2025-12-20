FROM python:3.9 AS builder
WORKDIR /opt/server
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt


FROM python:3.9-alpine 
WORKDIR /opt/server
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