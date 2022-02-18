# Ref: https://docs.docker.com/language/python/build-images/
FROM python:3.9-slim-buster

WORKDIR /api

COPY requirements.txt /tmp/requirements.txt

RUN apt-get update
RUN apt install libpq-dev build-essential wget -y

RUN pip3 install -r /tmp/requirements.txt

COPY . /api

EXPOSE 5000

RUN sed -i '/CipherString = DEFAULT/s/^#\?/#/' /etc/ssl/openssl.cnf

RUN chmod +x docker-entrypoint.sh

ENTRYPOINT ["/bin/bash", "docker-entrypoint.sh"]
