FROM python:3.11-slim

WORKDIR /usr/src/app

COPY . .

RUN apt update -y && \
    pip install --no-cache-dir -r requirements.txt

ENV DB_USERNAME=myuser
ENV DB_PASSWORD=${DB_PASSWORD}
ENV DB_PORT=5433
ENV DB_NAME=mydatabase

COPY . .

EXPOSE 5153

CMD [ "python", "./app.py" ]