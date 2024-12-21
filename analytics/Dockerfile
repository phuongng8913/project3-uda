FROM python:3.10

USER root
ARG DB_USERNAME=$DB_USERNAME
ARG DB_PASSWORD=$DB_PASSWORD

USER root
WORKDIR /src

COPY . .

RUN apt update -y && \
    pip install -r requirements.txt

COPY . .

EXPOSE 5153

CMD python app.py