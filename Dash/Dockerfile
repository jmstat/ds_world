FROM python:3.9.7

USER root

WORKDIR /app

COPY requirements.txt /tmp/requirements.txt

RUN pip3 install --upgrade pip

RUN pip3 install -r /tmp/requirements.txt

EXPOSE 5000

CMD flask run -h 0.0.0.0 -p 5000
