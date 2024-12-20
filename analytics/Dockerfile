FROM python:3.10

WORKDIR /usr/src/app

COPY . .

RUN apt update -y && \
    pip install --trusted-host pypi.python.org -r requirements.txt

ENV DB_USERNAME=myuser
ENV DB_PASSWORD=mypassword
ENV DB_PORT=5433
ENV DB_NAME=mydatabase

COPY . .

EXPOSE 5153

CMD [ "python", "./app.py" ]